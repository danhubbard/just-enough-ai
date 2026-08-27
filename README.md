# Just Enough AI

Marketing site for [justenoughai.co.uk](https://justenoughai.co.uk), built with [Jekyll](https://jekyllrb.com/) and hosted on [GitHub Pages](https://pages.github.com/).

The homepage is a single page with card-based sections. Industry landing pages under `/for/` (e.g. `/for/estate-agents/`) use the same brand colours and fonts but an editorial layout — conversational prose and longer-form idea sections rather than mirroring the homepage structure.

## Local development

You need Ruby and Bundler installed.

```bash
bundle install
bundle exec jekyll serve
```

Open [http://localhost:4000](http://localhost:4000). Jekyll rebuilds when you save changes.

To build without serving:

```bash
bundle exec jekyll build
```

Output goes to `_site/` (ignored by git).

## Project structure

```
├── index.html              # Homepage (Jekyll page)
├── _config.yml             # Site + collection config
├── _layouts/
│   ├── default.html        # HTML shell (head, nav, footer)
│   ├── home.html           # Homepage wrapper
│   └── vertical.html       # Industry landing page template
├── _includes/
│   ├── head.html           # Meta, fonts, Tailwind config
│   ├── nav.html            # Site navigation (shared across all pages)
│   ├── footer.html
│   └── scripts.html        # Mobile nav, smooth scroll
├── _for/                   # Vertical landing pages (Jekyll collection)
│   └── estate-agents.md
├── images/
│   ├── robot-1.svg         # Homepage mascots
│   ├── robot-2.svg
│   └── verticals/          # Per-vertical hero graphics
└── CNAME                   # Custom domain for GitHub Pages
```

`landing.html` and `index-alt.html` are design experiments and are excluded from the Jekyll build.

## Adding a vertical landing page

1. Copy an existing page:

   ```bash
   cp _for/estate-agents.md _for/solicitors.md
   ```

2. Update the front matter (see schema below). The URL is derived from the filename: `_for/solicitors.md` → `/for/solicitors/`.

3. Add a hero image at `images/verticals/solicitors-hero.svg` (or point `hero_image` at an existing robot under `images/`).

4. Preview locally, then commit and push. GitHub Pages builds Jekyll automatically on the default branch.

No layout changes are needed unless you want a new section type.

### Front matter schema

All copy for a vertical page lives in YAML front matter. The `vertical` layout renders:

1. **Hero** — dark blue band, audience label, headline, hero image
2. **Sound familiar?** — conversational prose (not cards)
3. **Where AI actually helps** — numbered long-form idea sections
4. **Products** — optional off-the-shelf product cards; hidden when `products: []`
5. **Want to see if this fits?** — split CTA on dark blue

| Field | Required | Description |
|-------|----------|-------------|
| `layout` | Yes | Use `vertical` (set automatically for `_for/` via `_config.yml`) |
| `title` | Yes | Browser tab / SEO title |
| `description` | Yes | Meta description |
| `audience` | Yes | Used in headline and hero label: “For **{audience}**” |
| `tagline` | Yes | One line under the headline |
| `hero_image` | No | Path from site root. Defaults to `images/robot-2.svg` |
| `hero_alt` | No | Alt text for hero image |
| `hero_glow` | No | Glow behind hero: `red` (default, homepage hero), `yellow`, or `blue` |
| `familiar_highlight` | Yes* | Pull-quote style opener for “Sound familiar?” |
| `familiar_paragraphs` | Yes* | List of conversational paragraphs |
| `ideas_intro` | No | Short intro above the ideas section |
| `ideas` | Yes | Long-form sections (see below) |
| `products_heading` | No | Section heading (default “Off-the-shelf, ready when you are”) |
| `products_intro` | No | Intro copy beside the heading |
| `products` | No | Product cards (see below). Leave as `[]` to hide |
| `closer_heading` | No | Defaults to “Let's find where AI can help you” |
| `closer_body` | No | Defaults to low-pressure CTA copy |

\*Use `familiar_highlight` + `familiar_paragraphs` for the conversational section. Legacy `pain_points` (card-style) still works as a fallback but is not recommended for new pages.

#### Idea sections (`ideas`)

Each idea supports longer copy — a lead line, multiple paragraphs, and an optional callout:

```yaml
ideas:
  - title: Listing copy that learns your voice
    lead: Not a template. Not generic AI slop.
    paragraphs:
      - First paragraph of the idea…
      - Second paragraph…
    note: Optional “Worth knowing” callout — e.g. how this avoids generic AI output.
```

Legacy short `helps: [{ title, description }]` still renders if `ideas` is omitted.

### Example: new vertical page

```yaml
---
layout: vertical
title: Just enough AI for solicitors
description: Practical AI for law firms — without the hype.
audience: solicitors
tagline: You're billing for advice, not retyping the same email.
hero_image: images/verticals/solicitors-hero.svg
hero_glow: yellow
familiar_highlight: You know AI could help. You're just not sure where it fits in a firm that lives and dies by accuracy.
familiar_paragraphs:
  - Every legal tech vendor promises transformation…
  - Maybe you tried ChatGPT for a client update…
ideas_intro: A few places we've seen AI earn its keep in practice.
ideas:
  - title: Client updates in your firm's voice
    lead: First drafts that sound like a partner wrote them — not a chatbot.
    paragraphs:
      - We learn from how your team already writes…
    note: Nothing sends without a human review.
products: []
---
```

#### Product cards (`products`)

Each product is an off-the-shelf offering:

```yaml
products:
  - title: Listing Draft
    tagline: Rightmove-ready copy from your notes
    status: Available now          # "Available now" (green) or "In development" (amber)
    badge: Free                    # optional — highlighted when "Free"
    description: What the product does.
    includes:                      # optional bullet list
      - Trained on your listings
```

Hide the section with `products: []`.

## Adding a case study

Case studies live in `_work/` and all use the `case-study` layout. Everything on the page comes from front matter — no layout changes needed for a new one.

1. Copy the template:

   ```bash
   cp _work/_template.md _work/acme-lettings.md
   ```

   `_template.md` starts with an underscore, so Jekyll never builds it. It stays put as the copy source.

2. Fill in the front matter (schema below). The URL comes from the filename: `_work/acme-lettings.md` → `/work/acme-lettings/`.

3. Set `order`. The sidebar sorts by it, and the **Work** menu item points at the lowest-numbered case study — so `order: 1` is the one people land on.

4. Drop any gallery images in `images/work/` and list them under `images` (1–4 works best).

Case studies are `noindex` by default (set in `_config.yml`, same as the vertical pages). Remove that default when you're ready for them to be indexed.

### What the layout renders

1. **Hero** — dark blue band, client label, headline, strapline, optional key/value meta row and hero image
2. **Sidebar** — links to every case study, current one highlighted (a horizontal pill row on mobile)
3. **The opportunity / What we did / The result** — prose sections, each optional
4. **Stats, quote** — optional, shown under “The result”
5. **Gallery** — 1–4 images, grid adapts to the count, click to enlarge
6. **Closer** — the shared dark blue CTA

Any section with no content is skipped entirely.

### Front matter schema

| Field | Required | Description |
|-------|----------|-------------|
| `order` | Yes | Sort order. Lowest is the “Work” menu destination |
| `title` | Yes | Browser tab / SEO title |
| `description` | Yes | Meta description |
| `client` | Yes | Hero eyebrow label |
| `nav_title` | No | Sidebar label. Defaults to `client` |
| `sector` | No | Small line under the sidebar label |
| `headline` | Yes | Hero `<h1>`. Defaults to `title` |
| `strapline` | No | One or two lines under the headline |
| `meta` | No | List of `{ label, value }` shown as a row in the hero |
| `client_logo` | No | Client logo in the hero. Needs to be white or light — it sits on the dark blue band |
| `client_logo_alt` | No | Alt text for the logo. Defaults to `client` |
| `hero_image` | No | Mascot-style hero image, used only when there's no `client_logo`. Omit both for a text-only hero |
| `hero_alt` | No | Alt text for the hero image |
| `hero_glow` | No | Glow behind hero: `blue` (default), `yellow`, or `red` |
| `opportunity_lead` | No | Pull-quote style opener for the section |
| `opportunity` | No | List of paragraphs |
| `what_we_did_lead` / `what_we_did` | No | Same shape as above |
| `result_lead` / `result` | No | Same shape as above |
| `stats` | No | List of `{ value, label }` — 1–3 render nicely |
| `quote` / `quote_attribution` | No | Client quote under “The result” |
| `images` | No | Gallery images (see below). Leave as `[]` to hide |
| `gallery_position` | No | Where the gallery renders (see below). Accepts one value or a list |
| `closer_heading` / `closer_body` | No | Overrides the default CTA copy |

Section headings can be overridden with `opportunity_heading`, `what_we_did_heading` and `result_heading` if a particular story needs different framing.

#### Gallery images

```yaml
images:
  - src: images/work/acme-dashboard.png
    alt: The listing drafting screen
    caption: Optional caption under the image
```

The grid adapts: one image goes full width, two or four sit in a 2-up grid, three go 3-up on desktop. Clicking one opens a lightbox — arrow keys and Esc work, and it's skipped entirely on pages with no images.

`gallery_position` controls where it renders:

| Value | Where |
|-------|-------|
| `after_hero` | Full-width band under the hero, above the sidebar |
| `body_top` | Top of the text column, above “The opportunity” |
| `opportunity` / `what_we_did` / `result` | Directly after that section (`what_we_did` is the default) |
| `end` | Last thing in the text column |

It also takes a list — `gallery_position: [after_hero, result]` — which renders the gallery in both places. The lightbox collapses duplicates, so it still counts three images, not six.

Captions don't show under the thumbnails; they appear when an image is enlarged.

#### Markdown body

Anything written below the front matter is rendered as an extra prose block after the three sections. Most case studies won't need it — use the YAML fields so every page stays consistent.

## Editing the homepage

Edit [`index.html`](index.html) (body sections only). Shared chrome lives in `_includes/` and `_layouts/`. Site-wide settings (email, URL, collection) are in [`_config.yml`](_config.yml).

## Deployment

Push to the repository’s default branch. GitHub Pages runs Jekyll using the `github-pages` gem (see [`Gemfile`](Gemfile)). The custom domain is set via [`CNAME`](CNAME).

After deploy:

- Homepage: `https://justenoughai.co.uk/`
- Vertical example: `https://justenoughai.co.uk/for/estate-agents/`
- Case studies: `https://justenoughai.co.uk/work/` (redirects to the first one)
- Case study example: `https://justenoughai.co.uk/work/sng-site-report-generator/`

## Brand / styling

- **Fonts:** Plus Jakarta Sans (headings), Inter (body) — loaded in `_includes/head.html`
- **Colours:** cream, blue, yellow, muted — defined in the Tailwind config in `head.html`
- **CSS:** Tailwind via CDN (no build step for styles)

To change colours or fonts site-wide, edit `_includes/head.html`.
