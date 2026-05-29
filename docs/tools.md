# Tools reference

When the VTS connector is installed, ten tools become available to Claude / ChatGPT. You don't have to know the tool names — the AI picks the right one based on what you ask in plain language. This page is for **power users** who want to know exactly what's available so they can write more precise prompts.

The connector runs on `https://vtsmcp.askgiya.com`. Tools are exposed via the Model Context Protocol (MCP) standard, so the same surface works on Claude.ai, Claude Desktop, ChatGPT, ChatGPT Desktop, and any other MCP client.

---

## Quick map

| Tool | Use it for | Trigger phrase |
| --- | --- | --- |
| [`transcribe_url`](#transcribe_url) | Transcribe a YouTube or Facebook link | *"Transcribe this: <url>"* |
| [`quote_url`](#quote_url) | Get a cost preview before running | *"How much would it cost to transcribe <url>?"* |
| [`upload_media`](#upload_media) | Stage a local file for transcription | *"Upload this file"* (with attachment) |
| [`transcribe_upload`](#transcribe_upload) | Transcribe an uploaded file | (usually chained after `upload_media`) |
| [`get_transcript`](#get_transcript) | Pull a saved transcript by ID | *"Get me transcript #88"* |
| [`get_format`](#get_format) | Get a specific output format | *"Give me the SRT for this"* |
| [`get_job_status`](#get_job_status) | Check on a long-running job | *"Is my transcription done?"* |
| [`search`](#search) | Search your transcript library | *"Find the transcript where Lisa talked about pricing"* |
| [`fetch`](#fetch) | Fetch a transcript for Deep Research | (used by ChatGPT Deep Research automatically) |
| [`get_topup_link`](#get_topup_link) | Get a Stripe top-up URL when wallet is low | *"My wallet's empty, give me a top-up link"* |

---

## `transcribe_url`

Transcribe a public video URL.

**Inputs**

- `url` — the YouTube or Facebook link to transcribe
- `diarize` *(optional)* — set to `true` to label speakers (Speaker 1, Speaker 2, …). Adds a small surcharge; only available on clips under 20 minutes.
- `denoise` *(optional)* — set to `true` to run a noise-reduction pre-pass. Best for songs, noisy field recordings, or low-quality audio. Adds 2¢/min.

**Returns**

- Transcript title, uploader, duration, thumbnail
- A transcript card with three downloadable formats — plain text, timestamps, SRT
- Anonymous download links that work without sign-in
- The internal transcript ID (so the AI can reference it in follow-ups)

**Example prompt**

> *"Transcribe https://www.youtube.com/watch?v=… with speaker labels and pull the three best quotes."*

**Supported sources**

YouTube — `youtube.com`, `youtu.be`, `m.youtube.com`, `music.youtube.com`
Facebook — `facebook.com`, `m.facebook.com`, `web.facebook.com`, `fb.watch`

For sources outside this list (Vimeo, TikTok, X, raw audio URLs), use the [VTS web app](https://vts.askgiya.com) directly, then paste the transcript into your chat.

---

## `quote_url`

Get the cost preview for a URL without committing to the transcription. Useful when you want to know whether something fits in your free tier before running it.

**Inputs**

- `url` — same supported hosts as `transcribe_url`

**Returns**

- Duration in seconds
- Cost in cents (with a `is_free` flag if it qualifies for the 300-min/month free tier)
- A `reason` field if the request is blocked (e.g. `reason=signin`, `reason=topup`)

**Example prompt**

> *"How much would it cost to transcribe this 2-hour podcast? <url>"*

---

## `upload_media`

Upload an audio or video file directly to VTS for transcription. The file is staged with an opaque token; you then call `transcribe_upload` with that token to actually run it.

**Inputs**

- The file itself (the AI handles the upload mechanics)

**Supported formats**

- **Audio** — `.mp3`, `.m4a`, `.wav`, `.flac`, `.ogg`, `.opus`, `.aac`, `.aiff`, `.amr`, `.wma`
- **Video** — `.mp4`, `.mov`, `.mkv`, `.webm`, `.avi`, `.wmv`, `.flv`, `.mpeg`, `.mpg`, `.3gp`, `.ts`

**Max size:** 2 GB.

**Returns**

- An upload `token`
- Duration + title from the file

**Note:** Whether you can attach a local file depends on the AI you're using. Claude Desktop supports file attachments well; ChatGPT's audio-file support is less reliable on the chat surface — for those cases, upload at [vts.askgiya.com](https://vts.askgiya.com) and use `transcribe_url` against the resulting public link instead.

---

## `transcribe_upload`

Transcribe a previously-uploaded file using the token returned by `upload_media`.

**Inputs**

- `token` — the opaque upload token
- `diarize` *(optional)* — speaker labels
- `denoise` *(optional)* — noise-reduction pre-pass

**Returns**

Same shape as `transcribe_url`.

---

## `get_transcript`

Fetch a saved transcript by its numeric ID. The AI usually calls this on its own when you reference a previous transcript ("the transcript from earlier").

**Inputs**

- `transcript_id` — the integer ID

**Returns**

- Full transcript content + metadata
- All three formats (plain, timestamps, SRT) ready to display

**Example prompt**

> *"Pull transcript 88 and turn it into a LinkedIn post."*

---

## `get_format`

Get a single specific format of a transcript. Use this when the chat is already showing the transcript but you want, say, just the SRT to download for captioning.

**Inputs**

- `transcript_id` — the integer ID
- `format` — `plain`, `timestamps`, or `srt`

**Returns**

- Just the requested format, ready to copy or download

**Example prompt**

> *"Give me the SRT for this transcript so I can drop it into Premiere."*

---

## `get_job_status`

For long files (over ~30 minutes), the AI may need to poll the job status while transcription runs. This tool returns the current state.

**Inputs**

- `job_id` — the integer ID

**Returns**

- Status — `queued`, `fetching`, `transcribing`, `done`, `error`
- Stage description (human-readable)
- Progress fields when available

You usually don't call this directly — the AI does it automatically.

---

## `search`

Search across your VTS transcript library by content. Returns the top matches with snippets.

**Inputs**

- `query` — natural-language search string

**Returns**

- An array of `{ id, title, snippet, created_at }` matches

**Example prompt**

> *"Find the transcript where the CEO mentioned the pricing change."*

The AI then typically chains `fetch` or `get_transcript` to pull the full text.

---

## `fetch`

Companion to `search`. Returns the full transcript for a given ID, formatted for ingestion by ChatGPT's **Deep Research** mode.

**Inputs**

- `id` — the transcript ID

**Returns**

- Full transcript content with citation metadata

Deep Research will call this without you having to ask.

---

## `get_topup_link`

Returns a one-time Stripe Checkout URL for topping up your VTS wallet. The AI calls this when a `quote_url` or `transcribe_url` returns `reason=topup`.

**Inputs**

- `amount_cents` *(optional)* — how much to top up; defaults to $5

**Returns**

- `checkout_url` — open in a browser to complete payment

**Example prompt**

> *"I'm out of credit. Give me a top-up link for $20."*

---

## How responses come back

When a transcription completes, the AI presents the result in this order (per the VTS connector's prompt):

1. **Video as a clickable link** — `[Title](url)`
2. **Thumbnail** as a markdown image (when one is available)
3. **One-line meta row** — duration, word count, source label
4. The AI **asks which format** you want — plain, timestamps, SRT, or all three — before dumping the text
5. The chosen format(s) as a **markdown blockquote** (`> ` prefix per line) so it word-wraps on mobile
6. **Download links** for `.txt` and `.srt` files — anonymous one-shot URLs that work without sign-in

This shape is consistent across Claude and ChatGPT.

---

## Tools we don't expose (yet)

These are tracked on the VTS roadmap but not in the connector today:

- `start_recording` / `list_recordings` / `transcribe_recording` — recording capture from a browser extension (parked for a future release)
- `translate_transcript` — currently handled by the AI itself reading the transcript and translating in-chat; native tool may land later
- `delete_transcript` — to keep destructive ops off the AI surface, deletion happens in the [VTS dashboard](https://vts.askgiya.com/dashboard)

---

## See also

- [Prompt cookbook](prompts.md) — copy-pasteable prompts that exercise every tool
- [Creator workflows](creator-workflows.md) — multi-tool chains for real tasks
- [Troubleshooting](troubleshooting.md) — what the various `reason=` and error fields mean
