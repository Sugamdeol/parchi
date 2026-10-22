# Parchi — Chhota Link, Bada Kaam 🇮🇳

**Made in India • Made in Ghaziabad with chai • Atmanirbhar Bharat**

**Live demo:** https://parchibysd.vercel.app

Parchi is a single-file, no-login, deliberate text sharing tool for normal users. Kirana list, address, shayari, code, file, checklist, secret — likho, chhota link banao, WhatsApp pe bhejo. 7 akshar ka link. No jargon. No signup. No foreign tracking.

> Design philosophy: hand-built, paper-like, Kalam handwriting font, taped chit, ruled paper. Feels like a real parchi, not AI.

---

## ✨ Modes

| Mode | Icon | Kya karta hai |
|------|------|---------------|
| **Parchi** | 📝 | Plain text — address, note, shayari. Title optional, paper color, font size |
| **Code** | 💻 | Code snippet with language selector, line numbers, Prettify JSON |
| **File** | 📎 | Photo, PDF, zip — drag & drop, up to 1GB, 1h/12h/24h/72h expiry via litterbox |
| **List** | ✅ | Har line ek item — viewer me tick kar sakte ho, local save |
| **Secret** | 🔒 | Burn after reading, password lock, views limit — ek baar padhne ke baad gayab |

All modes → same short link `parchibysd.vercel.app/#AbC123x` (7 chars, base62). Custom vanity ID bhi: `mummy-dawai`, `api-fix`.

## 🚀 Features — More than just text

- **Chhota link** — 7 akshar, base62 (A-Z a-z 0-9 without confusing chars), collision check
- **Custom naam** — `?id=mummy-dawai` → `/#mummy-dawai`, Check availability button
- **Edit after sharing** — same link update! Anyone with link can edit via ✏️ Edit button or `?edit=ID`. Edit banner: “Save → same link update”. Preserves views/reactions/comments on edit
- **Title + paper** — White/Cream/Pink paper, font A-/A+, Kalam + IBM Plex Mono
- **Lock + Burn** — 4-6 digit lock code, burn-after-reading, views limit 1/5/10/unlimited
- **Expiry** — 1 din / 7 din / 30 din (mantledb 30d purge, file expiry 1h-72h)
- **Reactions** — ❤️ 😂 🙏 🔥 — live count, stored in same object
- **Comments** — Local thread per parchi, 200 chars
- **QR Code** — auto via qrserver, scan karo
- **Voice** — 🎤 Bolo (Web Speech API, en-IN), 🔊 Suno (SpeechSynthesis)
- **Image banao** — Canvas → PNG of parchi, ruled paper look
- **Share** — WhatsApp, Telegram, Email, Copy, Download, Open, Duplicate, Nayi
- **History** — Last 30 parchis in localStorage, search, export JSON, clear
- **Viewer** — Views counter, expiry countdown, code gutter, list checkboxes (local tick), file preview for images
- **Offline draft** — Auto-save draft in localStorage, restores if no hash
- **Made in India UI** — Tricolor 4px sticky bar (saffron #FF9933, white with chakra dot, green #138808), flag gradient badge “Made in India”, footer: Atmanirbhar Bharat, No foreign tracking, Ghaziabad with chai

## 🇮🇳 Made in India

- No foreign tracking, no analytics
- Tricolor accent: `--saffron: #FF9933`, `--india-green: #138808`, chakra dot in white strip
- Header pill: flag gradient box + “Made in India” + “edit after sharing • 🇮🇳”
- Footer: “Made in India • Made in Ghaziabad with chai • 🇮🇳 Atmanirbhar Bharat • No foreign tracking • Data in India-friendly free tiers” + “Edit anytime — same link updates”
- Fonts: Kalam (handwriting), Anek Devanagari (Hindi-friendly title), IBM Plex Mono (code)
- Language: Hinglish UI — “Kya likhna hai?”, “Chhota link banao”, “Copy ho gaya!”
- Hosting: static single file, works on any India-friendly free tier (Cloudflare Pages, Netlify, Vercel, GitHub Pages)

## 🔗 Link kaise banta hai?

1. Likho (text/code/list/secret) ya file kheecho
2. (Optional) Apna chhota naam likho → Check → Available?
3. (Optional) More → expiry, lock code, burn, QR, views limit
4. “Chhota link banao” dabao
5. Link ready: `https://yourdomain/#AbC123x`
6. Copy / WhatsApp / QR
7. Edit karna hai? `?edit=AbC123x` kholo ya viewer me “Edit” dabao → edit → Save → same link update!

## ✏️ Edit After Sharing — How it works

- **Same ID overwrite**: When editing, app POSTs to same `API/{id}` with new `t`/`title` but preserves `views`, `reactions`, `comments`
- **Access**: Anyone with link can edit — no login, deliberate openness (parchi on fridge)
- **URL**: `?edit=ID` param triggers `loadEdit()` — fetches data, fills editor, shows banner “✏️ Edit mode 🇮🇳 • ID • ?edit=ID • Save → same link update”
- **Viewer Edit**: Viewer → Edit button → redirects to `?edit=ID`
- **Banner**: Saffron border, cream gradient, flag box
- **Toast**: “Edit mode — save karega toh same link update hoga 🇮🇳”

## 🛠️ Tech Stack — No Key, No Signup

- **Single file**: `/home/user/parchi.html` — HTML+CSS+JS, no build, no deps
- **Storage**: `https://mantledb.sh/v2/parchi-gzb` — anonymous POST/GET/DELETE, CORS `*`, 64KB per item, 30d auto-purge, namespace unclaimed (no key needed). Previous: jsonstorage.net required API key → rejected, jsonblob Cloudflare 403, npoint no new keys → switched
- **File upload**: `https://litterbox.catbox.moe/resources/internals/api.php` — POST multipart `reqtype=fileupload`, `time=1h/12h/24h/72h`, `fileToUpload`, returns `https://litter.catbox.moe/xxxx`, max 1GB, CORS `*` verified via `curl -i`. Alternatives tested: catbox user API 412, 0x0.st no CORS, file.io CORS not enabled, storage.to not tested
- **QR**: `https://api.qrserver.com/v1/create-qr-code/?size=140x140&data=URL`
- **Voice**: Web Speech API + SpeechSynthesis
- **Image**: Canvas 2D, ruled paper render, dataURL download
- **No backend**: All client-side, localStorage for history/draft/checks

Payload example:
```json
{
  "type": "text|code|file|list|secret",
  "t": "content",
  "title": "optional",
  "lang": "javascript",
  "files": [{"url":"https://litter.catbox.moe/abc","name":"photo.jpg","size":123,"type":"image/jpeg"}],
  "c": "2026-08-25T...",
  "exp": 1724630400000,
  "burn": false,
  "pwd": "4821",
  "views": 0,
  "viewsLimit": 0,
  "reactions": {"heart":0,"laugh":0,"pray":0,"fire":0},
  "comments": [],
  "paper": "#FFFFFF",
  "v": 6
}
```

## 📂 Files

- `parchi.html` — The app, v7, Made in India + edit-after-sharing
- `README.md` — This file

## 🧪 How to run

Just open `parchi.html` in browser, or host static:
```bash
# python
python -m http.server 8000
# then http://localhost:8000/parchi.html
```
No npm, no build.

## 🔒 Privacy & Limits

- 64KB per parchi (mantledb limit) — file mode stores only URLs, not file content, so bypasses limit
- 30 din auto-delete (mantledb), file links 1h-72h (litterbox)
- No auth — anyone with link can read/edit (if you need private, use lock code)
- No tracking — no GA, no FB pixel
- Data in India-friendly free tiers — you can self-host

## 🗺️ Roadmap (deliberate, not AI)

- [x] 5 modes
- [x] Made in India branding
- [x] Edit after sharing same link
- [x] README
- [ ] PWA offline
- [ ] Hindi transliteration toggle

---

**Parchi** — chhota link, bade kaam. 🇮🇳 Jai Hind!

> Made in Ghaziabad with chai. Edit anytime — same link updates.
