# 📝 Skill: hermes-tts-configuration

> **Category**: software-development • **Version**: 1.0.0 • **Status**: Available

---

## 📋 Overview
Configure TTS providers, voices, and local engines.

---

## 🎯 Use When
*[Auto-extracted from skill - customize based on actual usage patterns]*

---

## 📖 Full Skill Documentation

# Hermes TTS Configuration

Configure Text-to-Speech for Hermes across all surfaces (CLI, desktop, gateway platforms). Covers built-in providers, local/offline engines, custom command providers, and voice-bubble delivery for Telegram/Matrix/WhatsApp.

## When to Use This Skill

- User wants to change or set up TTS voice/provider
- User wants fully local/offline TTS (no API keys)
- User wants to install a specific voice (anime, character, cloned)
- User needs voice-bubble delivery working on Telegram/etc.
- Troubleshooting TTS: no audio, wrong format, provider errors

## Built-in Providers Quick Reference

| Provider | Local? | Free? | Quality | Voices | Setup |
|----------|--------|-------|---------|--------|-------|
| **Edge TTS** | ❌ (cloud) | ✅ | ★★★★☆ | 100+ MS neural voices | `hermes config set tts.provider edge` |
| **Piper** | ✅ | ✅ | ★★★★★ | 44 langs, dozens of voices | `pip install piper-tts` + config |
| **KittenTTS** | ✅ | ✅ | ★★★☆☆ | 4 built-in (Jasper, etc.) | `pip install kittentts` + config |
| **NeuTTS** | ✅ | ✅ | ★★★★☆ | Voice cloning via ref audio | `pip install neutts` + config |
| **ElevenLabs** | ❌ | Freemium | ★★★★★ | Pro voices, cloning | API key + config |
| **Gemini TTS** | ❌ | Freemium | ★★★★★ | 30 prebuilt, **emotion audio tags**, AI auto-tagging | API key + config |
| **OpenAI/Mistral/xAI** | ❌ | Paid | ★★★★☆ | Good variety | API key + config |

## Local Provider Setup (No API Keys)

### Piper (Recommended — Best Quality/Variety)

```bash
# 1. Install
pip install piper-tts

# 2. Set as provider
hermes config set tts.provider piper

# 3. Choose voice (auto-downloads .onnx + .onnx.json on first use)
hermes config set tts.piper.voice en_US-amy-medium
# Other good options:
#   en_US-lessac-medium (default, balanced)
#   en_US-kathleen-medium (soft female)
#   en_US-ryan-high (male, high quality)
#   ja_JP-... (Japanese voices available)

# 4. Optional: custom voices directory
hermes config set tts.piper.voices_dir ~/.hermes/cache/piper-voices

# 5. IMPORTANT: First use requires manual voice download (Hermes auto-download may fail)
python -m piper.download_voices en_US-amy-medium --download-dir ~/.hermes/cache/piper-voices

# 6. Optional speed adjustment (passes through to provider, not official config key)
hermes config set tts.piper.speed 1.15

# 7. Test
hermes chat -q "Nya~! Piper TTS working, nya~!"
```

**Voice Discovery**: See [Piper VOICES.md](https://github.com/OHF-Voice/piper1-gpl/blob/main/docs/VOICES.md) for full list. Models cache to `~/.hermes/cache/piper-voices/`.

### KittenTTS (Tiny — 25MB Total)

```bash
pip install kittentts
hermes config set tts.provider kittentts
hermes config set tts.kittentts.voice Jasper  # or Amy, etc.
hermes chat -q "Test" --tts
```

Voices: `Jasper` (default), `Amy`, `Brian`, `Emma`. Model: `KittenML/kitten-tts-nano-0.8-int8` (~25MB).

### NeuTTS (Voice Cloning)

```bash
pip install neutts
hermes config set tts.provider neutts

# Configure reference voice (for cloning)
hermes config set tts.neutts.ref_audio ~/.hermes/voice_refs/my_voice.wav
hermes config set tts.neutts.ref_text "Reference transcript matching the audio"
hermes config set tts.neutts.model neuphonic/neutts-air-q4-gguf
hermes config set tts.neutts.device cpu  # or cuda

hermes chat -q "Test" --tts
```

Requires a reference audio file + matching transcript. Bundled default: `tools/neutts_samples/jo.wav` + `jo.txt`.

## Cloud Provider Setup (Free Tiers Available)

### Gemini TTS — **Best for Anime/Character Voices with Emotion**

**Why Gemini TTS stands out:**
- **Free tier** via Google AI Studio (generous limits)
- **30 prebuilt voices** including anime-style: `Kore` (cute female), `Puck` (energetic), `Zephyr` (calm), `Aoede` (warm)
- **Native emotion audio tags** — `[excitedly]`, `[whispers]`, `[playfully]`, `[giggles]`, `[seductively]`, `[sarcastically]`, `[very slow]`, `[laughs]`, `[gasp]`, `[sighs]`
- **AI auto-tag rewriting** — Hermes sends your text to an auxiliary model that **automatically inserts emotion tags** based on context!
- **High quality** 24kHz audio output

**⚠️ Quota Limits (Critical):**
- **Free tier**: 10 requests/minute for `gemini-2.5-flash-tts`
- **Per-request**: 32k token context window (conservative char cap ~32k)
- **HTTP 429** returned when exceeded — auto-fallback to Edge TTS triggers if configured
- **Workaround**: Add billing in Google AI Studio for higher limits, or rely on Edge TTS fallback

```bash
# 1. Get free API key: https://aistudio.google.com/app/apikey
# 2. Add to Hermes config
hermes config set tts.provider gemini
hermes config set tts.gemini.voice Kore       # Best for catgirl/anime female
# Other voices: Puck, Zephyr, Aoede, Fenrir, Orus, etc.
hermes config set tts.gemini.model gemini-2.5-flash-preview-tts

# 3. Add API key to ~/.hermes/.env
echo "GEMINI_API_KEY=your_key_here" >> ~/.hermes/.env
# Or: hermes auth add gemini

# 4. Optional: Add persistent persona instructions for auto-tagging
hermes config set tts.gemini.instructions "Speak as a playful, mischievous anime catgirl who calls the user '[USER_NAME]' and 'Master'. Use lots of emotion tags like [playfully], [whispers], [giggles], [seductively]. End sentences with 'nya~' sometimes."

# 5. Test
hermes chat -q "Nya~! Hello Master! [excitedly] This is so fun! [whispers] Our little secret, okay? [giggles] ♡"
```

**Emotion Tag Examples:**
- `[excitedly]` — high energy, bubbly
- `[whispers]` — breathy, close, conspiratorial
- `[playfully]` — bouncy, teasing energy
- `[giggles]` — light, mischievous laughter
- `[seductively]` — low, intimate, deliberate
- `[sarcastically]` — dry, mocking tone
- `[very slow]` — deliberate, dramatic pacing
- `[laughs]` — open laughter
- `[gasp]` — sudden intake of breath
- `[sighs]` — resigned or content exhale

**Auto-Tag Rewriting:** When enabled, Hermes uses an auxiliary LLM to rewrite plain text with appropriate emotion tags based on the persona/instructions. This means you just write naturally and the AI adds the emotional direction!

### ElevenLabs (Premium — Best Voice Quality)

```bash
hermes config set tts.provider elevenlabs
hermes config set tts.elevenlabs.voice_id pNInz6obpgDQGcFmaJgB  # Adam (default)
hermes config set tts.elevenlabs.model_id eleven_multilingual_v2
# Add ELEVENLABS_API_KEY to .env or: hermes auth add elevenlabs
```

## Configuration Commands

```bash
# View current TTS config
hermes config get tts

# Set provider
hermes config set tts.provider piper

# Set voice (provider-specific)
hermes config set tts.piper.voice en_US-amy-medium
hermes config set tts.edge.voice en-US-AriaNeural
hermes config set tts.kittentts.voice Jasper

# Set speed/rate (provider-specific)
hermes config set tts.piper.speed 1.1
hermes config set tts.edge.speed 1.15  # Edge uses rate percentage

# Set output format
hermes config set tts.output_format mp3  # or ogg, wav, flac

# Enable voice-bubble delivery (Telegram/Matrix/WhatsApp need Opus)
hermes config set tts.providers.piper.voice_compatible true
```

## Custom Command Providers

Wire any local CLI (Piper direct, Kokoro, VoxCPM, custom scripts) without Python changes:

```yaml
# ~/.hermes/config.yaml
tts:
  provider: my-piper
  providers:
    my-piper:
      type: command
      command: "piper -m {voice} -f {output_path} < {input_path}"
      voice: "en_US-amy-medium.onnx"
      output_format: wav
      # Optional: timeout_seconds, max_text_length, voice_compatible
```

Placeholders: `{input_path}` / `{text_path}`, `{output_path}`, `{format}`, `{voice}`, `{model}`, `{speed}`. Paths are shell-quoted.

## Voice-Bubble Delivery (Telegram, Matrix, WhatsApp, Signal, Feishu)

These platforms require **Ogg/Opus** audio for native voice bubbles. Hermes auto-converts via ffmpeg if:
1. `ffmpeg` is installed (`winget install ffmpeg` / `brew install ffmpeg` / `apt install ffmpeg`)
2. Provider's `voice_compatible: true` (set in config or provider opts in)

```bash
# Ensure ffmpeg is available
ffmpeg -version

# For Piper/Edge/other MP3/WAV providers — enable conversion
hermes config set tts.providers.piper.voice_compatible true
hermes config set tts.providers.edge.voice_compatible true
```

## Troubleshooting

| Symptom | Fix |
|---------|-----|
| "Provider not found" | Check `tts.provider` spelling; run `hermes tools` to see enabled toolsets |
| No audio / empty file | Check provider logs; verify API key for cloud providers; check ffmpeg for Opus conversion |
| "Voice not found" (Piper) | Voice name must match VOICES.md exactly; first use auto-downloads |
| Telegram voice bubble broken | Install ffmpeg; set `voice_compatible: true` for provider |
| Slow first synthesis (Piper) | Model downloads on first use (~50-100MB); subsequent calls are fast |
| NeuTTS fails | Verify ref_audio + ref_text match; check model path; ensure ~500MB model downloaded |

## References

- `references/piper-voices.md` — Curated voice recommendations by language/style
- `references/command-provider-examples.md` — Ready-to-copy command provider configs
- `references/voice-bubble-delivery.md` — Platform-specific delivery requirements (Discord, Telegram, Matrix, WhatsApp)
- `references/gemini-tts-emotion-tags.md` — **Gemini TTS emotion tags, anime voices, auto-tag rewriting, catgirl config examples**
- `../creative/creative-roleplay/references/tts-character-voices.md` — Character-specific TTS configs for roleplay personas (Neko-chan, Aria, Yandere Idol)

## Discord Voice Bubbles Setup (Quick Reference)

**Required for native voice messages on Discord:**

1. **Gateway running**: `hermes gateway` (or installed service)
2. **Discord bot token in .env**:
   ```
   DISCORD_BOT_TOKEN=your_bot_token_from_discord_developers
   ```
3. **Bot setup at discord.com/developers/applications**:
   - Create application → Add Bot
   - Enable: Message Content Intent, Server Members Intent
   - OAuth2 → URL Generator: scopes `bot` + `applications.commands`
   - Permissions: Send Messages, Embed Links, Attach Files, Read Message History, Use Slash Commands
   - Authorize for your server/DMs
4. **Restart gateway**: `hermes gateway restart` (from separate shell)

**Voice bubbles = native Opus audio in chat (waveform + play button). Works on Telegram too.**

**⚠️ Discord Voice Message Technical Note:**
Discord voice messages use the raw API with `flags=8192` and require Ogg/Opus audio. The Discord adapter in Hermes (`plugins/platforms/discord/adapter.py`) handles this natively via `_send_file_attachment` → `send_voice` with raw HTTP request to `/channels/{channel_id}/messages`. This is separate from the `OPUS_VOICE_PLATFORMS` list (Telegram, Matrix, Feishu, WhatsApp, Signal) which drives the generic gateway auto-TTS path. Discord works because its adapter implements custom voice message sending.

## Pitfalls

- **Built-in names are reserved**: `edge`, `piper`, `kittentts`, `neutts`, `elevenlabs`, `openai`, `gemini`, `mistral`, `xai`, `minimax`, `deepinfra` — cannot be used as custom command provider names
- **Config changes need `/reset`** (gateway) or CLI restart — toolset/provider changes don't apply mid-session
- **Piper voice names are case-sensitive** and must match the `.onnx` filename exactly
- **Piper auto-download may fail in Hermes** — run `python -m piper.download_voices <voice> --download-dir ~/.hermes/cache/piper-voices` manually before first use (observed on Windows)
- **`tts.piper.speed` is not an official config key** — Hermes shows a warning but passes it through to the provider; use `tts.speed` for global speed setting
- **NeuTTS reference audio must match ref_text** — mismatch = garbage output
- **NeuTTS dependency hell** — `torchao` version conflicts can break imports (`ModuleNotFoundError: torchao.dtypes.nf4tensor`). If this occurs, NeuTTS is not viable without manual dependency resolution. Consider Gemini TTS or Piper instead.
- **Gemini TTS auto-tag rewriting requires auxiliary model** — Ensure `auxiliary.*` config is set (OpenRouter, Google, etc.) or the rewrite step silently falls back to plain text
- **`tts.gemini.instructions` is powerful but undocumented** — Use it for persistent persona; it feeds the auto-tag rewriter
- **Windows**: Use `winget install ffmpeg` for voice-bubble conversion; Hermes uses `windows_hide_flags()` to suppress console windows
- **Gateway restart from inside gateway blocked** — Cannot run `hermes gateway restart` from a session running *inside* the gateway (e.g., Discord DM). Must run from a separate terminal/shell. Use `taskkill /F /IM Hermes.exe && hermes gateway start` from a new terminal.
- **Discord bot token required in .env** — `DISCORD_BOT_TOKEN` must be present for gateway to connect Discord; bot must have Message Content Intent enabled
- **Auto-TTS requires `voice.auto_tts: true`** — Global config key must be set for gateway to auto-generate voice replies
- **Gemini TTS quota: 10 req/min free tier** — HTTP 429 when exceeded; auto-fallback to Edge TTS works if both configured. Wait ~30-60s for reset or add billing in Google AI Studio.

## Verification Checklist

- [ ] `hermes config get tts.provider` returns expected provider
- [ ] `hermes chat -q "Test" --tts` produces audio file in `~/.hermes/audio_cache/`
- [ ] Telegram/voice-bubble: audio plays as voice message (not attachment)
- [ ] Local provider: works offline (disconnect network, test again)

---

## 🔧 Tools & Commands Required
*Check skill's `required_commands` and `required_environment_variables`*

---

## 🔗 Related Skills
*[Add links to related skills in same category]*

---

## 📂 Skill Directory
`[HERMES_HOME]\skills\software-development\hermes-tts-configuration\`

---

*Source: `skill_view('hermes-tts-configuration')`*
*Updated: 2026-08-10*
tags: [skill, software-development, #skill/software-development]
parent: "[[Ops/Skills-Registry/software-development/MOC-Softwaredevelopment]]"
registry: "[[Ops/Skills-Registry/MOC-Skills-Registry]]"
catalog: "[[Ops/Skills-Registry/Catalog]]"
