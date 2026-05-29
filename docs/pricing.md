# Pricing

VTS bills you the same way whether you call it through the chat connector or through the web app — there's **no surcharge for going through Claude or ChatGPT**. The connector itself is free; you pay only for the transcription minutes you actually run.

> This page reflects pricing as of 2026. The live source of truth is [vts.askgiya.com](https://vts.askgiya.com) — check there if a number here looks stale.

---

## The headline

- **Free tier:** 300 minutes / month
- **Past free tier:** 6¢ per minute, prepaid wallet
- **Pro plan:** $20 / month for 25 hours included + priority queue

That's the whole thing. No per-seat fees, no annual contracts, no per-tool charges.

---

## Free tier (no card required)

Three caps must all pass for a transcription to qualify as free:

| Cap | Value | When it resets |
| --- | --- | --- |
| Monthly minutes | 300 / calendar month | 1st of the month, UTC |
| Daily minutes | 60 / UTC day | Each day at 00:00 UTC |
| Per-file duration | 20 minutes | n/a (hard limit) |

If any cap is hit, the next transcription returns `reason=topup` or `reason=blocked` and the AI prompts you to top up your wallet or upgrade to Pro.

The 20-minute per-file cap is the one most people bump into first — long podcasts, full meetings, and feature-length interviews don't qualify as free even if you're under the monthly cap. That's where PAYG or Pro kicks in.

---

## Pay as you go (PAYG)

Top up your VTS wallet in any increment of $5. The connector charges per transcription at:

| What | Rate |
| --- | --- |
| Standard transcription | **6¢ / minute** |
| Speaker labels (diarization, opt-in) | **10¢ / minute** |
| Denoise pre-pass (opt-in) | **+2¢ / minute** on top of the base rate |

Charging is **idempotent** — a repeated job (refresh, retry, post-payment resume) never double-charges.

Failed transcriptions are **never** billed. If we couldn't read your audio or the job errored on our side, you're not charged.

### When you'd ask for a top-up

If your wallet is below the quoted cost for a transcription, the AI surfaces `reason=topup` and the `get_topup_link` tool returns a Stripe Checkout URL. Common prompt shapes:

```
I'm out of credit. Give me a $20 top-up link.
```

```
What's my current wallet balance?
```

---

## Pro plan ($20 / month)

For heavy users — researchers, journalists, podcast producers, anyone running 5+ hours of transcription a week.

| What you get | Detail |
| --- | --- |
| **25 hours included** every month | Resets on your renewal date |
| **120-minute per-file cap** | Vs. 20-min on the free tier |
| **Priority queue** | Pro jobs jump ahead of free + PAYG in the worker queue |
| **No per-minute charges** | …until you exceed 25 hours; overage falls back to 6¢/min wallet PAYG |

The 25 hours is the **single best deal** if you're running more than ~5.5 hours/month — that's the break-even vs. 6¢/min PAYG.

[Subscribe at vts.askgiya.com/billing →](https://vts.askgiya.com/billing)

### Refund policy

- Failed transcriptions are auto-refunded — no ticket needed.
- Pro subscriptions are **not** prorated or refunded on cancel. Cancelling stops the next renewal; your current month stays active through the period end.
- Disputes don't refund Pro subscriptions either.

---

## Optional add-ons

### Speaker diarization ("Label speakers")

When set on a transcription, every line in the output is prefixed with a Speaker label (Speaker 1, Speaker 2, …) so it's clear who said what.

- **Rate:** 10¢ / minute (replaces the base 6¢)
- **Max duration:** 20 minutes per file (on CPU — longer files transcribe without labels)
- **Fairness:** if the engine fails to actually produce labels (HF model unavailable, etc.), we automatically re-bill at the base 6¢ rate. You never pay the diarized rate for labels you didn't get.

Trigger phrase: *"Transcribe this with speaker labels"*

### Denoise pre-pass

A noise-reduction filter that runs before transcription. Best for songs, noisy field recordings, low-quality phone audio.

- **Rate:** +2¢ / minute on top of the base or diarized rate
- **Behavior:** can be the first pass, or chained on a re-transcribe of an existing transcript ("re-run this with denoise")

Trigger phrase: *"Transcribe this with denoise"* or *"Re-transcribe this with noise reduction"*

### SLA auto-escalation

When the first transcription pass comes back with a low confidence score (typical on songs or noisy audio), VTS auto-escalates to a heavier engine on the same job:

1. **Tier 0:** small model + VAD (default, fastest)
2. **Tier 1:** denoise + medium model + song-mode params
3. **Tier 2:** OpenAI Whisper API (large)

You don't pay extra for the escalation — the original charge covers all attempts. Full SLA bands by audio type: [vts.askgiya.com/sla](https://vts.askgiya.com/sla).

---

## What the AI tells you about cost

When you ask the AI to transcribe something, it can:

- Call `quote_url` first to show the cost preview before committing
- Tell you which tier the file falls into (free / PAYG / Pro included)
- Surface `top_up_required_cents` if your wallet can't cover it

Useful prompts:

```
Before transcribing, tell me the cost: <url>
```

```
How much would 4 hours of transcription a week cost me on PAYG vs. Pro?
```

```
My free tier is mostly used — what's left this month?
```

---

## What the AI doesn't bill for

- Adding the connector (free, one-time setup)
- Tools that **read** existing transcripts — `get_transcript`, `get_format`, `search`, `fetch` — are free, they just pull from data you already paid to create
- Cost previews (`quote_url`) are free
- `get_topup_link` is free

---

## Big-picture price examples

| Workflow | Minutes | Cost |
| --- | --- | --- |
| One 15-min YouTube interview | 15 | **Free** (under all 3 caps) |
| Daily 60-min podcast for a month | 1,800 (~30h) | Pro $20 covers 25h; remaining 5h = $18 PAYG → **~$38** |
| Single 4-hour earnings call | 240 | PAYG: 240 × 6¢ = **$14.40** |
| 5-hour deposition with speaker labels | 300 | 300 × 10¢ = **$30** (≤ 20 min/file cap requires splitting, see [tools.md](tools.md)) |
| Music video for SRT captions (denoise) | 4 | Free tier; denoise adds nothing under the monthly cap |

---

## See also

- [VTS billing page](https://vts.askgiya.com/billing) — top up, subscribe to Pro, manage cards
- [VTS dashboard](https://vts.askgiya.com/dashboard) — your transcript library + wallet history
- [VTS SLA](https://vts.askgiya.com/sla) — accuracy bands by audio type
- [Tools reference](tools.md#get_topup_link) — `get_topup_link` tool behavior
- [FAQ](faq.md#do-i-need-a-vts-account-to-use-the-connector) — account, data handling, language support
