---
name: hubcap
description: "Chrome DevTools Protocol CLI. Use when the user wants to control Chrome, automate browsers, scrape web pages, take screenshots, test web UIs, monitor network traffic, check accessibility, or interact with page elements. Hubcap provides 110+ composable commands that connect to Chrome via WebSocket."
allowed-tools: Bash, Read, Write, Edit, Grep, Glob
---

# Hubcap — Chrome DevTools Protocol CLI

Hubcap is a CLI that controls Chrome via DevTools Protocol. Every command connects over WebSocket, performs an action, and prints JSON to stdout. Commands compose with `&&`, pipes, and `jq`.

## Prerequisites

Hubcap must be installed and Chrome must be running with debugging enabled:

```bash
hubcap setup launch   # launches Chrome with debug port open
```

## Getting detailed help

Run `hubcap help <command>` to get full documentation for any command including usage, arguments, flags, output format, error codes, and examples. Always do this before using a command you haven't used before in this session.

Run `hubcap help` (no args) for a categorized list of all commands.

## Command categories

**Navigate & manage tabs**: `goto`, `back`, `forward`, `reload`, `new`, `close`, `tabs`, `version`
**Read page info**: `title`, `url`, `info`, `source`, `meta`, `links`, `images`, `scripts`, `tables`, `forms`, `frames`
**Query DOM**: `query`, `html`, `text`, `attr`, `value`, `count`, `visible`, `exists`, `bounds`, `styles`, `computed`, `layout`, `shadow`, `find`, `selection`, `caret`
**Click & interact**: `click`, `dblclick`, `rightclick`, `tripleclick`, `clickat`, `hover`, `mouse`, `drag`, `fill`, `clear`, `type`, `press`, `focus`, `select`, `check`, `uncheck`, `setvalue`, `upload`, `dispatch`, `tap`, `swipe`, `pinch`
**Scroll**: `scroll`, `scrollto`, `scrolltop`, `scrollbottom`
**Wait**: `wait`, `waittext`, `waitgone`, `waitfn`, `waitload`, `waitnav`, `waiturl`, `waitidle`, `waitrequest`, `waitresponse`
**Capture**: `screenshot`, `pdf`
**Network**: `network`, `har`, `responsebody`, `intercept`, `block`, `throttle`, `console`, `errors`
**Emulate**: `emulate`, `useragent`, `geolocation`, `offline`, `media`, `viewport`, `permission`
**Storage**: `cookies`, `storage`, `session`, `clipboard`
**Analyze**: `a11y`, `coverage`, `csscoverage`, `domsnapshot`, `listeners`, `metrics`, `stylesheets`
**Profile**: `heapsnapshot`, `trace`
**JavaScript**: `eval`, `evalframe`, `run`
**Assert**: `assert` (subcommands: `text`, `title`, `url`, `exists`, `visible`, `count`)
**Utility**: `retry`, `pipe`, `shell`, `record`, `setup`, `help`
**Advanced**: `raw`, `dialog`, `highlight`

## Tab management

By default, all commands target the **first tab**. Use `--target <id>` to target a specific tab.

- `hubcap new <url>` creates a new tab and navigates to the URL in one step. Returns `{"targetId": "..."}`.
- Use `--target <id>` on subsequent commands to operate on that tab.
- When the user says "open a new tab and go to X", use `hubcap new <url>` — do NOT use separate `new` + `goto` (goto would hit the first tab, not the new one).

```bash
# Open a new tab and work in it
TARGET=$(hubcap new https://example.com | jq -r .targetId)
hubcap --target "$TARGET" wait 'h1'
hubcap --target "$TARGET" screenshot --output page.png
```

## Screenshots

- `hubcap screenshot --output file.png` captures the visible viewport
- `hubcap screenshot --output file.png --full` captures the **full scrollable page**
- `hubcap screenshot --output file.png --selector '.element'` captures a specific element

Use `--full` when the user wants to capture the entire page including content below the fold.

## Key patterns

```bash
# Chain commands
hubcap goto --wait https://example.com && hubcap screenshot --output page.png

# Parse JSON output
hubcap tabs | jq '.[].url'

# Wait then act
hubcap wait '.results' && hubcap text '.results'

# Mobile testing
hubcap emulate iPhone-12 && hubcap goto --wait https://example.com

# Retry flaky operations
hubcap retry click '#dynamic-button' --attempts 3 --interval 1s
```

## Exit codes

| Code | Meaning |
|------|---------|
| 0 | Success |
| 1 | Error (bad args, element not found, JS error) |
| 2 | Chrome connection failed |
| 3 | Timeout |

## Global flags

All commands accept: `-port <n>` (default 9222), `-host <s>` (default localhost), `-timeout <d>` (default 10s), `-output <fmt>` (json/ndjson/text), `-quiet`, `-target <id>`.

## Docs

Full documentation: https://hubcap.tomyandell.dev/commands/
