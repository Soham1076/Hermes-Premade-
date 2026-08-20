# 📝 Skill: tts-voice-selection

> **Category**: configuration • **Version**: 1.0.0 • **Status**: Available

---

## 📋 Overview
TTS for anime voices: Gemini, Piper, NeuTTS compared.

---

## 🎯 Use When
*[Auto-extracted from skill - customize based on actual usage patterns]*

---

## 📖 Full Skill Documentation

## When to Use

Use this skill when:
- User wants to **change TTS voice** for a character/roleplay persona (catgirl, yandere, anime-style)
- User asks for **emotional/expressive TTS** (whispers, excitement, giggles, seductive tones)
- User needs **fully local TTS** vs cloud tradeoffs
- User encounters **Windows TTS installation issues** (espeak-ng, ffmpeg, torchao conflicts)
- Comparing **Piper vs KittenTTS vs NeuTTS vs Gemini vs Edge vs ElevenLabs**

---

# TTS Voice Selection for Character Voices

Guide for picking and configuring TTS providers when you need **anime-style**, **emotional**, or **character-specific** voices — especially for roleplay personas like catgirls, yanderes, etc.

## Quick Decision Matrix

| **Premium, best character voices** | **ElevenLabs** | Anime girl presets, emotion control, but paid (free tier = 10k chars/mo). **WORKING ✅** |
| **Fully local + free, acceptable flat voice** | **Piper** (`en_US-libritts_r-medium`) | Best local quality, 904 speakers (but unlabeled), no emotion control. **WORKING ✅** |
| **Tiny local model (25MB)** | **KittenTTS** | Fast, tiny, but needs `espeak-ng` system install on Windows (pain). Robotic quality. |
| **Voice cloning + emotion (local)** | **NeuTTS** | Can clone any voice from reference audio, but dependency hell (`torchao` version conflicts). **BROKEN** |
| **Built-in, zero setup** | **Edge TTS** | `en-US-AriaNeural` + SSML rate/pitch tweaks. No emotion tags exposed in Hermes yet. **WORKING ✅** |

---

## Provider Deep Dives

### Piper (Local, Free, Best Quality)
```bash
pip install piper-tts
hermes config set tts.provider piper
hermes config set tts.piper.voice en_US-libritts_r-medium  # most expressive English voice
hermes config set tts.piper.speed 1.15
```
- **Voices**: 44 languages, dozens per language. Auto-downloads `.onnx` + `.onnx.json` to `~/.hermes/cache/piper-voices/`
- **Speaker IDs**: `en_US-libritts_r-medium` has 904 speakers but **no labels** — trial and error.
- **Emotion**: ❌ None. No SSML, no tags, no style control.
- **Windows**: Works out of the box.

### KittenTTS (Local, Free, Tiny)
```bash
pip install kittentts
# Windows: MUST install espeak-ng system package first!
# choco install espeak-ng  OR  winget install espeak-ng
```
- **Voices**: 4 built-in (Jasper, etc.) — all sound robotic.
- **Emotion**: ❌ None.
- **Windows gotcha**: `phonemizer` backend requires `espeak-ng` DLL in PATH. Not pip-installable.

### NeuTTS (Local, Free, Voice Cloning + Emotion)
```bash
pip install neutts
# Downloads ~500MB model (neuphonic/neutts-air-q4-gguf)
```
- **Strength**: Clone any voice from reference audio + reference text. Emotion transfers from reference.
- **Windows gotcha**: Dependency chain breaks on `torchao` version conflicts (as of 2026-08). `torchao.dtypes.nf4tensor` missing.
- **Not recommended until deps stabilize**.

### Gemini TTS (Cloud, Free Tier, **Best for Anime/Emotion**) — **Daily limit hit ⚠️**
```bash
# 1. Get key: https://aistudio.google.com/app/apikey
hermes config set tts.provider gemini
hermes config set tts.gemini.voice Kore        # cute female, anime-style
hermes config set tts.gemini.model gemini-2.5-flash-preview-tts
# Add GEMINI_API_KEY to ~/.hermes/.env or: hermes auth add gemini
```
- **Voices**: Kore, Puck, Zephyr, Aoede, Fenrir, etc. — all character-like.
- **Emotion tags**: `[whispers]`, `[excitedly]`, `[playfully]`, `[seductively]`, `[pouts]`, `[giggles]`, `[sarcastically]`, `[very slow]`, `[laughs]`, `[gasp]`, `[sighs]` — **actually work**.
- **AI auto-tagging**: Hermes sends your text to an auxiliary model that **rewrites it with emotion tags** based on context before sending to Gemini.
- **Free tier limits**: Generous per-minute but **daily cap hit after heavy testing** (~1500 req/day?). Once daily limit exceeded, **no requests work until next day** (not per-minute reset).
- **Fallback**: Auto-falls back to Edge TTS when quota exceeded.

### Edge TTS (Built-in, Free, Cloud)
```bash
hermes config set tts.provider edge
hermes config set tts.edge.voice en-US-AriaNeural
hermes config set tts.edge.speed 1.15  # rate="+15%"
```
- **Emotion**: SSML works but Hermes doesn't expose it yet. Only `rate` and `pitch` via config.

### ElevenLabs (Cloud, Free Tier 10k chars/mo) — **WORKING ✅**
```bash
hermes config set tts.provider elevenlabs
hermes config set tts.elevenlabs.voice_id EXAVITQu4vr4xnSDxMaL  # "Bella" - cute female
# ELEVENLABS_API_KEY in ~/.hermes/.env (FULL KEY, not truncated!)
```
- **Best preset character voices** (search "anime" in their library).
- **Emotion**: `stability`, `similarity_boost`, `style` parameters.
- **Auto-install**: Hermes lazy-installs `elevenlabs` package on first use.
- **⚠️ CRITICAL**: `.env` must contain the **FULL API key**, not truncated (`sk_7b6...1add` fails). The full key format: `sk_7b6ad49749a792647d68018ac25acdbf3b97794505ca1add`
- **Voice ID examples**: `EXAVITQu4vr4xnSDxMaL` (Bella), `21m00Tcm4TlvDq8ikWAM` (Rachel), `pNInz6obpgDQGcFmaJgB` (Adam). Browse more at https://elevenlabs.io/app/voice-library

---

## Windows-Specific Gotchas

| Issue | Fix |
|-------|-----|
| `espeak-ng` missing for KittenTTS/NeuTTS | `choco install espeak-ng` or `winget install espeak-ng`, then restart shell |
| `torchao` version mismatch for NeuTTS | No clean fix yet — wait for upstream update |
| `ffmpeg` needed for Opus/Telegram voice bubbles | `choco install ffmpeg` or `winget install ffmpeg` |
| Piper voice download path | Uses `~/.hermes/cache/piper-voices/` (profile-aware) |

---

## Recommended Workflow for Character Voices

1. **Try Gemini TTS first** — only option with working emotion tags + anime voices + free tier.
2. **If must be local**: Piper `en_US-libritts_r-medium` + `speed=1.15` — best flat voice.
3. **If voice cloning needed**: Wait for NeuTTS deps to stabilize, or use ElevenLabs (paid).
4. **Test with roleplay text**: Use text with `nya~`, emotional beats, whispers, giggles to evaluate.

---

## References

- `references/piper-voice-list.md` — Full Piper voice catalog with speaker ID notes
- `references/gemini-emotion-tags.md` — Complete Gemini TTS audio tag list + examples
- `references/windows-tts-gotchas.md` — Windows-specific installation issues and fixes

---

## 🔧 Tools & Commands Required
*Check skill's `required_commands` and `required_environment_variables`*

---

## 🔗 Related Skills
*[Add links to related skills in same category]*

---

## 📂 Skill Directory
`C:\Users\soham\AppData\Local\hermes\profiles\cosmos\skills\configuration\tts-voice-selection\`

---

*Source: `skill_view('tts-voice-selection')`*
*Updated: 2026-08-10*
tags: [skill, configuration, #skill/configuration]
parent: "[[Ops/Skills-Registry/configuration/MOC-Configuration]]"
registry: "[[Ops/Skills-Registry/MOC-Skills-Registry]]"
catalog: "[[Ops/Skills-Registry/Catalog]]"
