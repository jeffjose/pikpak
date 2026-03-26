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
```

### Flags

| Flag | Description |
|------|-------------|
| `-f`, `--file <path>` | Read magnets from a file instead of stdin |
| `--url <url1> [url2] ...` | Fetch magnets from URL(s) |
| `-a`, `--all` | Select all found magnets (skip interactive picker) |
| `-y`, `--yes` | Auto-confirm prompts (e.g. folder creation) |

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
