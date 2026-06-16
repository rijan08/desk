# desk

A small personal shelf of documents, served as a static site on **GitHub Pages**.
The root `index.html` is a landing hub that lists every entry as a card, grouped
by category, with a client-side filter box and a live entry count.

🔗 **Live:** https://rijan08.github.io/desk/

---

## Folder convention

```
desk/
├─ index.html                    ← the hub (cards, filter, count)
├─ .nojekyll                     ← tells Pages to serve files as-is (no Jekyll)
├─ README.md
├─ react-native-roadmap/
│  └─ index.html                 ← one doc per folder
└─ mqtt-rider-app/
   └─ index.html
```

**Rules**

- **One folder per document.** The folder name is the URL slug — keep it
  lowercase-with-hyphens (e.g. `react-native-roadmap`).
- **Each folder holds exactly one `index.html`.** Because of that filename,
  the doc is reachable at a **clean URL** with no `.html`:
  `https://rijan08.github.io/desk/react-native-roadmap/`.
- **Links never include `.html`.** Always link to the folder
  (`href="react-native-roadmap"`), not the file.
- `.nojekyll` (an empty file at the root) must stay — without it GitHub Pages
  runs Jekyll and may hide or mangle files.

---

## Adding a new entry

There are two parts to every entry:

1. a folder with an `index.html` (the document itself), and
2. a card in the hub so it shows up on the landing page.

### A. Via git (from a computer)

```bash
# 1. create the folder + document
mkdir my-new-doc
cp some-document.html my-new-doc/index.html   # filename MUST be index.html

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
2. In the filename box, type the folder and file together:
   `my-new-doc/index.html` — typing `/` creates the folder for you.
3. Paste your HTML into the editor.
4. **Commit changes** (commit straight to `main`).

> Pasting long HTML on a phone is fiddly — alternatively use
> **Add file ▸ Upload files**, then drag/upload the file once it's named
> `index.html` inside the right folder.

**Register the card in the hub**

5. Open `index.html` at the repo root → tap the ✏️ **pencil** to edit.
6. Find the `ENTRIES` array near the bottom and add one object:

   ```js
   {
     category: "Learning",          // "Learning" or "Personal" (or "Writing" once enabled)
     title: "My New Doc",
     url: "my-new-doc",             // the folder name — NO trailing slash, NO .html
     desc: "One-line description shown on the card.",
     accent: "violet",              // teal | amber | violet | rose
     tags: ["topic", "another"]     // shown as little chips; also searchable
   },
   ```

7. **Commit changes** to `main`. Done — the card appears and the entry count
   updates automatically.

---

## Categories

Categories are driven by the `CATEGORIES` array in `index.html`:

```js
const CATEGORIES = ["Learning", "Personal"];
```

- A category listed here always renders its heading, even with zero entries
  (it shows a muted “Nothing here yet.”).
- A **Writing** section is scaffolded but commented out — enable it later by
  uncommenting the `CATEGORIES.push("Writing")` line and tagging entries with
  `category: "Writing"`.

---

## How it's served

- **Settings ▸ Pages ▸** Source: *Deploy from a branch*, Branch: `main` / `/ (root)`.
- Static files only — no build step. The hub's filtering and counting are
  plain client-side JavaScript, so everything works straight off the CDN.
