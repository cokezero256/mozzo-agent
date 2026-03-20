# SOP: Video Editing Standards

> Version 1.0 — March 2026
> Owner: Mauricio / Carolina
> Applies to: All Mozzo editors (JD-A, JD-B, JD-C tiers)

---

## The ContentOps Formula

Every video must score high on all three dimensions:

| Dimension | Weight | What it means |
|-----------|--------|--------------|
| **Music** | 50% | Emotional tone set before a word is spoken |
| **Visual Editing** | 30% | Cuts, pacing, overlays — serves the story |
| **Emotional Connection** | 20% | Viewer feels something by the end |

**If a video is technically clean but emotionally flat, it fails.**

---

## Audio Standards

| Platform | Target LUFS | Peak | Notes |
|---------|------------|------|-------|
| YouTube (long-form) | -14 LUFS | -1 dBTP | YouTube normalizes to -14 |
| Instagram / TikTok | -16 LUFS | -1 dBTP | Mobile-first listening |
| Shorts / Reels | -16 LUFS | -1 dBTP | Same as social |

- Remove all background noise (use RX or Resolve's noise reduction)
- Voice clarity > everything. If the voice isn't clear, the video is rejected.
- Music must be mixed under voice at -18 to -22 LUFS (never competing with voice)

---

## Music Selection Rules

Music is 50% of ContentOps. These rules are non-negotiable:

1. **Match the energy arc** — music should build as the video builds, not stay flat
2. **No generic YouTube library tracks** on JD-A client projects
3. **Approved sources:** Epidemic Sound, Artlist, Musicbed, client-provided
4. **Trading/finance content:** Prefer atmospheric, minimal, slightly tense — not happy/corporate
5. **Never use music that overshadows the presenter's voice**
6. **At least 2 music tracks per long-form video** (one for intro energy, one for main content)

---

## Color Grade Standards

| Client Type | Color Approach |
|------------|----------------|
| Trading educators | Cooler tones, high contrast, clean blacks — professional authority |
| Business coaches | Warmer tones, energetic, punchy — aspirational and motivational |
| Brand/corporate | Match provided brand guide exactly |

**Always:**
- Match the client's established palette from previous videos
- Export thumbnail frame at full resolution before delivery
- Apply LUTs consistently across all videos in a series

---

## Pacing Rules

**Long-form YouTube (10-30 min):**
- No shot longer than 8 seconds without a cut or b-roll overlay
- Dead air (pause > 1.5 seconds) = cut it
- Re-hook every 90-120 seconds with a visual or audio change
- B-roll must be purposeful — no filler stock footage

**Short-form (Reels/Shorts, 30-60 sec):**
- Cuts every 1-3 seconds in the hook (0-10 sec)
- Slightly slower in value delivery (10-50 sec) — let the point land
- Visual variety: minimum 3 different shot types per video

---

## Text Overlay Rules

- **Font:** Match client's brand guide. If none exists, use clean sans-serif (Inter, Montserrat)
- **Size:** Never so small it can't be read on mobile in 1 second
- **Duration:** Text on screen minimum 1.5 seconds, maximum 4 seconds
- **Never use more than 7 words** in a single text overlay
- **Contrasting background:** Always ensure text is readable against the video behind it
- **Captions:** If client requires captions, they must be 100% accurate — no AI captions unreviewed

---

## Thumbnail Frame

Every video delivery MUST include:
- A thumbnail frame identified and exported at full resolution (1920x1080 minimum)
- The frame should show the presenter with a clear, expressive facial expression
- Note the timecode of the thumbnail frame in the ClickUp delivery comment

---

## Pre-Delivery Checklist

Before marking ANY task as "Delivered" in ClickUp:

- [ ] Audio levels match platform standards
- [ ] Background noise removed
- [ ] Color grade applied and consistent with previous videos
- [ ] Music mixed properly (not competing with voice)
- [ ] No dead air or filler pauses
- [ ] B-roll is purposeful, not random stock footage
- [ ] Text overlays are readable, correct, and timed properly
- [ ] Captions reviewed (if applicable)
- [ ] Thumbnail frame exported and attached
- [ ] File exported at correct resolution (1080p minimum, 4K if specified)
- [ ] File named correctly: `[ClientName]_[VideoTitle]_[Date]_v1.mp4`
- [ ] Uploaded to correct client Drive folder
- [ ] ClickUp task moved to "Pending Review" with delivery note

---

## Rejection Policy

A video can be rejected (sent back to editing) for:
- Audio quality issues
- Wrong color grade
- Pacing problems (too slow, dead air)
- Missing thumbnail frame
- Wrong file name or upload location
- Text overlay errors

**Two rejections on the same video** → flag to Carolina for editor review.

---

## Changelog

| Date | Change | Author |
|------|--------|--------|
| March 2026 | Initial SOP created | Mauricio |
