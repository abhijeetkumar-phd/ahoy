# Setup Guide — connecting RSVP + photo uploads + going live

This site is a single static file (`index.html`) with no backend of its own — everything it needs (fonts, QR code library) loads from public CDNs, so it's ready for free static hosting as-is.

## Publish it — GitHub Pages (recommended, free, your own `username.github.io` link)
1. Create a new GitHub repo (public), e.g. `honey-abhijeet-wedding`.
2. Upload `index.html` to the repo root (drag-and-drop on github.com works fine).
3. Go to **Settings → Pages** → under "Build and deployment", set **Source: Deploy from a branch**, **Branch: main / (root)** → Save.
4. Your site goes live in a minute or two at `https://<your-username>.github.io/honey-abhijeet-wedding/`.
5. Optional custom domain: add a `CNAME` file with your domain, or set it under Settings → Pages → Custom domain.

## Or — Netlify (drag-and-drop, also free)
1. Go to **app.netlify.com** → **Add new site → Deploy manually**.
2. Drag the folder containing `index.html` onto the upload area.
3. Netlify gives you a live link immediately (e.g. `honey-abhijeet.netlify.app`); you can rename the subdomain or attach a custom domain under Site settings.

Either way: whenever you update `index.html` (swap a placeholder photo, fix a date), just re-upload/re-drag the file and the live site updates.

---


Your website is fully built and styled. Two pieces need a one-time setup on **your** Google account, because they touch your Drive/Forms directly. Takes about 10–15 minutes total.

---

## Part 1 — RSVP form (Google Form, submits invisibly from your site)

The RSVP on your site is fully custom-styled (pill buttons, not checkboxes), but it needs a real Google Form behind it to actually collect responses.

1. Go to **forms.google.com** → create a blank form, e.g. "AHHA Wedding RSVP".
2. Add these questions, **in this order**, matching these types:
   | Question | Type |
   |---|---|
   | Your name | Short answer |
   | Email | Short answer |
   | Phone number | Short answer |
   | Which celebration(s) will you join? | **Checkboxes** — add options: `Haldi/Mehndi/Sangeet — Patna, 24 Nov`, `Wedding Ceremony — Patna, 25 Nov`, `Wedding & Reception — Bengaluru, 12 Dec` |
   | Bringing a guest? | Checkboxes or Multiple choice — options `Yes`, `No` |
   | Will you need accommodation arranged? | Multiple choice — options `Yes`, `No` |
   | Anything else you'd like to tell us? | Paragraph |
3. Click the **Send** button → click the **link icon** (🔗) → copy the link. It looks like:
   `https://docs.google.com/forms/d/e/1FAIpQLSxxxxxxxxxxxx/viewform`
4. To get the entry IDs your site needs:
   - Open that link in a normal browser tab.
   - Fill in dummy/junk answers to every question and hit Submit **once**.
   - Right after submitting, Google shows "Get pre-filled link" — click it, fill the form again with placeholder text, click **Get link**, then **copy link**. It'll look like:
     `...viewform?usp=pp_url&entry.111111111=John&entry.222222222=test%40email.com&entry.333333333=...`
   - Each `entry.XXXXXXXXX=` right before your dummy answer is the ID for that question, in order.
5. Open `index.html`, find the `CONFIG.rsvp` block near the bottom, and replace:
   - `actionUrl` → your form's `.../formResponse` URL (same as your form link, but ending in `/formResponse` instead of `/viewform`)
   - each `entry.111111111` placeholder → the real entry ID for that question
6. Delete your test submission from the form's **Responses → spreadsheet icon** so your response sheet starts clean.

That's it — your styled pills submit straight into this Google Form/Sheet in the background; guests never see the plain Google Forms UI.

---

## Part 2 — Photo/video uploads, auto-sorted by uploader name

You want: guests upload via QR → files land in **one Drive folder**, but automatically organized into a **subfolder per uploader's name** (for later photo-credit attribution) — up to 50 uploads per person.

**Step A — Create the upload form**
1. In Google Forms, create a new form, e.g. "Share your photos — AHHA Wedding".
2. Add:
   - "Your name" — Short answer (required)
   - "Upload your photos/videos" — **File upload** question → set **max 50 files**, allow image + video types, max file size as needed.
3. This automatically creates a Drive folder (Forms does this for you) where every upload lands — but all mixed together, not sorted by name yet. That's what the script below fixes.

**Step B — Add the sorting script**
1. In your form, click the **⋮ (more)** menu → **Script editor** (or go to script.google.com → New project, then link it to this form via Triggers).
2. Paste this:

```javascript
function onFormSubmit(e) {
  const itemResponses = e.response.getItemResponses();
  let uploaderName = "Unknown Guest";
  let fileIds = [];

  itemResponses.forEach(function(item) {
    const title = item.getItem().getTitle();
    if (title.indexOf("name") !== -1) {
      uploaderName = item.getResponse();
    }
    if (title.indexOf("Upload") !== -1) {
      fileIds = item.getResponse(); // array of file IDs for file-upload questions
    }
  });

  // Root folder where ALL guest uploads should end up (your single Drive folder)
  const rootFolder = DriveApp.getFolderById("YOUR_ROOT_DRIVE_FOLDER_ID");

  // Find or create a subfolder named after the uploader
  const safeName = uploaderName.toString().trim() || "Unknown Guest";
  let subFolder;
  const existing = rootFolder.getFoldersByName(safeName);
  subFolder = existing.hasNext() ? existing.next() : rootFolder.createFolder(safeName);

  // Move each uploaded file into that subfolder
  fileIds.forEach(function(fileId) {
    const file = DriveApp.getFileById(fileId);
    subFolder.addFile(file);
    // Files also live in Forms' default upload folder — remove them from there so they don't duplicate:
    file.getParents().forEachRemaining(function(parent) {
      if (parent.getId() !== subFolder.getId()) parent.removeFile(file);
    });
  });
}
```

3. Replace `YOUR_ROOT_DRIVE_FOLDER_ID` with the ID from your Drive folder's URL (the string after `/folders/`).
4. Click **Triggers** (clock icon on the left) → **+ Add Trigger** → choose function `onFormSubmit`, event source **From form**, event type **On form submit** → Save. Authorize the script when prompted (it's your own script on your own account, safe to allow).
5. Test: submit the form once with a photo — you should see a subfolder appear in your Drive folder named after whatever you typed as "Your name," containing that photo.

**Step C — Connect it to your website**
1. Get the upload form's shareable link (Send → link icon).
2. In `index.html`, find `CONFIG.photoUploadFormUrl` and replace the placeholder with this link.
3. The QR code on your site regenerates itself automatically from that link — no need to create the QR image separately.

---

## Quick checklist before publishing
- [ ] RSVP `actionUrl` + all 7 `entry.XXXXXXXXX` values replaced
- [ ] `photoUploadFormUrl` replaced with your real upload form link
- [ ] Apps Script root folder ID set + trigger added + tested with one real submission
- [ ] Swap in real photos: search `data-placeholder` in `index.html` to find every image/video slot (bride, groom, each celebration, story video, 15 gallery photos)
- [ ] Double-check event dates/times in `CONFIG.eventDates` at the top of the script block
