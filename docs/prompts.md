# Prompt cookbook

Thirty-plus copy-pasteable prompts you can use once the VTS connector is installed in Claude or ChatGPT. Replace `<url>` with your YouTube or Facebook link. The AI picks the right VTS tool based on what you ask — you don't have to name the tool.

Use any of these as-is or as a starting point for your own workflow.

---

## Quick wins (the basics)

```
Transcribe this and summarize the three main points:
<url>
```

```
Transcribe this with speaker labels and show me the diarized transcript:
<url>
```

```
Transcribe this and pull the five best quotes, each with a timestamp:
<url>
```

```
What's the gist of this video? Don't dump the transcript — just give me the takeaways:
<url>
```

```
Transcribe this and give me an SRT file I can drop into Premiere:
<url>
```

---

## Cost checks (before you commit)

```
How much would it cost to transcribe this? <url>
```

```
This is a 3-hour podcast — would it fit in my free tier?
<url>
```

```
Quote me both transcribe and transcribe-with-denoise costs for this:
<url>
```

---

## For content repurposing

```
Transcribe this and turn it into a 1500-word blog post with H2 sections,
a meta description under 160 chars, and two bullet lists:
<url>
```

```
Transcribe this podcast episode and write show notes:
intro hook (2 sentences), guest bio (1 paragraph), 5 timestamped chapters,
3 pull quotes for social, and an outro CTA.
<url>
```

```
Transcribe this YouTube video and rewrite the script as a 60-second
TikTok script — high hook, fast cuts, captions-friendly.
<url>
```

```
Transcribe this interview and write me 10 single-sentence threads
I can post on X, each under 240 characters, no hashtags.
<url>
```

```
Transcribe this and pull the most-quotable line. Give me 5 cover-image
variations (text + visual concept) that lead with that line.
<url>
```

---

## For research & journalism

```
Transcribe this and identify every claim with a specific number
(percentages, dollar amounts, dates). Quote each verbatim with timestamp.
<url>
```

```
Transcribe this and give me an interview summary: who said what,
what was decided, what's the open question.
<url>
```

```
Transcribe this Spanish-language video, translate it to English,
and tell me whether the key claim is supported by what the speaker
actually said:
<url>
```

```
Transcribe this and list every named person, organization, and place
that appears, with timestamps for the first mention.
<url>
```

```
Pull the three sharpest quotes for use in an article. Format as
"Quote (HH:MM:SS) — paraphrased lead-in":
<url>
```

---

## For meetings & calls

```
Transcribe this Google Meet recording (Facebook upload link) and give me:
- 3-bullet summary
- action items by owner
- one follow-up question we should have asked
<url>
```

```
Transcribe this with speaker labels and tell me who talked most
and what their top three concerns were.
<url>
```

```
Transcribe this with speaker labels and write the meeting minutes:
attendees, decisions, action items, next steps.
<url>
```

---

## For language learners

```
Transcribe this Spanish video. Show the original transcript with
English translation underneath each paragraph. Bold any vocabulary
that's above B2 level:
<url>
```

```
Transcribe this French podcast and give me 10 idiomatic phrases
worth memorizing, with example contexts:
<url>
```

```
Transcribe this Japanese interview. Output the romaji transcript,
then a literal English gloss, then natural English.
<url>
```

---

## For songs (when accuracy is hard)

```
Transcribe this music video with denoise enabled — the audio's noisy:
<url>
```

```
Transcribe this and tell me which parts you're least confident about,
based on the confidence score:
<url>
```

> Note: Whisper-class engines struggle with songs and overlapping vocals.
> The `denoise` flag helps; expect lower accuracy than spoken-word audio.
> See [VTS SLA bands](https://vts.askgiya.com/sla) for typical accuracy
> ranges by audio type.

---

## Working with your transcript library

Once you've transcribed a few things, you can ask the AI to search and pull from your library:

```
Find the transcript where I discussed pricing with a customer last month,
and turn the relevant section into a one-page FAQ.
```

```
Search my transcripts for "Q3 strategy" and summarize the three meetings
that come back into a single executive briefing.
```

```
Pull transcript 88 and rewrite it as a script for a 5-minute YouTube short.
```

```
Find all my transcripts that mention "user research" and identify the
five recurring themes across them.
```

---

## Long-running jobs

For very long files (over 30 minutes), the transcription runs in the background and the AI polls until it's done:

```
Transcribe this 4-hour interview. Tell me when it's done and what
you've found — don't paste the full transcript, just the topline.
<url>
```

```
I started a transcription a few minutes ago — is it done yet?
If so, what was the main point?
```

---

## When you run out of credit

```
I'm out of credit — give me a top-up link for $20.
```

```
What's my balance? If I'm low, give me a top-up link.
```

---

## Output shape control

VTS returns the transcript as a markdown blockquote by default (so it word-wraps on mobile). You can override:

```
Transcribe this and give me the plain transcript in a fenced code block
(I'm pasting it into a markdown file):
<url>
```

```
Transcribe this and output ONLY the SRT file content, no preamble:
<url>
```

```
Transcribe this and skip the AI ceremony — just give me the title,
duration, and the three best quotes:
<url>
```

---

## Multi-step recipes

These chain multiple VTS tools across a single conversation:

### Podcast → 1-week content calendar

```
Transcribe this podcast: <url>

Then turn it into a content calendar:
- Day 1: announcement post (LinkedIn, 200 words, one statistic from the episode)
- Day 2: pull-quote graphic (give me the quote + alt-text)
- Day 3: 60-second TikTok script using the most surprising moment
- Day 4: Twitter/X thread of 5 single-sentence takeaways
- Day 5: blog post (1200 words, H2 sections matching the episode chapters)
- Day 6: behind-the-scenes post (1 thing the host got wrong, my correction)
- Day 7: CTA post (link to the full episode + my notes)
```

### Interview transcript → research paper section

```
Transcribe this research interview: <url>

Then:
1. Give me a verbatim transcript with speaker labels (R: for researcher, P: for participant).
2. Identify three themes that emerge from the participant's answers.
3. For each theme, pull two direct quotes (with timestamps) that illustrate it.
4. Draft a 400-word "Findings" section I can drop into an APA-style paper,
   referencing the quotes as P1 (interview, 2026).
```

### YouTube video → SEO blog post

```
Transcribe this YouTube video: <url>

Then write me an SEO blog post:
- Title (under 60 chars, leading with the primary keyword the video targets)
- Meta description (under 160 chars)
- Opening paragraph that answers the search intent in 2 sentences
- 5 H2 sections matching the video's main points
- One paragraph per H2, written in plain US English (no AI tells, no fluff)
- Pull-quote callouts where the video says something specifically quotable
- A short FAQ section with 4 questions from the video's "people also ask"-style follow-ups
```

---

## See also

- [Creator workflows](creator-workflows.md) — full multi-step recipes by persona
- [Tools reference](tools.md) — exact tool inputs / outputs
- [Troubleshooting](troubleshooting.md) — when the AI can't find a tool
