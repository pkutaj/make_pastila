# make_pastila

Claude Code skill that summarizes the current conversation and pushes it to [pastila](https://pastila.clickhouse.com) as shareable HTML.

## What it does

1. Summarizes the current Claude Code conversation as markdown
2. Converts to self-contained HTML (with mermaid diagrams rendered as inline SVG)
3. Uploads to pastila
4. Returns a shareable `.html` URL

## Setup

### 1. Install dependencies

```bash
# pandoc — markdown to HTML
brew install pandoc

# mermaid-cli — renders ```mermaid``` blocks to SVG
npm install -g @mermaid-js/mermaid-cli

# python3 — post-processing (likely already installed)
```

### 2. Install pastila client

You need one of:

| Binary | Uploads to | Auth | Source |
|--------|-----------|------|--------|
| `chpst` | pastila.clickhouse.com | cookie file `~/.pastila.cookie` | Internal ClickHouse tool |
| `pastila` | pastila.nl | none (or encryption key) | https://github.com/nicksherron/pastila |

Place the binary on your `$PATH`.

For `chpst`, get a cookie from https://pastila.clickhouse.com and save it:

```bash
echo 'your-cookie-value' > ~/.pastila.cookie
chmod 600 ~/.pastila.cookie
```

### 3. Install md2html

```bash
cp bin/md2html ~/Tools/md2html   # or anywhere on your $PATH
chmod +x ~/Tools/md2html
```

### 4. Install the Claude Code skill

```bash
mkdir -p ~/.claude/skills/make_pastila
cp SKILL.md ~/.claude/skills/make_pastila/SKILL.md
```

Then register the skill in your Claude Code settings.

## Usage

In a Claude Code conversation:

```
/make_pastila
```

Claude will summarize the conversation, convert to HTML, upload, and return a pastila URL.

### Standalone md2html usage

```bash
# Convert markdown to HTML (stdout)
md2html file.md

# Convert and upload to pastila.clickhouse.com
md2html file.md -c

# Convert and upload to pastila.nl
md2html file.md -p

# From stdin
cat notes.md | md2html -c
```

## Flags

| Flag | Upload target |
|------|---------------|
| (none) | stdout (HTML) |
| `-c` | pastila.clickhouse.com via `chpst` |
| `-p` | pastila.nl via `pastila` |

## No secrets

No credentials are embedded. Auth is handled by:
- `chpst`: reads `~/.pastila.cookie` (user-specific)
- `pastila`: no auth needed for `-plain` uploads
