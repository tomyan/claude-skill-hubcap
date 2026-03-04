# Hubcap — Claude Code Skill

A [Claude Code](https://docs.anthropic.com/en/docs/claude-code) skill for [Hubcap](https://hubcap.tomyandell.dev), a CLI that controls Chrome via the DevTools Protocol.

With this skill installed, Claude can browse the web, take screenshots, scrape pages, test web UIs, monitor network traffic, check accessibility, and more — all through Hubcap's 110+ composable commands.

## Prerequisites

Install Hubcap:

```bash
brew install tomyan/tap/hubcap
```

Or download a binary from the [latest release](https://github.com/tomyan/hubcap/releases/latest).

## Install

```bash
claude /install-skill https://github.com/tomyan/claude-skill-hubcap
```

## What it does

Once installed, Claude will automatically use Hubcap when you ask it to interact with a browser. For example:

- "Open Hacker News and screenshot the front page"
- "Check if the login form on localhost:3000 is accessible"
- "Scrape the product prices from this page"
- "Test that clicking the submit button shows a success message"

The skill teaches Claude how to launch Chrome, manage tabs, chain commands, and handle screenshots — so you don't have to spell out the CLI usage yourself.

## Links

- [Hubcap documentation](https://hubcap.tomyandell.dev/commands/)
- [Hubcap source](https://github.com/tomyan/hubcap)
