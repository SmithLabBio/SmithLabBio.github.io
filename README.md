# meganlsmith.github.io

Source for the Smith Lab website — plain HTML/CSS/JS, no build step required.

## File structure

```
index.html          Home
research.html        Research
people.html          People
publications.html    Publications (reads pubs.json)
software.html        Software
news.html             News (reads news.json)
contact.html          Contact
styles.css            All styles
nav.js                Sidebar mobile menu toggle
pubs.json             Publication data — edit this file to add new papers
news.json             News/updates data — edit this file to add new items
photos/               Empty folders for header/gallery/people/research photos
```

## Design

Left sidebar navigation (persistent on desktop, collapses to a hamburger
drawer on mobile) instead of a top nav bar. Circular photo crops for people
and the hero portrait, rounded cards throughout, and two horizontally
scrolling strips on the homepage (a fieldwork/specimen photo gallery that
auto-scrolls on hover, and the people row). Typeset in Petrona (headings),
Nunito Sans (body), and JetBrains Mono (labels/dates) — deliberately
different from the Fraunces/Source Sans/Space Mono pairing used by the
nearby Stone Lab site, so the two don't read as a reskin of each other.

## Adding photos

Every photo spot is currently a dashed/circular placeholder so nothing is
blocked on images. To add a real photo:

1. Drop the image file into the matching `photos/` subfolder (`header/`,
   `gallery/`, `people/`, or `research/`).
2. Find the placeholder (`.photo-circle`, `.person-photo`,
   `.gallery-item`, or `.figure-placeholder`) in the relevant `.html` file
   and add an `<img src="photos/…/your-photo.jpg" alt="…">` inside it. For
   circular photo spots the CSS already clips any `<img>` you add into a
   circle — no extra cropping needed on your end.

## Adding a new publication

Open `pubs.json` and add a new object to the array:

```json
{
  "year": 2027,
  "title": "Your paper title",
  "meta": "<strong>Smith, Megan L.</strong>, & Coauthor Name – Journal, vol(issue), pages",
  "type": "peer-reviewed",
  "doi": "https://doi.org/..."
}
```

`type` is `"peer-reviewed"`, `"preprint"`, or `"in-review"` (controls the
filter buttons and, for `"in-review"`, a small badge — set `status` to the
badge text, e.g. `"In review"` or `"In revision"`). `doi` is optional.

Note: publication PDFs previously hosted on Squarespace
(`meganlsmith.org/s/...`) were **not** carried over, since that hosting goes
away once Squarespace is cancelled. Only DOI links are included for now — you
can add self-hosted PDFs into a `pdfs/` folder later and link them with a
`"pdf"` field the same way `pubs.json` already supports.

## Adding lab news

Open `news.json` and add a new object:

```json
{
  "date": "2027-03-01",
  "display_date": "MAR 2027",
  "title": "Short headline",
  "body": "A sentence or two of detail."
}
```

Newest-first ordering is automatic (sorted by `date`). The homepage shows
the 3 most recent items; `news.html` shows all of them.

## Deploying to GitHub Pages

### 1. Create the repository

Go to [github.com/new](https://github.com/new) while signed in as
**meganlsmith** and create a repository named exactly:

```
meganlsmith.github.io
```

(Public, no README/gitignore/license — this repo will be empty.) Naming it
this way makes GitHub Pages serve it directly at
`https://meganlsmith.github.io/`, with no extra configuration.

### 2. Push this folder to it

From this folder, run:

```bash
git init
git add .
git commit -m "Initial site"
git branch -M main
git remote add origin https://github.com/meganlsmith/meganlsmith.github.io.git
git push -u origin main
```

(If you don't have `git`/`gh` set up locally yet, GitHub's own "uploading an
existing file" web UI also works — drag every file/folder from this zip into
the empty repo and commit.)

### 3. Turn on Pages

Usually nothing else to do — a `username.github.io` repo publishes
automatically. If it doesn't appear within a minute or two, check
**Settings → Pages** in the repo and make sure the source is set to
"Deploy from a branch" → `main` / `/(root)`.

Your site will be live at **https://meganlsmith.github.io/**.

### 4. Later: point meganlsmith.org at it

When you're ready to retire Squarespace hosting and move the domain over:

1. Add a file named `CNAME` (no extension) to the root of this repo
   containing one line: `meganlsmith.org`
2. At your domain's DNS provider (wherever meganlsmith.org's nameservers /
   DNS records are managed — check your registrar), add:
   - Four **A** records for the apex (`@` / `meganlsmith.org`) pointing to
     GitHub Pages' IPs:
     `185.199.108.153`, `185.199.109.153`, `185.199.110.153`,
     `185.199.111.153`
   - A **CNAME** record for `www` pointing to `meganlsmith.github.io`
3. In the repo's **Settings → Pages**, enter `meganlsmith.org` as the custom
   domain and (once DNS propagates, sometimes up to 24h) enable "Enforce
   HTTPS".

DNS changes can take a few hours to propagate, and the site keeps working at
`meganlsmith.github.io` the whole time, so there's no downtime risk in doing
this whenever you're ready — it doesn't have to happen at initial launch.
