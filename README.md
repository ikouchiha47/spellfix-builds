# spellfix-builds

Unofficial Android builds of SQLite's [`spellfix1`](https://www.sqlite.org/spellfix1.html)
extension (`ext/misc/spellfix.c`), cross-compiled via the Android NDK for
`arm64-v8a`, `armeabi-v7a`, and `x86_64`.

## Why this exists

`spellfix1` is part of SQLite's own source tree, but SQLite doesn't publish
a prebuilt Android binary for it (unlike e.g. [`sqlite-vec`](https://github.com/asg017/sqlite-vec),
which ships official prebuilt `.so` files). Rather than trust an
unmaintained third-party binary, this repo builds it directly from the
official [`sqlite/sqlite`](https://github.com/sqlite/sqlite) GitHub mirror.

## How it works

- `sync.yml` runs weekly, checks whether `ext/misc/spellfix.c` has changed
  upstream since the last build, and tags a new release if so.
- `build.yml` runs on any `v*` tag (or manually via `workflow_dispatch`),
  fetches `spellfix.c` + `sqlite3ext.h` from the mirror, cross-compiles with
  the NDK for each ABI, and publishes `spellfix1-android-<abi>.tar.gz`
  files as GitHub Release assets.

## Usage

Download the tarball for your ABI from the
[Releases](../../releases) page, extract `libspellfix.so`, and load it via
`androidx.sqlite`'s `BundledSQLiteDriver.addExtension()` (entry point:
`sqlite3_spellfix_init`).

## Trust model

This is a from-source build off SQLite's own official mirror, not a
third-party binary - the CI config here is the full audit trail for what
went into each release.
