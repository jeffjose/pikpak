# pikpak

A CLI tool to submit magnet links to [PikPak](https://mypikpak.com/) cloud storage.

## Requirements

- [uv](https://github.com/astral-sh/uv)

Dependencies (`httpx`, `questionary`, `keyring`) are managed automatically via the inline script metadata.

## Usage

```
./pikpak login                 # set up and verify credentials
./pikpak ls [path]             # list files/folders
./pikpak <folder>              # submit magnet links to a folder
```

### Examples

```
./pikpak ls
./pikpak ls dropbox
./pikpak dropbox/TV.Shows
./pikpak dropbox/Movies
```

## Setup

On first run, you will be prompted for your PikPak username and password. The password is stored in your system keyring.

Config is saved to `~/.config/pikpak/config.json`. Tokens are cached at `~/.cache/pikpak/tokens.json`.

## Submitting magnets

Run `./pikpak <folder>`, then paste any text containing magnet links and press `Ctrl+D`. The tool parses HTML anchor tags and plain text to extract magnet links with their titles. If multiple links are found, a checkbox prompt lets you choose which to submit. If only one link is found, it is submitted automatically.
