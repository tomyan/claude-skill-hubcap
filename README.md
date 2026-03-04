# Hubcap — Claude Code Skill

A [Claude Code](https://docs.anthropic.com/en/docs/claude-code) skill for [Hubcap](https://hubcap.tomyandell.dev), a Go CLI that wraps the entire Chrome DevTools Protocol in 118 composable shell commands.

## Why Hubcap?

**Full CDP coverage.** 118 commands spanning navigation, DOM queries, form filling, screenshots, network interception, device emulation, accessibility audits, performance profiling, and more. If you can do it in Chrome DevTools, you can script it with hubcap.

**Designed for agents.** Every decision prioritises AI agent usability: JSON output by default, semantic exit codes (0 success, 1 error, 2 connection failed, 3 timeout), a self-describing interface (`hubcap help <command>`), and stateless architecture that eliminates session management. Commands compose naturally with Unix pipes and shell chaining, keeping token consumption low.

**Context friendly.** The skill keeps a small footprint — just command categories and key patterns. When the agent needs detailed usage for a specific command, it runs `hubcap help <command>` to pull full documentation on demand. No context burned on 118 command docs upfront.

**Single binary, no dependencies.** Just `brew install` and go. No runtimes, no SDKs, no page objects to construct.

## Prerequisites

Install hubcap:

```bash
brew install tomyan/tap/hubcap
```

Or download a binary from the [latest release](https://github.com/tomyan/hubcap/releases/latest).

## Install the skill

Add the marketplace and install:

```bash
/plugin marketplace add tomyan/claude-skills
/plugin install hubcap@tomyan-skills
```

Or install directly:

```bash
git clone https://github.com/tomyan/claude-skill-hubcap.git ~/.claude/plugins/hubcap
```

## What the agent can do

- Navigate to pages and wait for them to load
- Fill out forms and click buttons
- Take screenshots and export PDFs
- Scrape text, links, tables, and structured data
- Extract content without vision (HTML, accessibility trees, DOM snapshots)
- Run accessibility audits
- Monitor network traffic and intercept requests
- Emulate mobile devices and set viewport sizes
- Execute JavaScript and evaluate expressions
- Assert page state for testing
- Capture performance metrics and heap snapshots

## Example

Ask the agent something like:

```
Take a screenshot of https://example.com on an iPhone 12
```

The agent will run:

```bash
hubcap setup launch
hubcap emulate iPhone-12
hubcap goto --wait https://example.com
hubcap screenshot --output example-iphone12.png
```

## Links

- [Introducing Hubcap](https://tomyandell.dev/blog/introducing-hubcap) — blog post on the motivation and design
- [Hubcap documentation](https://hubcap.tomyandell.dev/commands/)
- [Hubcap skill page](https://hubcap.tomyandell.dev/skill/)
- [Hubcap source](https://github.com/tomyan/hubcap)
