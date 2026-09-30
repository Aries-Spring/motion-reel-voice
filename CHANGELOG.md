# Changelog

## 1.0.0 - 2026-09-30

First release, split out of Motion Reel 1.1.0 so the core plugin needs no credentials.

- Local MCP server `mcp/elevenlabs.mjs` with `quota`, `voices`, `tts` (cached takes + forced
  alignment) and `transcribe`.
- ElevenLabs API key as a sensitive plugin option.
- Depends on `motion-reel`, whose skill drives these tools.
