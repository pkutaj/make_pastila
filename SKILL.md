---
name: make_pastila
description: Summarize the current Claude Code conversation and push it to pastila as shareable HTML
---

## 1. SUMMARIZE the conversation

- Read the full conversation context you have
- Write a concise markdown summary covering:
  - **What was the goal** — what the user asked for
  - **What was done** — key steps, decisions, findings
  - **What was the outcome** — final state, URLs, artifacts produced
  - **Key snippets** — include important code, queries, or commands (fenced blocks)
- Use bullet points, keep it skimmable
- Max ~2 pages — this is a summary, not a transcript
- Do NOT include any secrets, tokens, cookies, or credentials

## 2. WRITE to temp file

- Write the markdown summary to a temp file:

```bash
tmpfile=$(mktemp /tmp/pastila_XXXXXX.md)
```

- Write the summary content to `$tmpfile`

## 3. CONVERT and UPLOAD

- Run `md2html` with the `-c` flag to convert to HTML and upload to pastila:

```bash
url=$(md2html "$tmpfile" -c)
rm -f "$tmpfile"
```

- The `-c` flag converts markdown to self-contained HTML (with mermaid diagrams rendered as inline SVG) and uploads via `chpst plain` to pastila.clickhouse.com
- The `-p` flag does the same but uploads via `pastila -plain` to pastila.nl

## 4. OUTPUT

- Print the pastila URL to the user
- The URL ends with `.html` and is directly shareable
- Format: `https://pastila.clickhouse.com/<id>.html`
