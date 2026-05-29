# Install VTS MCP in Claude

Same three steps on the web and Desktop apps. Both use the **custom connector** flow with OAuth, so there's no API key to copy around. Total time: about 60 seconds.

---

## Before you start

- A **VTS account.** If you don't have one, paste any short YouTube link at [vts.askgiya.com](https://vts.askgiya.com) — it'll prompt you to sign in via magic link the first time you need a paid feature (or after your 300 free minutes for the month).
- A **Claude.ai account** (free or paid both work).
- For the Desktop path: [Claude Desktop](https://claude.ai/download) installed (macOS or Windows).

That's it. No API key, no Python, no `config.json`.

---

## Option 1: Claude.ai (browser)

### Step 1 — Open Settings → Connectors

Click your avatar (top-right) → **Settings** → **Connectors** in the sidebar.

You'll see a list of connectors (Google Drive, Gmail, etc.) and an **Add custom connector** button at the bottom.

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

You'll see "Using VTS Transcription…" briefly, then a transcript card appears in the chat with the video thumbnail and a Plain / Timestamps / SRT picker, followed by the summary.

---

## Option 2: Claude Desktop

Pick this path if you prefer the native app over the browser. Same connector URL, same OAuth, same approval screen.

### Step 1 — Open Settings → Connectors

Click your name (or the gear icon) in the sidebar → **Settings** → **Connectors**.

You'll see the list of available connectors with an **Add custom connector** button at the bottom.

> **No "Add custom connector" button?** Custom connectors require a recent Claude Desktop build. Open **About** and check that you're on the current release, then restart the app.

### Step 2 — Paste the VTS connector URL

Click **Add custom connector** and enter:

- **Server URL** — `https://vts.askgiya.com/mcp`
- **Name** — `VTS Transcription`

Click **Add**. The connector card appears with a **Sign in** link.

### Step 3 — Approve the OAuth sign-in

Click **Sign in**. Claude Desktop opens the VTS authorization page.

Sign in if needed, click **Approve**, and the popup closes. Start a new chat:

```
Transcribe this and pull the three best quotes:
https://www.youtube.com/watch?v=...
```

You'll see "Using VTS Transcription…", then the transcript card + your quotes.

---

## How long does the sign-in last?

VTS issues refresh tokens, so the connector stays signed in for **60 days** without re-prompting. Access tokens rotate hourly behind the scenes; you only see the sign-in popup once, after first consent.

If you start seeing a sign-in popup more often than that, you're probably on an older Claude build that predates refresh-token support — restart or update.

---

## Confirming it's wired up

Open the connector page and look for the **green check** or "Connected" badge next to the VTS row. The first time you paste a URL into a chat, Claude shows "Using VTS Transcription" right above the response.

If nothing happens when you paste a YouTube link, see [troubleshooting.md](troubleshooting.md#nothing-happens-when-i-paste-a-link).

---

## Next

- [Tools reference](tools.md) — what each VTS tool does and what to ask Claude for it
- [Prompt cookbook](prompts.md) — 30+ tested prompts to copy
- [Troubleshooting](troubleshooting.md) — popup-blocked, 404, "Sign in to enable", quota errors
