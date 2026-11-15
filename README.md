# Tracklint plugin for Claude

This repository holds the Tracklint plugin for Claude (`plugins/tracklint`): two skills for writing and checking
tracking plans and for decoding GA4, Google Ads conversion, Meta Pixel and TikTok Pixel requests, plus the local
Tracklint MCP server they call. The plugin's README describes what it includes, where it works and what it runs, sends and fetches:
[plugins/tracklint/README.md](plugins/tracklint/README.md).

## Install in Claude Code

```
/plugin marketplace add montherionlabs/tracklint-claude-plugin
/plugin install tracklint@montherion
```

Requires Node.js 22 (22.23.4 or later) or 26 (26.9.0 or later).

## About Tracklint

Tracklint is a DevTools extension for Chrome and Edge that checks GA4, Google Ads, Meta Pixel and TikTok Pixel
tracking against a plan: https://tracklint.netlify.app

Google Analytics and Google Ads are trademarks of Google LLC. Meta is a trademark of Meta Platforms, Inc. TikTok and
Claude are trademarks of their respective owners. Tracklint is not affiliated with, endorsed or sponsored by Google,
Meta, TikTok or Anthropic.
