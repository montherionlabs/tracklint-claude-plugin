# Tracklint plugin for Claude

This repository holds the Tracklint plugin for Claude (`plugins/tracklint`): two skills for writing and checking
tracking plans and for reading GA4, Google Ads conversion, Meta Pixel and TikTok Pixel requests.

## Editions

- **`directory` branch: the skills-only edition prepared for submission to the Claude directory.** It starts no server or program. See [plugins/tracklint/README.md](plugins/tracklint/README.md) for its features, scope and privacy policy.
- **`main` branch: a separate edition for installation from this repository in Claude Code.** It includes skills and a local Tracklint MCP server. It is not part of the directory submission and is not required to use the skills-only edition. Its README documents its requirements and data handling.

## About Tracklint

Tracklint is a DevTools extension for Chrome and Edge that checks GA4, Google Ads, Meta Pixel and TikTok Pixel
tracking against a plan: https://tracklint.netlify.app

Google Analytics and Google Ads are trademarks of Google LLC. Meta is a trademark of Meta Platforms, Inc. TikTok and
Claude are trademarks of their respective owners. Tracklint is not affiliated with, endorsed or sponsored by Google,
Meta, TikTok or Anthropic.
