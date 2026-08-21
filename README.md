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

## Editing the homepage

Edit [`index.html`](index.html) (body sections only). Shared chrome lives in `_includes/` and `_layouts/`. Site-wide settings (email, URL, collection) are in [`_config.yml`](_config.yml).

## Deployment

Push to the repository’s default branch. GitHub Pages runs Jekyll using the `github-pages` gem (see [`Gemfile`](Gemfile)). The custom domain is set via [`CNAME`](CNAME).

After deploy:

- Homepage: `https://justenoughai.co.uk/`
- Vertical example: `https://justenoughai.co.uk/for/estate-agents/`

## Brand / styling

- **Fonts:** Plus Jakarta Sans (headings), Inter (body) — loaded in `_includes/head.html`
- **Colours:** cream, blue, yellow, muted — defined in the Tailwind config in `head.html`
- **CSS:** Tailwind via CDN (no build step for styles)

To change colours or fonts site-wide, edit `_includes/head.html`.
