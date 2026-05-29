# FAQ

Common questions about the VTS connector for Claude and ChatGPT.

---

## Setup

### Do I need a VTS account to use the connector?

Yes — but creating one is free and takes about 10 seconds. The first time you need a paid feature (or you hit the 300-minute monthly free limit), the connector prompts you to sign in via magic-link email. No credit card required to start.

### Do I need a credit card?

Only if you go past the free tier. The 300 minutes / month free includes the connector, all 10 tools, and standard transcription. Speaker diarization, denoise, and anything past the free cap need a wallet top-up or Pro plan.

### Does the connector cost anything?

No. Adding the connector is free. The VTS rate is the same whether you call it from Claude, ChatGPT, the web app, or the API — no AI-tax surcharge.

### Will I see the OAuth popup every time?

No. VTS issues refresh tokens, so the connector stays signed in for **60 days** before re-prompting. Access tokens rotate hourly behind the scenes; you only see the consent popup on first connect (and again after 60 days).

If you see it more often, you're probably on an older Claude or ChatGPT build that predates refresh-token support — update the app.

---

## Capabilities

### What sources can it transcribe?

Public URLs:
- YouTube: `youtube.com`, `youtu.be`, `m.youtube.com`, `music.youtube.com`
- Facebook: `facebook.com`, `m.facebook.com`, `web.facebook.com`, `fb.watch`

Uploaded files (via `upload_media`):
- Audio: `.mp3`, `.m4a`, `.wav`, `.flac`, `.ogg`, `.opus`, and more
- Video: `.mp4`, `.mov`, `.mkv`, `.webm`, `.avi`, and more
- 2 GB max upload size

For Vimeo, TikTok, X, raw audio URLs, or other unsupported sources, transcribe at [vts.askgiya.com](https://vts.askgiya.com) directly and paste the result into your chat.

### How accurate is the transcription?

Typical word-error rate for clean single-speaker audio in major languages: 3–8%.

For heavy accents, overlapping speakers, or noisy backgrounds: 10–20%.

Songs are the hardest — denoise + the higher tiers help, but expect lower accuracy than spoken-word audio.

Full bands by audio type: [vts.askgiya.com/sla](https://vts.askgiya.com/sla).

### What languages does it support?

Whisper-class language coverage — 100+ languages. Major languages (English, Spanish, French, German, Mandarin, Portuguese, Japanese, Arabic, Russian, Hindi, Italian, Korean) get the best accuracy.

Auto-detect is on by default; if the engine detects the wrong language (common with very short clips or unusual accents), you can pin it with a prompt like *"transcribe this Spanish video"* and the AI will set the language hint.

### Can it diarize (label speakers)?

Yes — set `diarize=true` or just say *"transcribe with speaker labels"* in your prompt.

- Available on clips **under 20 minutes**
- Adds a per-minute surcharge (see [pricing.md](pricing.md))
- If the engine fails to produce labels, you're auto-refunded down to the base rate

### Can it translate?

The connector itself returns transcripts in the source language. **Translation** is then done by the AI (Claude or ChatGPT) reading the transcript and producing the translated version in-chat.

Example: *"Transcribe this Spanish video and translate the key claims to English."* — VTS handles the transcription, Claude / ChatGPT handles the translation.

### Can it caption a YouTube video?

Yes — ask for the SRT output and either drop it into your video editor (Premiere, Final Cut, DaVinci) or upload it directly to YouTube as a caption file.

---

## Data & privacy

### What does VTS see when I use the connector?

When you transcribe a URL: VTS sees the URL, your account ID, and the resulting transcript text.

When you upload a file: VTS sees the file's audio content (extracted at upload time), your account ID, and the transcript.

When you read a saved transcript: VTS sees the transcript ID and your account ID.

### What does Claude / ChatGPT see?

The full transcript flows through the chat history as a regular message, so anything the AI does with it (summarize, quote, translate, paste into a document) is subject to the AI provider's data-handling policies — Anthropic for Claude, OpenAI for ChatGPT.

The **audio file itself never reaches the AI** — VTS extracts the audio, transcribes it on our infrastructure, and only the text + download links flow back into the chat.

### Does VTS use my data for training?

No. VTS does not train AI models on your transcripts.

### Where is data stored?

Transcripts are stored on VTS infrastructure (US/EU regions). They live in your account until you delete them from the [dashboard](https://vts.askgiya.com/dashboard). Account deletion purges all transcripts.

For full details: [vts.askgiya.com/privacy](https://vts.askgiya.com/privacy).

### Can I delete a transcript from inside the chat?

No — deletion is intentionally not exposed as an AI tool, to keep destructive ops off the chat surface. Delete from the [VTS dashboard](https://vts.askgiya.com/dashboard).

---

## Limits & errors

### What happens if I hit a quota mid-prompt?

The connector returns `reason=topup` (wallet too low for the cost) or `reason=blocked` (free-tier cap hit). The AI surfaces the message and offers `get_topup_link` so you can pay and retry in the same chat without losing context.

### What's the longest file I can transcribe?

- **Free tier:** 20 minutes per file
- **PAYG:** 4 hours per file (the system-wide cap)
- **Pro:** 2 hours per file (raised from 20 min for free-tier files; system cap still applies)

For longer files, split into segments and feed them in sequence. The AI can stitch the transcripts back together in-chat.

### What if the engine fails on my audio?

You're not charged. The job marks itself `error` with a clear reason ("we couldn't read this file", "no speech detected", etc.) and you can re-queue or try a different approach (denoise pre-pass, different segment).

### Can I get a refund if a transcript is wrong?

Per the [refund policy](https://vts.askgiya.com/refund-policy):

- **Failed transcriptions:** auto-refunded, no ticket needed
- **Successful but low-accuracy transcriptions** (e.g. a song where Whisper hallucinated): the auto-escalation ladder tries to get you above the SLA floor for that audio type at no extra cost. If the final result still falls below the [published SLA band](https://vts.askgiya.com/sla) for your audio type, contact support for a credit.
- **Pro subscriptions:** not refundable on cancel, dispute, or prorated mid-month.

---

## Compatibility

### Does it work in Claude Code (the CLI)?

The connector is built for the Claude.ai web app and Claude Desktop. Claude Code supports MCP servers via stdio + remote configurations — see [docs.claude.com](https://docs.claude.com) for setup.

### Does it work in Cursor / Windsurf / other MCP-capable IDEs?

It uses the standard MCP HTTP protocol, so yes — any client that supports remote MCP connectors over HTTP can add it. Point them at `https://vtsmcp.askgiya.com` and approve the OAuth flow.

### Does it work in voice mode?

No. Voice mode in both Claude and ChatGPT is its own pipeline (live mic input → transcript) and doesn't call custom connectors. Switch back to text mode to use VTS.

### Will there be a Firefox / Safari version?

The connector is server-side, so it doesn't matter what browser you're in — what matters is whether the AI app you're using supports custom connectors. Claude.ai and ChatGPT work in any modern browser (Chrome, Edge, Firefox, Safari).

---

## Other

### Why isn't there an MCP tool to upload a file from a URL?

We don't proxy arbitrary URLs to avoid being weaponized as an open relay (SSRF, file-fetch tarpits, copyright issues with paid sites). Supported hosts are the explicit YouTube / Facebook list above.

### Can I run my own MCP server pointing at the VTS API?

The VTS API is accessible via API keys (see your dashboard). The hosted connector at `https://vtsmcp.askgiya.com` is the supported path for AI clients. Self-hosted MCP servers are a "you're on your own" path — happy to point you at the API docs.

### How do I get help?

- **Bug in the connector or these docs:** [open an issue](https://github.com/raketbizdev/vts-mcp/issues)
- **Account, billing, or transcript-specific questions:** email [support@askgiya.com](mailto:support@askgiya.com)
- **Status page:** [vts.askgiya.com](https://vts.askgiya.com) (we'll post a banner if there's an outage)

---

## See also

- [Install on Claude](install-claude.md)
- [Install on ChatGPT](install-chatgpt.md)
- [Pricing details](pricing.md)
- [Troubleshooting](troubleshooting.md) — what to do when the connector misbehaves
