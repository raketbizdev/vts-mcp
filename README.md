# VTS MCP — Transcribe inside Claude and ChatGPT

[![VTS](https://img.shields.io/badge/Powered_by-vts.askgiya.com-4a7870)](https://vts.askgiya.com)
[![Free tier](https://img.shields.io/badge/Free-300_min%2Fmonth-2c8a76)](docs/pricing.md)
[![License](https://img.shields.io/badge/Docs_License-MIT-blue.svg)](LICENSE)

**Paste a YouTube or Facebook link inside Claude or ChatGPT. Get the transcript back in the same chat — ready to summarize, quote, translate, or repurpose.**

VTS MCP is a hosted Model Context Protocol connector for [vts.askgiya.com](https://vts.askgiya.com). Add it once to Claude or ChatGPT (about 60 seconds, no API key) and from then on every chat can transcribe a video link, search your past transcripts, and pull back any format — plain text, timestamped, or SRT.

This repo is the **user manual**: install guides, prompt recipes, the tool reference, and the troubleshooting playbook. There's no code to clone here — the connector runs on our infrastructure.

---

## See it in action

[![Watch the VTS Transcription demo](https://img.youtube.com/vi/gti8B8Q3XpA/maxresdefault.jpg)](https://www.youtube.com/watch?v=gti8B8Q3XpA)

Two-minute walkthrough of the install + first prompt. Click the thumbnail to play on YouTube.

---

## Who this is for

- **Content creators** — turn a podcast or YouTube interview into a script, show notes, or short-form clips without leaving the AI tab you already work in.
- **Power users / knowledge workers** — paste a meeting recap link, ask for action items, draft a follow-up email in one turn.
- **Journalists & researchers** — transcribe a public interview, pull the three sharpest quotes with timecodes, translate foreign-language clips.
- **Anyone tired of the copy-paste round trip** — drop the link, ask the question, get the answer. That's the whole flow.

This is documentation for **paying VTS customers** and **the 300-minutes-free tier**. VTS is a commercial service; the docs are licensed permissively so you can excerpt, mirror, or translate them.

---

## Quick install

| Tool | Guide | Time |
| --- | --- | --- |
| **Claude.ai (web)** | [→ Install on Claude.ai](docs/install-claude.md#option-1-claudeai-browser) | ~60s |
| **Claude Desktop** (macOS/Windows) | [→ Install on Claude Desktop](docs/install-claude.md#option-2-claude-desktop) | ~60s |
| **ChatGPT (web)** | [→ Install on ChatGPT](docs/install-chatgpt.md#option-1-chatgptcom-browser) | ~60s |
| **ChatGPT Desktop** (macOS/Windows) | [→ Install on ChatGPT Desktop](docs/install-chatgpt.md#option-2-chatgpt-desktop) | ~60s |

All four use the same connector URL — `https://vtsmcp.askgiya.com` — and the same OAuth sign-in. No API key, no config file.

---

## What you can do with it

Once the connector is wired in, you can paste a prompt like any of these into a normal chat and the AI calls VTS automatically:

```
Transcribe this video and pull the five sharpest quotes with timestamps:
https://www.youtube.com/watch?v=...
```

```
Translate this Spanish Facebook reel to English and summarize the main argument:
https://www.facebook.com/.../videos/...
```

```
Find the transcript in my library where the founder talked about pricing,
and turn it into a 200-word LinkedIn post.
```

The full prompt cookbook lives in [docs/prompts.md](docs/prompts.md). Workflows by persona (creator / student / journalist) are in [docs/creator-workflows.md](docs/creator-workflows.md).

---

## What the connector exposes

Ten tools — enough to turn any chat into a transcription workspace:

| Tool | What it does |
| --- | --- |
| `transcribe_url` | Transcribe a YouTube or Facebook video / reel |
| `quote_url` | Get the cost preview (free / paid + minutes) before committing |
| `upload_media` | Upload an audio or video file for transcription |
| `transcribe_upload` | Transcribe an uploaded file |
| `get_transcript` | Pull a saved transcript by ID |
| `get_format` | Get a specific format — plain text, timestamps, or SRT |
| `get_job_status` | Check on a long-running transcription job |
| `search` | Search your transcript library for matching content |
| `fetch` | Fetch a transcript for Deep Research mode |
| `get_topup_link` | Get a Stripe checkout link when your wallet is low |

Full reference with arguments and example prompts: [docs/tools.md](docs/tools.md).

---

## Supported sources

| Source | Works as | Notes |
| --- | --- | --- |
| YouTube videos | URL | `youtube.com`, `youtu.be`, `m.youtube.com`, `music.youtube.com` |
| Facebook videos & reels | URL | `facebook.com`, `m.facebook.com`, `web.facebook.com`, `fb.watch` |
| Audio files | Upload | `.mp3`, `.m4a`, `.wav`, `.flac`, `.ogg`, `.opus`, etc. |
| Video files | Upload | `.mp4`, `.mov`, `.mkv`, `.webm`, `.avi`, etc. |

For everything else (Vimeo, TikTok, X, raw audio URLs), use the [vts.askgiya.com](https://vts.askgiya.com) web app directly, then paste the transcript into Claude/ChatGPT.

---

## Pricing summary

- **300 minutes per month — free.** No credit card. 20-minute per-file cap on the free tier.
- **Past the free tier — 6¢/minute** from a prepaid wallet. Top up in $5 increments.
- **Pro plan — $20/month** for 25 hours included, plus a 120-minute per-file cap and priority queue.

Full table with options like speaker diarization, denoise pre-pass, and SLA tiers: [docs/pricing.md](docs/pricing.md).

---

## Docs index

- 🟢 [Install on Claude](docs/install-claude.md) — web + Desktop, three steps each
- 🟢 [Install on ChatGPT](docs/install-chatgpt.md) — web + Desktop, three steps each
- 📘 [Tools reference](docs/tools.md) — every tool, what it returns, when to call it
- 🍳 [Prompt cookbook](docs/prompts.md) — 30+ tested prompts you can copy
- 🎬 [Creator workflows](docs/creator-workflows.md) — podcast → show notes, interview → article, etc.
- 💰 [Pricing](docs/pricing.md) — free tier, PAYG, Pro, optional add-ons
- ❓ [FAQ](docs/faq.md) — accounts, data handling, language support
- 🛠️ [Troubleshooting](docs/troubleshooting.md) — popup-blocked, 404, "Sign in to enable", quota errors

---

## Status & contributions

- **Status:** v1, in active use. The connector URL `https://vtsmcp.askgiya.com` is stable.
- **Reporting issues** — open an issue on this repo with the steps to reproduce. For account-specific or billing questions, email [support@askgiya.com](mailto:support@askgiya.com) instead.
- **Pull requests** — typo fixes, clarifications, additional prompt recipes, and translations are welcome. Behavior changes to the connector itself happen on the VTS side, not in this repo.

---

## License

Documentation in this repo is [MIT-licensed](LICENSE) — fork it, excerpt it, translate it, mirror it.

VTS the service is a separate commercial product. The connector at `https://vtsmcp.askgiya.com` is operated by Ask Giya / Video Transcription Service. Use is governed by the [VTS terms](https://vts.askgiya.com/terms) and [privacy policy](https://vts.askgiya.com/privacy).
