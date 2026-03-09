# ERC - Etersoft Archive Tool

Universal command-line archive manager with a single interface for all archive formats.

## Description

ERC provides a unified interface to work with various archive formats. It uses `patool` as the primary backend, with `7z` as a fallback when patool is not available.

## Installation

### ALT Linux
```bash
apt-get install erc
```

### Dependencies
- **patool** (recommended) - provides support for many formats
- **p7zip** or **7-zip** - fallback backend, also used for 7z format

## Usage

### Commands

| Command | Description |
|---------|-------------|
| `a`, `create`, `pack`, `add` | Create archive or add files |
| `x`, `extract`, `unpack` | Extract files from archive |
| `l`, `list` | List archive contents |
| `t`, `test`, `check` | Test archive integrity |
| `type` | Print archive type |
| `basename` | Print predicted directory name for archive |
| `repack`, `conv` | Convert archive to another format |
| `formats` | List supported formats |
| `b`, `bench`, `benchmark` | Run CPU benchmark |
| `diff` | Compare two archives |
| `search`, `grep` | Search in archive files |

Commands can also be used with a dash prefix: `-a`, `-x`, `-l`, `-t`, `-b`.

### Options

| Option | Description |
|--------|-------------|
| `-h`, `--help` | Show help |
| `-V`, `--version` | Show version |
| `-q`, `--quiet` | Quiet mode |
| `-f`, `--force` | Overwrite existing files |
| `-C DIR`, `--directory DIR` | Extract to specified directory |
| `--here`, `--no-subdir` | Extract as-is without creating a subdirectory |
| `--flat`, `-j`, `--junk-paths` | Extract all files stripping directory structure |
| `--use-patool` | Force patool backend |
| `--use-7z` | Force 7z backend |

Also supported as aliases for `-C`: `--extract-to`, `--destination`, `--outdir`. Long options accept `=` syntax.

### Extraction Behavior

When extracting an archive, erc decides where to place the files:

- **Single file** in archive — extracted directly to the current directory
- **Single directory** in archive — renamed to match the archive basename (e.g. `data.zip` containing `src/` extracts as `data/`)
- **Multiple files or directories** — extracted into a subdirectory named after the archive (e.g. `test.zip` → `test/`)

Use `erc basename archive.tar.gz` to see the predicted directory name.

Modifiers:
- `--here` / `--no-subdir` — extract as-is without creating a subdirectory
- `--flat` / `-j` — extract all files stripping directory structure
- `-C DIR` — extract into the specified directory

### Examples

**Create archive:**
```bash
erc a archive.zip file1.txt file2.txt   # create zip with files
erc dir                                  # pack directory to dir.zip
erc file.txt zip:                        # pack to zip using format specifier
erc a archive.tar.gz directory/         # create tar.gz archive
```

**Extract archive:**
```bash
erc archive.zip                          # extract (implicit command)
erc x archive.tar.gz                     # extract with explicit command
erc -C dir archive.zip                   # extract directly into dir
erc --directory=dir archive.tar.gz       # same with long option
erc --here archive.zip                   # extract without creating subdirectory
erc --flat archive.zip                   # extract all files stripping paths
erc --flat -C dir archive.zip            # strip paths into target directory
unerc archive.7z                         # extract using unerc alias
erc basename archive.tar.gz              # print predicted directory name
```

**List and test:**
```bash
erc l archive.zip                        # list contents
erc t archive.7z                         # test integrity
erc type archive.tar.xz                  # print archive type
```

**Convert formats:**
```bash
erc repack archive.zip archive.7z        # repack zip to 7z
erc archive.zip 7z:                      # repack using format specifier
erc -f repack old.tar.gz new.tar.xz      # force overwrite target
```

## Utilities

### erc
Main archive tool with all commands.

### unerc
Symlink to `erc` that defaults to extract mode.

### ercat
Output contents of compressed files to stdout (like `bzcat`, `zcat`).

```bash
ercat /var/log/messages.1.gz | grep error
ercat file.bz2 file.xz                   # concatenate multiple files
ercat --quiet file.gz                    # suppress extra output
```

Supported formats: gz, bz2, xz, lzma, zst, lz4, Z

## Backends

ERC supports two backends:

1. **patool** (default if installed) - supports many archive formats
2. **7z** (fallback) - uses 7-zip tools (7z, 7za, 7zr, 7zz)

### Backend selection

```bash
erc --use-patool a archive.zip files     # force patool
erc --use-7z x archive.7z                # force 7z
ERC_BACKEND=patool erc a test.zip dir    # via environment variable
ERC_BACKEND=7z erc x test.7z             # via environment variable
```

## Supported Formats

### With patool backend
All formats supported by patool: zip, 7z, tar, rar, cab, arj, cpio, rpm, deb, iso, and many more.

### With 7z backend
zip, 7z, tar, tar.gz, tar.bz2, tar.xz, tar.zst, and other 7-zip supported formats.

### Special formats
- **exe, dll** - extract resources using 7z
- **AppImage** - extract using built-in --appimage-extract
- **squashfs** - extract using 7z

Run `erc formats` for a complete list of supported formats.

## Running Tests

```bash
cd tests
bats *.bats                              # run all tests
bats erc.bats                            # run specific test file
```

## License

GNU Affero General Public License v3.0 (AGPLv3)

## Links

- **GitHub**: https://github.com/Etersoft/erc
- **Wiki**: http://wiki.etersoft.ru/ERC
- **EEPM**: http://wiki.etersoft.ru/EEPM
- **patool**: https://github.com/wummel/patool
- **Issues**: https://github.com/Etersoft/erc/issues
