# book-fetch

Search [Anna's Archive](https://annas-archive.li), download ebooks (epub/pdf), and upload them directly to MEGA.

## Features

- Search by title or author
- Auto-picks best result or lets you choose interactively
- Download pipeline: libgen.li mirror → Anna's Archive fast/slow servers
- Upload to MEGA via `megacmd` or `rclone`
- Retry logic for slow/unstable connections

## Requirements

```bash
pip install requests beautifulsoup4 lxml
```

**MEGA upload** (one of):
- [megacmd](https://mega.io/cmd) — official MEGA CLI
- [rclone](https://rclone.org) with MEGA remote configured (`rclone config`)

## Usage

```bash
# Download epub (default)
python3 book_fetch.py "A Wizard of Earthsea Le Guin"

# Download PDF
python3 book_fetch.py "Designing Data-Intensive Applications" --format pdf

# Search only (no download)
python3 book_fetch.py "Karen Horney Self Analysis" --dry-run

# Pick result interactively
python3 book_fetch.py "DDIA Kleppmann" --pick -1

# Pick specific result index
python3 book_fetch.py "DDIA Kleppmann" --pick 2
```

## MEGA Setup

With rclone:
```bash
rclone config
# Add new remote -> type: mega -> enter credentials
rclone ls mega:/  # verify connection
```

Downloaded files land in `MEGA:/Books/` by default. Change `MEGA_FOLDER` in the script to customize.

## Notes

- `annas-archive.li` is used as the default mirror. If it's unreachable, edit `ANNAS_BASE` at the top of the script.
- Newer books (2023+) may only have Anna's Archive servers available (no libgen mirror). Fast download requires an AA membership; slow download works without login but is rate-limited.
- Downloaded files are cached in `/tmp/books/`.
