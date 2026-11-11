# 11:11 — Maybe, Someday?

A small, cinematic, single-page web experience: a video dedication that gradually becomes a personal letter.
Built with plain HTML, CSS and JavaScript. No build step, no dependencies, no tracking.

> **Privacy note:** nothing the viewer clicks is stored or sent anywhere. Her answer exists only on her screen.

---

## The experience (chapters)

| # | Chapter | What happens |
|---|---------|--------------|
| 1 | Opening | Animated starfield and a glowing **11:11**. A single button opens the gift. |
| 2 | Video | Your video plays. When it ends: hold, slow zoom, fade to black, then "Well..." |
| 3 | Memories | A swipeable timeline of polaroid-style cards. |
| 4 | Voice | A custom audio player for your recording (never autoplays). |
| 5 | Confession | Paragraphs fade in as she scrolls. |
| 6 | The question | A falling-star sequence, then the question and three equally weighted answers. Nothing is stored or transmitted. |
| 7 | Closing | A quiet ending. |

---

## Project structure

```
project-1111/
├── index.html        <- the whole site (design + code + CONFIG block)
├── README.md         <- this file
├── .gitignore
├── .nojekyll         <- tells GitHub Pages to serve files as-is
└── assets/
    ├── README.txt
    ├── dedication-video.mp4   <- YOU ADD THIS
    ├── voice-message.mp3      <- YOU ADD THIS
    └── memory-01.jpg ...      <- OPTIONAL photos
```

---

## Everything personal lives in one place: `CONFIG`

Open `index.html` and look for the block that starts with `const CONFIG = {` near the top of the `<script>`.

```js
const CONFIG = {
  name: "Pinki",
  video: "assets/dedication-video.mp4",
  audio: "assets/voice-message.mp3",
  memories: [ { when, note, text, photo }, ... ],
  confession: [ "paragraph 1", "paragraph 2", ... ],
  options: [ { label, reply }, ... ]
};
```

| To change... | Edit |
|---|---|
| Her name | `name` (used wherever `{name}` appears in the confession) |
| Video file | `video` |
| Voice recording | `audio` |
| A memory's title, caption, text | the matching item in `memories` |
| A memory photo | set `photo: "assets/memory-01.jpg"` (use `null` for the placeholder) |
| The confession text | the `confession` array, one string per paragraph |
| The three answers and replies | the `options` array |

Smaller lines (opening screen text, chapter headings, closing lines) are plain text inside the HTML of `index.html`; search for the words to find them.
Colours are CSS variables at the top of the `<style>` block (`--navy`, `--gold`, `--ivory`, ...).

**Keep filenames lowercase with no spaces** and make sure they match `CONFIG` exactly. Servers like GitHub Pages are case-sensitive: `Video.MP4` is not `video.mp4`.

## Getting her answer (optional, her choice)

GitHub Pages is a static host with no server, and the site deliberately does **not** track clicks. Instead, after she picks an answer, an optional **"Send my answer"** button can appear. Tapping it opens WhatsApp (or SMS) with a pre-filled message such as `Hey, I opened the 11:11 page. My answer: "I'm not sure. I need some time to think."`. She can edit it and **she** presses send. Nothing is ever sent automatically.

Set it up in `CONFIG`:

```js
reply:{ whatsapp:"919876543210", sms:"", button:"Send my answer", hint:"...", intro:"..." }
```

- `whatsapp`: your number with country code, digits only, no `+`, spaces or dashes.
- `sms`: used only if `whatsapp` is empty, e.g. `"+919876543210"`.
- Leave both empty and the button stays hidden.
- It appears for all three answers, so no option is favoured. She can ignore it, close the page and talk to you later.

## Memory card photos

Each memory in `CONFIG` can have its photo set in one of four ways:

| Setting | Result |
|---|---|
| `photo` left out | card with no picture |
| `photo: null` | labelled "Your photo here" placeholder |
| `photo: "assets/x.jpg"` | one photo |
| `photo: ["assets/a.jpg","assets/b.jpg"]` | swipeable photos on one card |

Currently expected files: `memory-01.jpg` (7th Grade), `memory-03.jpg` (The dream), `birthday-her-01.jpg`, `call-01.jpg`, `durga-01.jpg`, `mysuru-01.jpg` to `mysuru-03.jpg`, `birthday-me-01.jpg` (26th September) and `today-01.jpg`. If a file is missing, the card shows an "Add assets/..." note instead of a broken image. Use `ratio:"1/1"` for portrait photos and the default `4/3` for landscape ones. Resize photos to ~1200 px wide before uploading.

---

## Preparing your files

- **Video:** export as MP4 (H.264 video + AAC audio), 720p or 1080p. Aim for **under ~25 MB** so it can be uploaded straight through the GitHub website and starts quickly on mobile data. HandBrake (free) can compress it; the "Fast 1080p30" preset is a good start.
- **Audio:** MP3, mono or stereo, 96-128 kbps is plenty for a voice.
- **Photos:** JPG, around 1200 px wide, under 500 KB each.

---

## Run it locally (optional)

Opening `index.html` by double-click works for a quick look, but a local server behaves more like the real site:

```bash
cd project-1111
python3 -m http.server 8000
# open http://localhost:8000
```

To test on your phone, connect it to the same Wi-Fi and open `http://<your-computer-ip>:8000`.

---

## Deploy with GitHub Pages

1. Push this folder's contents to a GitHub repository (see below).
2. In the repo go to **Settings -> Pages**.
3. Under **Build and deployment**, set **Source: Deploy from a branch**.
4. Choose **Branch: `main`** and folder **`/ (root)`**, then **Save**.
5. Wait about a minute. The page shows your live link:
   `https://<your-username>.github.io/<repo-name>/`

Every time you commit a change to `main`, the site redeploys automatically (usually within 1-2 minutes).

### Editing later, straight on GitHub

- **Text/config:** open `index.html` in the repo, click the pencil icon, edit, then **Commit changes**.
- **Replace a file** (new video, new photo): open the `assets` folder -> **Add file -> Upload files**, upload a file with the *same name*, and commit. It overwrites the old one.
- After committing, wait a minute or two, then hard-refresh the live page (on phone: close the tab and reopen it).

---

## Troubleshooting

| Problem | Fix |
|---|---|
| A dashed "placeholder" box appears instead of the video/audio | The file is missing or its name/case doesn't match `CONFIG`. |
| Changes don't show up | Wait 1-2 minutes after committing, then hard-refresh. Check **Actions** tab for the Pages deployment status. |
| Video won't upload on github.com | The browser upload limit is 25 MB per file. Compress it, or use GitHub Desktop / git (limit 100 MB). |
| Photo doesn't show | Check the path and filename case in `CONFIG`. |
| Volume fade doesn't happen on iPhone | iOS ignores script volume changes. The rest of the ending works normally. |

---

## Notes

- A **public** repository means anyone who finds it can read `index.html`, including the confession text. The page is marked `noindex` so search engines are asked to skip it, but it is not secret. See the setup guide for options.
- Fonts (Cormorant Garamond, DM Sans, Caveat) load from Google Fonts. Without internet it falls back to Georgia / system fonts.

Made for one person. Maybe, someday.
