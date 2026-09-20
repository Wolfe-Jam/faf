# This is not the live FAFb spec

**This file is a retired pointer.** It used to describe FAFb **v1** (numeric section types, `version_major = 1`). That layout is pre-release history. It is not the format.

**Live specification (wire v2):**  
[github.com/Wolfe-Jam/faf-rust/blob/main/crates/faf-fafb/BINARY-FORMAT.md](https://github.com/Wolfe-Jam/faf-rust/blob/main/crates/faf-fafb/BINARY-FORMAT.md)

**Human page:** [faf.one/spec](https://faf.one/spec)

The `.faf` YAML format is IANA-registered as `application/vnd.faf+yaml`.

v1 files are rejected by a v2 reader. The `.faf` source is always authoritative: **recompile, never migrate.**

The previous v1 body remains in git history on this path. Do not treat it as current.
