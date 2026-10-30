---
name: reel-edit
description: Use whenever the user asks to edit, cut, or build a short vertical video (Reel, TikTok, Short) from their own clips and a voiceover. Works for any niche. Interviews first, no filters, previews before rendering.
---

# Reel Edit Recipe (blank slate, any niche)

The job: turn the user's approved clips + their voiceover into a ready-to-post vertical video with the same look every time. You handle the tedious steps. The user makes every taste call.

Fill in the "Your Style Settings" section at the bottom once. Until then, ask the user for each setting.

---

## Step 0 - Interview first (always)

Before touching any file, ask these as multiple choice, in one message:

1. Which voiceover file? (list the audio files you find)
2. Hook text for frame 1? (the user types it, max 7 words)
3. Which clips? (A: you suggest from the Clip Library, B: the user gives numbers)
4. Target length? (A: 7-15s, B: 15-30s, C: 30-45s)

Do not start editing until all 4 are answered.

---

## Step 1 - Clips

- Use ONLY clips from the Clip Library at the bottom. Never pull other footage.
- Use the exact in/out times listed (for example "0-1.25s" means start at 0s, end at 1.25s).
- Make a contact sheet (one still frame per chosen clip on a numbered grid) and show it. Wait for "approved" or a changed list.
- Skip any clip with glare, blur, or shaking, even if it is in the library. Say which one and why.
- Clip order follows the voiceover: the clip on screen should match what is being said.

---

## Step 2 - Color: NO FILTERS

- Do not add color grading, LUTs, saturation, contrast, brightness, sharpening, vignette, film grain, or any "look." No eq, curves, colorbalance, hue, or lut3d filters.
- The only video filters allowed: scale, crop, trim, fps, format, subtitles, and the HDR fix below.
- Footage should look exactly like it did on the phone.

### The HDR fix
Phones record HDR (high dynamic range) video by default. If it is converted the wrong way, it comes out grey, washed-out, or oddly colored.

Check every clip first:
```
ffprobe -v error -select_streams v:0 -show_entries stream=color_transfer -of default=nw=1:nk=1 "CLIP.MOV"
```
- If it says `arib-std-b67` or `smpte2084`, it is HDR. Convert it with this, and nothing else:
```
zscale=t=linear:npl=100,format=gbrpf32le,zscale=p=bt709,tonemap=tonemap=hable:desat=0,zscale=t=bt709:m=bt709:r=tv,format=yuv420p
```
- If it says `bt709` or anything else, it is normal video. No color filter at all.
- After converting, show one still frame next to the original frame so the user can confirm the colors match.

---

## Step 3 - Captions and text

Use the values from "Your Style Settings." If a value is blank, ask the user.

### Hook (frame 1)
- Burned into the very first frame. No fade-in, no animation.
- Max 7 words.
- Style, font, size, position, and duration: see Style Settings.

### Voiceover captions
- Generate with Whisper (a speech-to-text tool) using word-level timestamps from the voiceover.
- Max 4 words on screen at once, 1 line.
- Style, font, size, and position: see Style Settings.
- Check the spelling of names and places against the Clip Library.

### Safe zones (where app buttons sit)
- Keep all text out of the bottom 20% and the right 15% of the frame. Instagram and TikTok put buttons and captions there.

### Preview before rendering
Render ONE still frame with the hook and one with a caption. Show both. Render the full video only after the user says yes.

---

## Step 4 - Audio
- Voiceover only unless the user says otherwise. No music or sound effects by default.
- Normalize voiceover loudness to -14 LUFS (a standard loudness level for social apps).

---

## Step 5 - Export
One file that works on Instagram, TikTok, YouTube Shorts, and Facebook:
```
-vf "scale=1080:1920:force_original_aspect_ratio=increase,crop=1080:1920,fps=30"
-c:v libx264 -preset veryfast -crf 18 -pix_fmt yuv420p
-color_primaries bt709 -color_trc bt709 -colorspace bt709
-c:a aac -b:a 192k -movflags +faststart
```
File name: `[topic]_[YYYY-MM-DD]_v[#].mp4`

---

## Hard rules
- Never post, upload, or schedule anything. Hand over the file only.
- Never invent a number (prices, counts, results) in on-screen text.
- Never use footage that is not in the Clip Library.

---

## Your Style Settings (fill in once)

| Setting | Your value |
|---|---|
| Hook text style | (example: black bold text on a white rounded box) |
| Hook font and size | (example: your font, 72px) |
| Hook position | (example: centered, 30% down from the top) |
| Hook duration | (example: first 2.5 seconds) |
| Caption style | (example: white bold text, 4px black outline) |
| Caption font and size | (example: your font, 60px) |
| Caption position | (example: centered, 65% down from the top) |
| Words not allowed in text | (add any) |

---

## Clip Library (add your own approved clips)

| # | Label | File | Use (in-out seconds) |
|---|---|---|---|
| 1 | Example clip A | (your file name) | 0-2s |
| 2 | Example clip B | (your file name) | 0-1.5s |

On hold (not approved): list clips here and never use them until the user confirms.
