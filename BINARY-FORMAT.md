# FAFB Binary Format Specification

**Version:** 1.0
**Status:** Implementation Complete
**IANA:** Pending registration as `application/vnd.fafb`

---

## Overview

FAFB (FAF Binary) is a compact binary format for representing FAF project context. It provides:

- **220x faster parsing** than YAML text processing
- **Priority-based truncation** for constrained token budgets
- **Pre-computed token estimates** for instant budget calculations
- **Source integrity verification** via CRC32 checksum

---

## File Structure

```
┌─────────────────────────────────────┐
│           HEADER (32 bytes)         │
├─────────────────────────────────────┤
│         SECTION DATA (variable)     │
│                                     │
│  ┌─────────────────────────────┐    │
│  │ Section 1 Data              │    │
│  ├─────────────────────────────┤    │
│  │ Section 2 Data              │    │
│  ├─────────────────────────────┤    │
│  │ ...                         │    │
│  └─────────────────────────────┘    │
├─────────────────────────────────────┤
│     SECTION TABLE (16 bytes/entry)  │
│                                     │
│  ┌─────────────────────────────┐    │
│  │ Entry 1 (16 bytes)          │    │
│  ├─────────────────────────────┤    │
│  │ Entry 2 (16 bytes)          │    │
│  ├─────────────────────────────┤    │
│  │ ...                         │    │
│  └─────────────────────────────┘    │
└─────────────────────────────────────┘
```

---

## Header (32 bytes)

| Offset | Size | Field | Description |
|--------|------|-------|-------------|
| 0 | 4 | `magic` | `0x46414642` ("FAFB" ASCII, little-endian) |
| 4 | 1 | `version_major` | Format major version (currently 1) |
| 5 | 1 | `version_minor` | Format minor version (currently 0) |
| 6 | 2 | `flags` | Feature flags (see below) |
| 8 | 4 | `source_checksum` | CRC32 of original .faf YAML source |
| 12 | 8 | `created_timestamp` | Unix timestamp (seconds since epoch) |
| 20 | 2 | `section_count` | Number of sections (max 256) |
| 22 | 4 | `section_table_offset` | Byte offset to section table |
| 26 | 2 | `reserved` | Reserved for future use (must be 0) |
| 28 | 4 | `total_size` | Total file size in bytes |

### Magic Number

```
Bytes:  0x46 0x41 0x46 0x42
ASCII:  'F'  'A'  'F'  'B'
u32 LE: 0x42464146
```

### Feature Flags

| Bit | Mask | Name | Description |
|-----|------|------|-------------|
| 0 | `0x0001` | COMPRESSED | Content is zstd compressed |
| 1 | `0x0002` | EMBEDDINGS | Contains pre-computed embeddings |
| 2 | `0x0004` | TOKENIZED | Contains token boundaries |
| 3 | `0x0008` | WEIGHTED | Contains attention weights |
| 4 | `0x0010` | MODEL_HINTS | Contains model-specific hints |
| 5 | `0x0020` | SIGNED | Contains cryptographic signature |
| 6-15 | - | Reserved | Must be ignored by readers |

**Compatibility Rule:** Readers MUST ignore unknown flags and continue processing.

---

## Section Entry (16 bytes)

| Offset | Size | Field | Description |
|--------|------|-------|-------------|
| 0 | 1 | `section_type` | Section type identifier |
| 1 | 1 | `priority` | Truncation priority (0-255) |
| 2 | 4 | `offset` | Byte offset to section data |
| 6 | 4 | `length` | Section data length in bytes |
| 10 | 2 | `token_count` | Pre-computed token estimate |
| 12 | 4 | `flags` | Section-specific flags |

---

## Section Types

### Core Sections (0x01-0x0F)

| ID | Name | Default Priority | Description |
|----|------|------------------|-------------|
| `0x01` | META | 255 (Critical) | Project metadata (name, version, score) |
| `0x02` | TECH_STACK | 200 (High) | Languages and frameworks |
| `0x03` | KEY_FILES | 200 (High) | Important file references |
| `0x04` | ARCHITECTURE | 128 (Medium) | System design description |
| `0x05` | COMMANDS | 180 (High) | Build/test/run commands |
| `0x06` | CONTEXT | 64 (Low) | Additional context |
| `0x07` | BISYNC | 32 (Low) | Bi-sync metadata |

### Extended Sections (0x10-0xFE)

| ID | Name | Default Priority | Description |
|----|------|------------------|-------------|
| `0x10` | EMBEDDINGS | 16 (Optional) | Pre-computed embedding vectors |
| `0x11` | TOKEN_MAP | 16 (Optional) | Token boundary markers |
| `0x12` | MODEL_HINTS | 16 (Optional) | Model-specific optimization hints |

### Special

| ID | Name | Description |
|----|------|-------------|
| `0xFF` | CUSTOM | User-defined section |

**Compatibility Rule:** Readers MUST skip unknown section types gracefully.

---

## Priority System

Priority determines truncation order when the context window is constrained. Higher values = more important = truncated last.

| Level | Value | Behavior |
|-------|-------|----------|
| CRITICAL | 255 | Never truncate (project identity) |
| HIGH | 200-254 | Truncate last |
| MEDIUM | 128-199 | Normal truncation |
| LOW | 1-127 | Truncate first |
| OPTIONAL | 0 | Can be omitted entirely |

### Budget Loading Algorithm

```
1. Sort sections by priority (descending)
2. For each section:
   a. If token_count <= remaining_budget:
      - Include section
      - remaining_budget -= token_count
   b. Else if priority == CRITICAL:
      - Include section anyway (identity must survive)
   c. Else:
      - Skip section (graceful degradation)
```

---

## Token Estimation

Token count is pre-computed using:

```
tokens = min(byte_length / 4, 65535)
```

**Rationale:** ~4 bytes per token is the empirical average for English prose in BPE tokenizers.

---

## Integrity

### Source Checksum

The `source_checksum` field contains a CRC32 hash of the original `.faf` YAML source. This enables:

- Verifying the `.fafb` was created from a known source
- Detecting if the source has changed since compilation
- Cache invalidation strategies

### Validation Sequence

```
1. Check magic (fail fast on non-FAFB files)
2. Check version_major compatibility
3. Verify total_size matches actual file size
4. Verify section_table_offset is within bounds
5. For each section entry:
   a. Verify offset + length doesn't overflow
   b. Verify offset + length <= total_size
```

---

## Limits

| Limit | Value | Rationale |
|-------|-------|-----------|
| Max sections | 256 | DoS protection |
| Max file size | 10 MB | DoS protection |
| Max token count | 65,535 | u16 field size |

---

## Byte Order

All multi-byte integers use **little-endian** byte order for cross-platform compatibility (x86/ARM native).

---

## Example

A minimal FAFB file for a project named "example":

```
Header (32 bytes):
  00-03: 46 41 46 42        magic: "FAFB"
  04:    01                 version_major: 1
  05:    00                 version_minor: 0
  06-07: 00 00              flags: none
  08-11: XX XX XX XX        source_checksum: CRC32
  12-19: XX XX XX XX XX XX XX XX  created_timestamp
  20-21: 01 00              section_count: 1
  22-25: 40 00 00 00        section_table_offset: 64
  26-27: 00 00              reserved
  28-31: 50 00 00 00        total_size: 80

Section Data (32 bytes at offset 32):
  "faf_version: 2.5.0\nname: example"

Section Table (16 bytes at offset 64):
  00:    01                 section_type: META
  01:    FF                 priority: 255 (CRITICAL)
  02-05: 20 00 00 00        offset: 32
  06-09: 20 00 00 00        length: 32
  10-11: 08 00              token_count: 8
  12-15: 00 00 00 00        flags: none

Total: 80 bytes
```

---

## Reference Implementation

See `src/binary/` in the xai-faf-rust repository:

| File | Description |
|------|-------------|
| `header.rs` | 32-byte header read/write |
| `section.rs` | Section entries and table |
| `section_type.rs` | Section type identifiers |
| `priority.rs` | Priority levels |
| `flags.rs` | Feature flags |
| `error.rs` | Error types |

**Test Coverage:** 118+ tests in `tests/wjttc_fafb_tests.rs`

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2026-01 | Initial specification |

---

*Generated from xai-faf-rust v1.0.0*
