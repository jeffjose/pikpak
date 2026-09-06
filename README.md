# pikpak

A CLI tool to submit magnet links to [PikPak](https://mypikpak.com/) cloud storage.

## Requirements

- [uv](https://github.com/astral-sh/uv)

Dependencies (`httpx`, `questionary`) are managed automatically via the inline script metadata.

## Setup

Create a `.env` file in the project directory with your PikPak credentials:

```
PIKPAK_USERNAME=you@example.com
PIKPAK_PASSWORD=yourpassword
```

Alternatively, set `PIKPAK_USERNAME` and `PIKPAK_PASSWORD` as environment variables (useful for CI).

Tokens are cached at `~/.cache/pikpak/tokens.json` and refreshed automatically.

## Usage

```
./pikpak login                 # verify credentials
./pikpak ls [path]             # list files/folders
./pikpak <folder>              # submit magnet links to a folder
./pikpak fix <folder>          # repair the names PikPak gave those downloads
```

### Flags

| Flag | Description |
|------|-------------|
| `-f`, `--file <path>` | Read magnets from a file instead of stdin |
| `--url <url1> [url2] ...` | Fetch magnets from URL(s) |
| `-a`, `--all` | Select all found magnets (skip interactive picker) |
| `-y`, `--yes` | Auto-confirm prompts (e.g. folder creation) |

### `fix`

PikPak names a download after whatever the torrent called itself. That is
sometimes missing its extension (`Dark.Matter.2024.S02E02...H265-TBK` with no
`.mkv`, which Plex ignores) and sometimes just the show (`Conan` for a 2.8 GB
episode). The magnet's own `dn=` is reliably the release name, so `fix` renames
against it, collapsing any spaces to dots.

Feed it the same magnets you submitted:

```
echo "$MAGNETS" | ./pikpak fix dropbox/TV.Shows --wait 180
```

Only entries matching a magnet you passed are touched. **Run it before anything
syncs the folder** — renaming a file that has already been copied elsewhere does
not move the copy, it just strands the old name and re-fetches the whole file
under the new one.

A multi-file torrent arrives as a folder. The folder's own name is left alone —
Plex reads the file, not the directory, and renaming a directory re-copies
everything under it — so `fix` reaches one level in and renames the video
itself. Season packs (more than one video in the folder) are skipped, since each
episode needs a different name and the release name fits none of them.

| Flag | Description |
|------|-------------|
| `-f`, `--file <path>` | Read magnets from a file instead of stdin |
| `-n`, `--dry-run` | Print the renames without making them |
| `--wait <seconds>` | Wait for freshly submitted downloads to register first |
| `-A`, `--all` | Also repair missing extensions on entries with no matching magnet |

### Examples

```
./pikpak ls
./pikpak ls dropbox
./pikpak dropbox/TV.Shows
./pikpak dropbox/Movies -f magnets.html
./pikpak dropbox/TV.Shows --url https://example.com/torrents -a -y
```

### Headless / CI

For fully non-interactive use (e.g. GitHub Actions), combine `-a -y` with `--url` or `-f`:

```
./pikpak dropbox/TV.Shows --url https://example.com/page -a -y
```
