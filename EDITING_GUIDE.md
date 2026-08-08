# Editing Guide — how to change anything on the site

Everything lives in one file, `index.html`. Open it in any text editor (VS Code, Notepad, TextEdit — even GitHub's own web editor if you've uploaded it to a repo). No coding tools required for the changes below; you're just changing text between existing tags.

## 1. Text — names, dates, venues, copy
All the visible wording is plain text inside the HTML, in the same order as the page reads top to bottom:
- Hero tagline, dates → search for `hero-sub` and `hero-date-chip`
- Welcome paragraphs → search for `id="welcome"`
- Bride/groom bios → search for `id="couple"`
- Celebration details (venue, time, dress code) → search for `id="event1"`, `id="event2"`, `id="event3"`
- Farewell message → search for `farewell-line`

To edit: find the sentence, select the text between the `>` and `<` (never touch the tags themselves), and type your replacement.

## 2. Dates, countdowns, and links
All of these live together near the bottom of the file, inside `<script>`, in a block labeled:
```
const CONFIG = { ... }
```
- `eventDates` → the three countdown targets
- `photoUploadFormUrl` → your Google Form link for guest photo uploads
- `rsvp.actionUrl` and `rsvp.entries` → your RSVP Google Form (see SETUP_GUIDE.md for how to generate these)

Change only the text inside the quotes `" "`, nothing else.

## 3. Photos and videos
Every photo/video slot is tagged so you can find it instantly. In your editor, search for:
```
data-placeholder
```
Each match is one slot — bride photo, groom photo, each celebration photo, the 15 gallery photos, the story video, and the journey map. The search result tells you exactly which one it is (e.g. `data-placeholder="bride-photo"`).

To swap a **photo**, replace the whole `<div class="photo-slot">...</div>` (or `.celeb-photo-slot` / `.map-slot`) block with:
```html
<img src="assets/your-photo.jpg" alt="" style="width:100%; border-radius:6px;">
```
Put your image file in the `assets/` folder next to `index.html` first, and reference it by that filename.

For the **15 gallery polaroids**, they're generated automatically by a small script (search `galleryGrid` near the bottom) — easiest path is to keep the placeholders until you have all 15 photos ready, then I (or you, following the same pattern) can swap the loop for real filenames in one go.

To swap the **video**, see the comment directly below the video placeholder in the HTML — it shows the exact `<video>` tag to paste in.

## 4. Colors, fonts, motifs
All colors are defined once at the top of the file inside `:root { ... }` — change a hex code there and it updates everywhere it's used. Same for fonts, just below.

## 5. Optional artwork layers (backdrop art, video)
A few enhancements are built in but switched off by default (a hero backdrop image, textured backgrounds behind the Patna/Bengaluru panels). Each one has a comment right above it in the CSS explaining the exact line to uncomment once you have the artwork — see `PROMPT_DIRECTORY.md` for prompts to generate that artwork with ChatGPT or Gemini.

## 6. If something breaks
The most common issue is an unmatched `<` or `>` or a missing quote after editing text. If the page stops rendering correctly, undo your last change and try again more narrowly — change only the words between tags, never the tags themselves.

## 7. Publishing your changes
If you're hosting on GitHub Pages or Netlify (see SETUP_GUIDE.md), just re-upload the edited `index.html` (and `assets/` folder if you added images) — the live site updates automatically.
