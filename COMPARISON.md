# Comparison of universal archive managers

erc is a universal CLI archive manager (Etersoft).
Below is a comparison with similar tools.

## Summary table

| Tool | Language | License | Formats | Active | Tarbomb | Repack | Distros |
|------|----------|---------|---------|--------|---------|--------|---------|
| **erc** | Bash | AGPLv3 | backends | yes | yes | yes | ALT Linux |
| **atool** | Perl | GPLv2+ | backends | no (2012) | yes | no | all major |
| **patool** | Python | GPLv3 | 60+ | yes | yes | yes | all major |
| **dtrx** | Python | GPLv3+ | backends | no (2017) | yes | no | Debian/Ubuntu |
| **ouch** | Rust | MIT | 12+ | yes | no | no | Fedora, AUR |
| **unar** | Obj-C | LGPL 2.1 | 40+ | low | no | no | all major |
| **bsdtar** | C | BSD-2 | 30+ | yes | no | no | all major |
| **unp** | Perl | GPLv2 | backends | no | no | no | Debian |
| **PeaZip** | Pascal | LGPLv3 | 200+ | yes | no | yes | own packages |

## Detailed descriptions

### atool (aunpack/apack/als/acat/adiff)

URL: https://www.nongnu.org/atool/

The classic universal archive wrapper. Provides tarbomb protection
(wrapping multiple root files into a subdirectory). Supports all
common formats through external tools. Abandoned since 2012, but
still packaged in all major Linux distributions.

### patool

URL: https://github.com/wummel/patool

Record holder for number of supported formats (60+). Actively
maintained. Supports create/extract/list/test/repack/diff/search.
Used as a backend by erc. Python-based, available in all major
distros.

### dtrx ("Do The Right Extraction")

URL: https://github.com/dtrx-py/dtrx

Extraction-only tool with smart behavior: tarbomb protection,
recursive extraction, interactive conflict resolution. Originally
by Brett Smith (2006), community fork exists. No target directory
option -- only extracts in current directory.

### ouch ("Obvious Unified Compression Helper")

URL: https://github.com/ouch-org/ouch

Modern Rust tool, no external dependencies. Auto-detects action
from arguments (similar to erc). Supports compress/decompress/list.
Active development. Available in Fedora, AUR, Homebrew.

### unar / lsar (The Unarchiver CLI)

URL: https://theunarchiver.com/command-line

Native extraction without external tools, especially good for RAR
and exotic formats (Amiga, StuffIt, etc.). Extraction and listing
only, no archive creation. Written in Objective-C.

### bsdtar (libarchive)

URL: https://libarchive.org/

Replacement for GNU tar with built-in support for zip, 7z, rar,
iso, cpio, xar and more. System tool on FreeBSD and macOS.
Actively maintained. C library with CLI frontend.

### unp

URL: https://packages.debian.org/unp

Simple Perl wrapper for extraction. Debian-specific. Abandoned.

### PeaZip

URL: https://peazip.github.io/

GUI + CLI tool supporting 200+ formats. Written in Free Pascal.
Ships its own backends. Cross-platform (Windows, Linux, macOS).
Not in standard Linux repositories.

## Unique features of erc

- Auto-detection of action (archive argument = extract, directory = pack)
- Switchable backends (patool / 7z) via `--use-patool` / `--use-7z` flags
- AppImage, snap, squashfs support
- Optimized tar repack via pipe (tar.gz -> tar.xz without temp files)
- EEPM-style interface familiar to ALT Linux users
- Extract to directory: `-C` / `--directory` / `--extract-to` / `--destination` / `--outdir`
