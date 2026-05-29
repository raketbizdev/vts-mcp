# Creator workflows

End-to-end workflows that real content people run on top of VTS + Claude / ChatGPT. Each one is a single prompt (or short prompt chain) you can copy, plus the rationale for why this shape works.

These chain the VTS tools (`transcribe_url`, `search`, `get_format`, etc.) without you having to name them — the AI picks. Replace `<url>` with your YouTube or Facebook link.

---

## Podcast hosts

### Episode → show notes + chapters

**Workflow:** Transcribe with speaker labels, generate timestamped chapters from topic shifts, write show notes in your voice, surface 3 pull-quotes for social.

**Prompt:**

```
Transcribe this podcast episode with speaker labels:
<url>

Then:
1. Generate 5–8 timestamped chapter markers (HH:MM:SS — chapter title).
   Pick chapter boundaries at clear topic shifts, not arbitrary intervals.
2. Write show notes:
   - 2-sentence hook (what's this episode about + why it matters)
   - Guest bio paragraph (pull anything mentioned in the intro)
   - The chapter list from step 1
   - 3 pull-quote candidates, each with timestamp + 1-line context
3. End with a clear CTA line I can edit ("Subscribe", "Leave a review", etc.).

Match my podcast's voice: conversational, opinionated, no marketing-speak.
```

**Why it works:** The AI does the boring stitching (timestamping, finding the natural breaks) so you keep the creative judgment (which quote actually lands, what the hook should be).

---

### Episode → 1-week content calendar

**Prompt:**

```
Transcribe this episode: <url>

Now turn it into 7 posts:
- Day 1: announcement (LinkedIn, 200 words, one statistic from the episode)
- Day 2: pull-quote graphic — give me the quote + alt-text + 2 design directions
- Day 3: 60-second TikTok script using the most surprising moment
- Day 4: X/Twitter thread of 5 single-sentence takeaways
- Day 5: 1200-word blog post with H2 sections matching the chapter structure
- Day 6: behind-the-scenes post (one thing I'd correct or rethink)
- Day 7: CTA post (link to the full episode + my notes)

Use the transcript's actual quotes. Don't invent anything.
```

**Tip:** Run this against your last 4 episodes and you've got a month of content from work you already did.

---

## YouTube creators

### Long-form video → script for a 60-second short

**Prompt:**

```
Transcribe this long-form video: <url>

Find the single most "stop-the-scroll" moment — the line that, on its own,
would make someone pause and watch.

Then write me a 60-second TikTok/Reels/Shorts script built around it:
- 0–3s: the hook line, delivered cold, no setup
- 3–15s: context — why this matters (use real lines from the video)
- 15–50s: payoff — the insight or punchline
- 50–60s: CTA — what to do next (no shilling, just one ask)

Output as a shot list: line of dialogue, suggested B-roll, on-screen text
overlay for each beat.
```

**Why it works:** Most creators bury the lede in long videos. The AI is good at finding it, freeing you to nail the delivery.

---

### Interview video → SEO blog post

**Prompt:**

```
Transcribe this interview: <url>

Write a blog post that targets the obvious search intent from the title:
- Title: under 60 chars, leading with the primary keyword
- Meta description: under 160 chars, value prop in one sentence
- Opening 2 paragraphs that answer the search query directly
- 5 H2 sections matching the main themes from the interview
- One paragraph per H2, weaving in real quotes (with the speaker's name + a paraphrased lead-in)
- A short FAQ section with 4 questions that come up naturally in the interview
- Closing paragraph with one clear next step for the reader

Voice: plain US English. No "in today's fast-paced…", no "let's dive in",
no em-dash carpet-bombing. Write like a knowledgeable human, not a content mill.
```

---

## Video editors

### Auto-caption a video for client review

**Prompt:**

```
Transcribe this and give me the SRT only — no preamble, no chat,
just the SRT content I can save to disk:
<url>
```

**Why this works:** When you just need the file, kill the chat ceremony so a copy-paste lands clean in your text editor.

For longer files where word-level timing matters:

```
Transcribe this with timestamps. Give me a markdown table:
| Timestamp | Speaker | Line |
That makes it easy to scrub and find the cut points.
<url>
```

---

### Find the exact moment a speaker said something

You're editing a documentary and need to find the line "we never thought it would work" — but you don't remember which interview tape it's in.

**Prompt:**

```
Search my transcripts for the phrase "we never thought it would work"
or close variations. For each match, show me:
- transcript title
- speaker (if labeled)
- the line with 30 seconds of context before and after
- the timestamp

Sort by best match.
```

---

## Journalists

### Public-figure interview → article-ready notes

**Prompt:**

```
Transcribe this interview: <url>

Now help me write:
1. Three lede paragraph options — different angles I could lead with.
2. Five pull-quote candidates, each one a single sentence I could
   put in a sidebar. Include timestamps for fact-checking.
3. Every named person, organization, place, and figure (numbers/dates)
   mentioned, with timestamps for the first mention. Group by category.
4. The single most surprising or newsworthy claim, paraphrased.
5. The follow-up question I should have asked but didn't, based on
   what's missing or contradictory.

If you spot a claim that's worth fact-checking, flag it with [check].
```

**Why this works:** The AI is fast at extraction and slow at judgment — perfect for the boring 90% so you can spend your time on the angle.

---

### Foreign-language source for an English article

**Prompt:**

```
Transcribe this Spanish-language video: <url>

Then:
1. Give me the original Spanish transcript with timestamps.
2. Translate to English, paragraph-by-paragraph (Spanish above, English below).
3. Highlight any spots where the translation is non-literal because of an
   idiom or cultural reference. Flag with [idiom: literal meaning vs
   intended meaning].
4. Pull the three most quotable lines as: "English translation"
   (Spanish original, HH:MM:SS).
```

---

## Students & researchers

### Lecture → study notes

**Prompt:**

```
Transcribe this lecture: <url>

Now turn it into study notes:
- 3-bullet TLDR at the top
- Key concepts defined inline (when the lecturer defines a term, copy
  the definition verbatim with timestamp)
- Sub-headings matching the lecture's structure
- A short glossary of every domain-specific term used
- 5 likely exam questions based on the topics emphasized
```

---

### Research interview → coded transcript

**Prompt:**

```
Transcribe this qualitative-research interview: <url>

Then:
1. Verbatim transcript with speaker labels (R: researcher, P: participant).
2. Identify 4–6 themes that emerge from the participant's answers.
   For each theme: a 1-sentence definition, then 2 quoted excerpts
   (verbatim, with timestamps) that exemplify it.
3. A short methods note describing what kind of coding pass this is
   (this is the equivalent of a first-pass inductive coding).
4. Flag any moments where the participant gave a notably different
   answer than the question was asking for — those are often the
   most interesting findings.
```

---

## Workflow-of-workflows

If you're running these regularly, two tips that compound:

### 1. Use VTS Projects (Plus and above on ChatGPT, custom system prompts on Claude)

Pin one of the prompt shapes above as a project / system prompt. Every transcript you bring into that project automatically gets the same treatment — you don't re-type the recipe each time.

### 2. Build a transcript library you can search

After 10–20 transcriptions, the `search` tool becomes powerful. You can ask "find the interview where X talked about Y" and the AI threads it without you remembering which file it was. The library lives at [vts.askgiya.com/dashboard](https://vts.askgiya.com/dashboard); transcripts there are searchable from any chat.

---

## See also

- [Prompt cookbook](prompts.md) — single-step prompts you can mix and match
- [Tools reference](tools.md) — what each VTS tool returns
- [Pricing](pricing.md) — how much these workflows actually cost
