---
name: youtube-forge
description: Use when creating devlog videos, recording workflow, building video pipelines, or turning development sessions into publishable content. Triggers on video production, devlog, screen recording pipeline, content creation from code sessions, "I want to make a video about this."
---

# YouTube Forge

Local-first devlog production pipeline. Zero cost per video. Turn development sessions into publishable content.

## When This Fires

- "I want to make a devlog"
- "How do I record and edit this?"
- "Turn this session into a video"
- "I need a video pipeline that costs nothing"
- Any content creation from development work

Do NOT fire on:
- Streaming setup (different workflow)
- Non-dev content (cooking videos, vlogs)
- Paid editing software workflows

## Philosophy

Every solo dev has the same problem: you build cool stuff but never show it. The friction isn't the building — it's the production pipeline. YouTube Forge removes that friction.

**Principles:**
- $0/video — no SaaS, no subscriptions, all local tools
- Pipeline over perfection — ship a rough video, don't polish forever
- Dev session IS the content — you're already doing the work, just capture it

## Pipeline

### Phase 1: SCOUT — Plan The Video

Before recording anything:

```
VIDEO PLAN
━━━━━━━━━

Title: [working title, can change]
Hook: [first 15 seconds — why should someone watch?]
Beats:
  1. [what you're building and why]
  2. [the interesting problem]
  3. [the solution / build montage]
  4. [the result — does it work?]
Length target: [5-10 min for devlogs]
```

Rules:
- Hook answers "why should I care?" in under 15 seconds
- 3-5 beats maximum — this is a devlog, not a documentary
- End on the result, not a cliffhanger (unless it's a series)

### Phase 2: CAPTURE — Record Everything

**Screen recording (free tools):**
- OBS Studio — screen + mic + webcam
- ShareX — quick clips and GIFs
- Built-in OS tools (Win+G on Windows, built-in on Mac)

**Audio:**
- Record narration separately if possible (cleaner edit)
- Any USB mic beats laptop mic
- Record in a quiet room > expensive mic in a noisy room

**Capture strategy:**
```
1. Start OBS before you start coding
2. Narrate what you're doing as you do it (even roughly)
3. If you forget to narrate, that's fine — add voiceover later
4. Record 2-3x your target length — you'll cut it down
5. Save raw footage locally (never trust cloud-only)
```

### Phase 3: TRANSCRIBE — Get Text From Audio

Free transcription:
- Whisper (OpenAI, runs locally) — best quality, free
- YouTube's auto-captions — upload unlisted, download transcript

```bash
# Whisper locally (one-time install)
pip install openai-whisper
whisper recording.mp4 --model base --output_format txt
```

Why transcribe:
- Lets you find the good parts without rewatching
- Transcript becomes video description / blog post
- Captions = accessibility + SEO

### Phase 4: SCENE MAP — Structure The Edit

Read the transcript. Mark timestamps for each beat:

```
SCENE MAP
━━━━━━━━

00:00-00:15  Hook — "I built X in one session"
00:15-02:30  Context — what the project is, why this feature
02:30-06:00  Build — screen recording of the work
06:00-07:30  Problem — the bug / unexpected challenge
07:30-09:00  Solution — how it got fixed
09:00-10:00  Result — demo of the working feature
10:00-10:30  Outro — what's next
```

Cut list:
- Mark dead air, tangents, debugging rabbit holes
- Keep mistakes that are educational
- Cut mistakes that are just typos

### Phase 5: EDIT — Assemble The Video

**Free editors:**
- DaVinci Resolve — professional grade, free tier is more than enough
- Kdenlive — open source, Linux/Windows/Mac
- Shotcut — simple, fast, open source

**Edit workflow:**
```
1. Import raw footage
2. Rough cut — remove dead air and tangents (get to target length)
3. Add title card (text on solid background is fine)
4. Add captions if you have them
5. Normalize audio levels
6. Export at 1080p (4K not needed for screen recordings)
```

**What you DON'T need:**
- Fancy transitions (cut is fine)
- Background music (optional, adds copyright risk)
- Animated intros (wastes viewer time)
- Color grading (it's a screen recording)

### Phase 6: PUBLISH — Ship It

**Title:** specific > clever. "Building X with Y" beats "You Won't Believe What I Built"

**Description template:**
```
[One sentence: what you built]

[Timestamp list from scene map]

[Links to repo/project]

[Tools used]
```

**Thumbnail:** screenshot of the result with large readable text. Canva (free) or just a screenshot with annotation.

**Tags:** technology name, "devlog", "indie dev", "building in public"

## Batch Production

Once the pipeline is established, batch it:

```
Week 1: Record 3 sessions naturally while working
Week 2: Transcribe + scene map all 3 (1-2 hours)
Week 3: Edit all 3 (2-3 hours)
Week 4: Publish one per week while recording next batch
```

This keeps a consistent upload schedule without constant context-switching between coding and editing.

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Waiting for perfection | Ship rough. Your 10th video > your perfect 1st |
| Recording without a plan | 5-minute scout saves 2 hours of editing |
| Over-editing | Devlogs should feel authentic, not produced |
| No hook | First 15 seconds decide if they stay |
| Skipping captions | Massive accessibility and discoverability loss |
| Trying to be entertaining | Be useful. Entertainment follows expertise |

## File Organization

```
videos/
  2026-04-03-feature-name/
    raw/              Original recordings
    transcript.txt    Whisper output
    scene-map.md      Timestamp structure
    edit/             Project files
    export/           Final render
    assets/           Thumbnails, title cards
```

One folder per video. Everything self-contained. Nothing floating loose.

## Rules

1. **Plan before recording.** 5-minute scout saves hours.
2. **Record more than you need.** Cutting is easier than reshooting.
3. **Transcribe everything.** Text is searchable, video isn't.
4. **Ship rough over perfect.** Consistency beats quality early on.
5. **Local-first always.** Your footage lives on YOUR drive, not someone's cloud.
6. **One video = one topic.** Don't cram three features into one devlog.
