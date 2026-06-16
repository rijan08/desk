# desk

A small personal shelf of documents, served as a static site on **GitHub Pages**.
The root `index.html` is a landing hub that lists every entry as a flat list of
cards (with the category shown as a small tag), plus a client-side filter box
and a live entry count.

🔗 **Live:** https://rijan08.github.io/desk/

---

## Folder convention

Docs are grouped on disk by **category folder**, and each doc gets its own
**slug folder** containing a single `index.html`:

```
desk/
├─ index.html                              ← the hub (flat card list, filter, count)
├─ .nojekyll                               ← serve files as-is (no Jekyll)
├─ README.md
└─ learning/                               ← category folder
   ├─ react-native-roadmap/
   │  └─ index.html                        ← one doc per slug folder
   └─ mqtt-rider-app/
      └─ index.html
```

**Rules**

- **Group by category folder.** The category becomes a lowercase folder:
  `learning/`, `personal/`, `writing/`. The category is *not* a heading on the
  landing page — it shows only as a tag on each card.
- **One slug folder per document**, holding exactly one `index.html`. Keep slugs
  lowercase-with-hyphens (e.g. `react-native-roadmap`).
- Because the file is `index.html`, the doc is served at a **clean URL** with no
  `.html`: `https://rijan08.github.io/desk/learning/react-native-roadmap/`.
- **Links never include `.html`.** In the hub, an entry's `url` is
  `<category-folder>/<slug>` — e.g. `learning/react-native-roadmap`.
- `.nojekyll` (an empty file at the root) must stay — without it GitHub Pages
  runs Jekyll and may hide or mangle files.

---

## Adding a new entry

There are two parts to every entry:

1. a `category/slug/index.html` (the document itself), and
2. a card in the hub so it shows up on the landing page.

### A. Via git (from a computer)

```bash
# 1. create the category folder (if new) + slug folder + document
mkdir -p learning/my-new-doc
cp some-document.html learning/my-new-doc/index.html   # filename MUST be index.html

# 2. register it in the hub: open index.html and add an object to the
#    ENTRIES array (see the template below)

# 3. commit + push
git add .
git commit -m "Add: my new doc"
git push
```

Pages redeploys automatically within a minute or two.

### B. Via the GitHub web UI (from a phone)

You never need a terminal — everything below is doable in the browser/app.

**Create the document**

1. Go to the repo → **Add file ▸ Create new file**.
2. In the filename box, type the full path — typing `/` creates each folder:
   `learning/my-new-doc/index.html`
3. Paste your HTML into the editor.
4. **Commit changes** (commit straight to `main`).

> Pasting long HTML on a phone is fiddly — alternatively use
> **Add file ▸ Upload files**, then make sure the uploaded file ends up named
> `index.html` inside `learning/my-new-doc/`.

**Register the card in the hub**

5. Open `index.html` at the repo root → tap the ✏️ **pencil** to edit.
6. Find the `ENTRIES` array and add one object:

   ```js
   {
     category: "Learning",                 // shown as the card tag
     title: "My New Doc",
     url: "learning/my-new-doc",           // <category-folder>/<slug> — no trailing .html
     desc: "One-line description shown on the card.",
     accent: "violet",                     // tag colour: teal | amber | violet | rose
     tags: ["topic", "another"]            // not shown, but included in search
   },
   ```

7. **Commit changes** to `main`. Done — the card appears and the entry count
   updates automatically.

---

## Categories

Categories are just labels + folders — pick whatever you like; common ones:

- **Learning** → `learning/`
- **Personal** → `personal/`
- **Writing** → `writing/`

Set the `category` field on the entry (it renders as the card's tag) and put the
doc under the matching lowercase folder. New categories need no config — the
landing page is a single flat list and doesn't group by category.

---

## How it's served

- **Settings ▸ Pages ▸** Source: *Deploy from a branch*, Branch: `main` / `/ (root)`.
- Static files only — no build step. The hub's filtering and counting are
  plain client-side JavaScript, so everything works straight off the CDN.
