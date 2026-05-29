# Install VTS MCP in ChatGPT

Same three steps on the web and Desktop apps. Both use the **custom connector** flow with OAuth — no API key, no Python, no config file. Total time: about 60 seconds.

[![Watch the VTS Transcription demo](https://img.youtube.com/vi/gti8B8Q3XpA/maxresdefault.jpg)](https://www.youtube.com/watch?v=gti8B8Q3XpA)

If you'd rather see it before doing it, the demo above walks through the full install + first prompt end to end.

---

## Before you start

- A **VTS account.** If you don't have one, paste a YouTube link at [vts.askgiya.com](https://vts.askgiya.com) — magic-link sign-in the first time you need a paid feature (or after your 300 free minutes for the month).
- A **ChatGPT account** with custom-connector support. Free, Plus, Team, and Enterprise plans all qualify on a current build.
- For the Desktop path: [ChatGPT Desktop](https://chatgpt.com/download) installed (macOS or Windows).

That's it. No keys to copy around.

---

## Option 1: ChatGPT.com (browser)

### Step 1 — Open Settings → Connectors

Click your avatar (top-right) → **Settings** → **Connectors** in the sidebar.

You'll see the list of available connectors (Google Drive, GitHub, etc.) and an **Add custom connector** button.

> **Don't see "Connectors"?** Custom connectors rolled out in waves. If the section isn't visible on your account, try switching to a paid plan or check OpenAI's release notes — the docs flow itself doesn't change once it shows up.

### Step 2 — Paste the VTS connector URL

Click **Add custom connector** and enter:

- **Server URL** — `https://vts.askgiya.com/mcp`
- **Name** — `VTS Transcription`

Click **Add**. A new connector card appears in your list with a **Sign in** link.

### Step 3 — Approve the OAuth sign-in

Click **Sign in**. A popup opens pointing at `https://vts.askgiya.com/oauth/authorize?…`.

Sign in with your VTS email (magic-link if needed), click **Approve**, and the popup closes.

Open any chat and paste:

```
Transcribe this and summarize the three main points:
https://www.youtube.com/watch?v=dQw4w9WgXcQ
```

You'll see "Using VTS Transcription…" briefly, then a transcript card appears with the video thumbnail and the summary.

---

## Option 2: ChatGPT Desktop

### Step 1 — Open Settings → Connectors

Click your name (or the gear icon) → **Settings** → **Connectors**.

### Step 2 — Paste the VTS connector URL

Click **Add custom connector** and enter:

- **Server URL** — `https://vts.askgiya.com/mcp`
- **Name** — `VTS Transcription`

Click **Add**. The connector card appears with a **Sign in** link.

### Step 3 — Approve the OAuth sign-in

Click **Sign in**. ChatGPT Desktop opens the VTS authorization page.

Sign in if needed, click **Approve**, and the popup closes. Start a new chat:

```
Transcribe this and pull the three best quotes:
https://www.youtube.com/watch?v=...
```

---

## Deep Research mode

ChatGPT's **Deep Research** picks up VTS automatically as a searchable source via the `search` and `fetch` tools — same connector, no extra setup.

When you ask Deep Research to "look through my transcripts for X," it queries your VTS library, pulls the matching transcript, and cites it inline.

---

## How long does the sign-in last?

VTS issues refresh tokens, so the connector stays signed in for **60 days** without re-prompting. Access tokens rotate hourly behind the scenes; the sign-in popup only appears once, on first consent.

If you see the sign-in popup more often, you're on an older build that predates refresh-token support — update ChatGPT.

---

## Confirming it's wired up

On the connector page you'll see a **green check** or "Connected" badge next to the VTS row. In a chat, the first response that uses VTS shows "Using VTS Transcription" above the answer.

If nothing happens when you paste a link, see [troubleshooting.md](troubleshooting.md#nothing-happens-when-i-paste-a-link).

---

## What if voice mode is open?

Voice mode in ChatGPT is its own pipeline (live mic → transcript) and doesn't call custom connectors. If you want to transcribe a YouTube link, switch back to text mode for that prompt.

---

## Next

- [Tools reference](tools.md) — what each VTS tool does
- [Prompt cookbook](prompts.md) — copy-pasteable prompts
- [Creator workflows](creator-workflows.md) — podcast → show notes, interview → article
- [Troubleshooting](troubleshooting.md)
