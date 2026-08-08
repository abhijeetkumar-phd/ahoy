# Prompt Directory — AI artwork to fill the remaining placeholders

Everything I could draw myself (the Mithila-inspired lattice border, the Kerala palm silhouette, the wax seal integration) is already built into the site. What's below is genuine illustration/painting work that a text-to-image model (ChatGPT/DALL·E, Gemini/Imagen, Midjourney) will do far better than hand-coded SVG. Run these prompts, save the result with the exact filename listed, drop it in the `assets/` folder next to `index.html`, and it'll slot straight in (see EDITING_GUIDE.md for exactly which line to point at the file).

Style anchor for all prompts below — paste this alongside each one so every image feels like part of one family:
> Style reference: flat, elegant editorial illustration, engraved linework in the spirit of a wax-seal monogram (deep maroon #6d1b24, temple gold #c8a13c, backwater blue-green #134a52, ivory #faf9f6 background). No pure black anywhere — use a warm deep brown instead. No photorealism, no literal clip-art, no text/lettering baked into the image.

---

## 1. Journey Map Illustration
**Where it goes:** the new "Journey" section, `data-placeholder="journey-map-illustration"`, save as `assets/journey-map.png` or `.jpg`, roughly 1600×900px (16:9).

**Prompt:**
> A minimalist, elegant illustrated map of India showing only Bihar, Kerala, and Karnataka highlighted, connected by a single hand-drawn dotted travel line running Patna → Kerala → Bengaluru, with a small line-art paper boat or oar icon marking the route (echoing a boat/oar motif — do not include any text or labels). Flat vector illustration style, warm ivory background (#faf9f6), route line and state outlines in deep maroon (#6d1b24) and temple gold (#c8a13c), no pure black, no photorealism, generous negative space, wedding-invitation quality linework.

---

## 2. Hero Backdrop Art (optional, subtle — currently the hero uses a soft gradient + the wax seal watermark, which already looks clean; only add this if you want more atmosphere)
**Where it goes:** add as a full-bleed background image behind the hero text. Save as `assets/hero-backdrop.jpg`, ~2400×1600px. See EDITING_GUIDE.md for the one CSS line to activate it.

**Prompt:**
> A very soft, pale watercolor wash suggesting an open sky and horizon line at dawn, almost abstract, extremely light and airy so text can sit on top of it comfortably. Palette: ivory, pale gold, the faintest wash of maroon at the edges. No figures, no objects, no text — just atmosphere and light. Minimalist editorial wedding-invitation background art.

---

## 3. Patna Celebration Panel Backdrop (optional — currently a solid maroon gradient with a gold Mithila-style border, which already reads as intentional; add this only if you want more texture)
**Where it goes:** background layer behind the Haldi/Mehndi/Sangeet and Wedding Ceremony panels. Save as `assets/patna-backdrop.jpg`, ~2000×1400px.

**Prompt:**
> An abstracted illustration inspired by Mithila (Madhubani) folk art patterns — geometric florals, fish, and sun motifs rendered as fine gold linework on a deep maroon background, very subtle and low-contrast so it can sit behind text, more texture than illustration. No text, no literal deities or figures, tasteful and abstract, wedding-invitation quality.

---

## 4. Bengaluru / Kerala Celebration Panel Backdrop (optional — currently a light blue gradient with a palm silhouette + ripple border already built in)
**Where it goes:** background layer behind the Wedding & Reception panel. Save as `assets/blr-backdrop.jpg`, ~2000×1400px.

**Prompt:**
> A soft, minimal illustration of Kerala backwaters at dusk — gentle ripples, a distant row of coconut palms as flat silhouettes, one small traditional houseboat far in the distance. Muted backwater blue-green (#134a52) and ivory tones, very low contrast, almost more mood than scene, meant to sit quietly behind text. Flat illustration, no text, no photorealism.

---

## 5. "Our Story" Video — intro/opener (if you want a short AI-generated video clip instead of just a placeholder box)
**Where it goes:** replace the placeholder box in the video section — see EDITING_GUIDE.md.

**Prompt (for a video model like Gemini Veo, Sora, or similar):**
> A slow, gentle 8–10 second cinemagraph-style loop: a single paper boat drifting on calm water at golden hour, soft ripples trailing behind it, camera very slightly drifting forward. Warm, minimal, elegant — like the opening shot of a boutique wedding film. No text, no people, no logos. Color palette: warm gold light, deep teal-blue water, soft ivory highlights.

*(If a video model isn't available to you, a still image from the same prompt works fine as a poster frame for the video placeholder.)*

---

## Not in this list on purpose
Your own photos and videos (bride/groom portraits, gallery images, the real "our story" footage) shouldn't be AI-generated — those placeholders are simply waiting for your real files. Search `data-placeholder` in `index.html` to find every one of those slots.
