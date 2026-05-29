# Troubleshooting

Common failure modes and their fixes. If you hit something that isn't here, [open an issue](https://github.com/raketbizdev/vts-mcp/issues) with the steps to reproduce.

---

## Connection problems

### "Sign in to enable" never goes away

The OAuth popup was blocked by your browser. Open your site permissions, allow popups from `claude.ai` (or `chatgpt.com`), then click the connector's **Sign in** link again.

### OAuth popup shows "404 Not Found"

Your connector is pointing at the wrong URL. Double-check the server URL is exactly:

```
https://vtsmcp.askgiya.com
```

No trailing slash, no `/mcp` suffix needed. Some people accidentally paste the marketing site (`https://vts.askgiya.com`) — that returns the website, not the MCP server.

### OAuth popup closes immediately

Your browser intercepted the redirect, or you have a popup blocker on `vts.askgiya.com`. Click the connector again — Claude Desktop falls back to its in-app browser on the second attempt; on the web you may need to disable the blocker for VTS's domain.

### Tools don't show up in the picker

Connectors load when a chat session starts, not retroactively. Open a new chat or refresh the current one and the tools will appear.

### "Connector unavailable" / 500 errors when adding it

Check [vts.askgiya.com](https://vts.askgiya.com) directly — if the site is down, the connector is too. Otherwise, try removing and re-adding the connector. If it persists, [open an issue](https://github.com/raketbizdev/vts-mcp/issues) with the error message.

### Sign-in popup every hour

This shouldn't happen — VTS issues refresh tokens that keep the connector signed in for 60 days. If you're seeing the popup more often, you're probably on an older client build that predates refresh-token support. Update Claude or ChatGPT (or restart the Desktop app) and the issue should resolve.

### "Add custom connector" button isn't there

Custom connectors require a recent build of Claude Desktop / ChatGPT Desktop. Open **About** (or the equivalent menu) and confirm you're on a current version, then restart the app.

On Claude.ai or ChatGPT.com (web), the section may not be visible if your account is on a plan that doesn't include custom connectors yet. Custom connectors rolled out in waves — check the provider's release notes if you don't see it.

---

## Recording / transcription failures

### Nothing happens when I paste a link

Make sure:

1. The connector is **connected** (green check next to "VTS Transcription" in your connector list).
2. The URL is from a [supported host](tools.md#transcribe_url) — YouTube or Facebook. For other sources, use the [VTS web app](https://vts.askgiya.com) directly.
3. The video is **public** — VTS can't reach age-restricted, unlisted-with-required-sign-in, or region-locked videos.
4. The AI is in **text mode**, not voice mode. Voice mode doesn't call custom connectors.

If all four pass and it still doesn't work, prepend your prompt with the explicit ask: *"Use the VTS connector to transcribe https://..."* — sometimes the AI just needs the nudge.

### "Unsupported URL" error

The connector only handles YouTube and Facebook URL hosts (see [tools.md](tools.md#transcribe_url) for the exact list). For Vimeo, TikTok, X, raw audio URLs, or anything else, transcribe at [vts.askgiya.com](https://vts.askgiya.com) and paste the resulting transcript into your chat.

### "Free tier ran out mid-transcription"

You hit one of the three free-tier caps:

- Monthly (300 min / month, resets 1st UTC)
- Daily (60 min / UTC day)
- Per-file (20 min on free tier; 120 min on Pro)

The AI relays the error and offers `get_topup_link`. Top up your wallet (default $5) and the next prompt in the same chat picks up where it left off.

```
Give me a $20 top-up link.
```

### "Long files take a few minutes" — and I'm worried it's stuck

Files over 30 minutes can take 1–3 minutes to transcribe on the medium model; over an hour can take longer. The AI shows "Transcribing…" while it waits.

- **Don't refresh** — you'll lose the chat context.
- The connector polls `get_job_status` in the background; the AI will surface the result when it's done.
- For files over an hour, consider splitting them at natural breaks for higher per-segment accuracy.

### Speakers aren't labeled

Diarization is **opt-in**. Add the trigger phrase: *"transcribe with speaker labels"* or pass `diarize=true` in your prompt. The per-file cap on diarized audio is 20 minutes (it runs on CPU and gets expensive on longer clips).

### Transcript looks wrong on a song

Songs are hard for Whisper-class engines — overlapping vocals, instrumentation, autotune, and reverb all cause hallucinations. Try:

```
Re-transcribe this with noise reduction (denoise).
<url or transcript ID>
```

VTS auto-escalates through 3 engine tiers (small → medium with denoise → OpenAI large) until accuracy clears the floor for the audio type. See [SLA bands](https://vts.askgiya.com/sla) for what to expect by audio type.

### Transcript came back as the wrong language

Whisper's language detection can misfire on:

- Very short clips
- Strong accents
- Mixed-language audio (the engine locks onto the first language it detects)

Force the language by saying so in your prompt:

```
Transcribe this Tagalog interview (Whisper sometimes guesses Spanish):
<url>
```

### Speaker labels are wrong (Speaker 1 and Speaker 2 are swapped)

Diarization assigns labels in order of first appearance, not by identity. If you know who spoke first, ask the AI to relabel:

```
In this transcript, Speaker 1 is the host and Speaker 2 is the guest.
Show me the transcript with those names instead.
```

---

## Wallet / payment

### "Your balance is too low"

Your wallet doesn't cover the transcription cost. The AI tells you the exact shortfall (e.g. `top_up_required_cents: 198`) and offers a checkout link via `get_topup_link`. Top up in $5 increments.

### Payment went through but my balance didn't update

Charging is idempotent on the Stripe session id — refreshes and retries never double-charge, but they can occasionally race. Refresh the [billing page](https://vts.askgiya.com/billing) directly to confirm. If the balance still hasn't updated 5 minutes after the Stripe confirmation, email [support@askgiya.com](mailto:support@askgiya.com) with the Stripe session ID.

### How do I cancel Pro?

Cancel from the [billing page](https://vts.askgiya.com/billing) (Stripe Customer Portal). Cancellation takes effect at the end of the current billing period — your transcription budget stays active until then.

Pro subscriptions are **not** refunded on cancel, dispute, or prorated mid-month.

---

## Inside the chat

### The AI didn't use VTS — it just dumped what it remembered about the video

This happens when the AI thinks it already knows the content (popular YouTube videos that were in its training data). Force the tool with an explicit prompt:

```
Use the VTS transcription tool to transcribe this URL — don't summarize
from memory. I want the actual transcript:
<url>
```

### The AI says "I don't have a transcription tool" but I added the connector

Connectors are per-conversation. Open a **new chat** (not the one that was open before you added the connector) — the tools load when a session starts.

If a new chat still doesn't show the tools, the connector is probably disconnected. Check the connector page; sign in again if needed.

### The transcript text is huge and overflowing my screen

By default, VTS returns the transcript as a markdown blockquote (per-line `> ` prefix) so it word-wraps on mobile. If yours is overflowing, you may be on an older connector that uses a fenced code block — ask the AI explicitly:

```
Show that transcript as a blockquote, not a code block.
```

### I want to skip the "Which format would you like?" question

VTS asks before dumping the full transcript so the chat doesn't get flooded. To skip:

```
Transcribe this and just give me the plain transcript — no format
picker, straight to the text:
<url>
```

---

## Performance

### Transcription is slow

Standard turnaround for a 60-minute file is 2–6 minutes on the medium model. Longer if:

- The free queue is busy (Pro users have priority queue access)
- Diarization is on (CPU-bound)
- Denoise is on (adds a preprocessing pass)
- The file is over an hour

If a job has been "transcribing" for more than 15 minutes on a file under 30 minutes, something's wrong — [open an issue](https://github.com/raketbizdev/vts-mcp/issues) or email support with the job ID.

### My VTS dashboard is missing transcripts I created via the connector

Transcripts created through the connector show up in the same library as web-app transcripts at [vts.askgiya.com/dashboard](https://vts.askgiya.com/dashboard). If one is missing:

1. Confirm you're signed into VTS with the **same email** as the OAuth identity used by the connector.
2. Check the dashboard's date filter — connector transcripts use the same `created_at` as web ones.
3. If you transcribed a YouTube video the AI knew about, it may have skipped the tool — there's no transcript to find. Re-run with the explicit "use the VTS tool" prompt above.

---

## See also

- [Install on Claude](install-claude.md)
- [Install on ChatGPT](install-chatgpt.md)
- [Tools reference](tools.md) — what each tool returns, including error fields
- [FAQ](faq.md) — broader questions about capabilities and data handling
- [VTS support](mailto:support@askgiya.com) — for account-specific issues
