# Motion Reel Voice

An optional add-on for [Motion Reel](https://github.com/Aries-Spring/motion-reel), the Claude Code
plugin that makes showreel-grade motion videos from code. Motion Reel works on its own and makes
films without a voice; install this add-on when you want a narrator or a talking character. It gives
the Motion Reel skill four ElevenLabs tools: text-to-speech takes in any language ElevenLabs speaks,
forced alignment of each take (so phrases land on the beat and a character's mouth follows the words),
transcription to check that every word survives the music, and a quota check.

## Install

```bash
claude plugin marketplace add Aries-Spring/motion-reel
claude plugin install motion-reel-voice@aries-spring
```

Installing it also installs Motion Reel if you don't have it yet. Then set your ElevenLabs API key:
in Claude Code run `/plugin configure motion-reel-voice@aries-spring` (or open `/plugin` >
**Installed > motion-reel-voice > Configure options**), paste it into **ElevenLabs API key**, then run
`/reload-plugins`. The key is stored in your operating system's credential store.

Then ask Motion Reel for a video with a voice-over, e.g. "a 20 s promo for https://… with a narrator
in Urdu". A free ElevenLabs plan works with its premade voices.

## What it runs, fetches and sends

The plugin is one small local MCP server, `mcp/elevenlabs.mjs` (plain Node, no dependencies), which
Claude Code starts with the plugin and talks to over stdin/stdout.

- `tts` sends the script text to `api.elevenlabs.io` for text-to-speech, then sends the resulting
  audio and text back for forced alignment. It writes the take, its timestamps and the alignment to
  `<film>/audio/vo/` in your project.
- `transcribe` sends audio files you choose (takes or a film's final mix) to ElevenLabs' speech-to-text
  and returns the transcript.
- `quota` and `voices` read your plan's remaining characters and the voices your key can use.

The only credential it uses is the **ElevenLabs API key** plugin option. `.claude-plugin/plugin.json`
declares it as `userConfig.elevenlabs_api_key` (`sensitive: true`) and hands it to the server as
`"env": { "MOTION_REEL_ELEVENLABS_KEY": "${user_config.elevenlabs_api_key}" }`. That variable is the only
one the server reads, and `https://api.elevenlabs.io/v1` is the only host it calls, so your ElevenLabs
key goes only to ElevenLabs' own API, in the `xi-api-key` header. It never reads keys from your
environment or files and never writes the key to disk. With no key set, the tools make no network
calls. There is no telemetry and there are no hooks.

## License

MIT, see [LICENSE](LICENSE).
