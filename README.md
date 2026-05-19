# Just Enough AI

Marketing site for [justenoughai.co.uk](https://justenoughai.co.uk), built with [Jekyll](https://jekyllrb.com/) and hosted on [GitHub Pages](https://pages.github.com/).

The homepage is a single page. Industry-specific landing pages live under `/for/` (for example `/for/estate-agents/`) and share the same brand, layout, and components.

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
│   ├── nav.html            # Navigation (home vs vertical)
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

All copy for a vertical page lives in YAML front matter. The `vertical` layout renders five sections; the **Tools** section is omitted when `tools` is empty.

| Field | Required | Description |
|-------|----------|-------------|
| `layout` | Yes | Use `vertical` (set automatically for `_for/` via `_config.yml`) |
| `title` | Yes | Browser tab / SEO title |
| `description` | Yes | Meta description |
| `audience` | Yes | Used in headline: “Just enough AI for **{audience}**.” |
| `tagline` | Yes | One line under the headline |
| `hero_image` | No | Path from site root, e.g. `images/verticals/estate-agents-hero.svg`. Defaults to `images/robot-2.svg` |
| `hero_alt` | No | Alt text for hero image |
| `hero_glow` | No | Blur behind hero: `yellow`, `blue`, or `red` (default `yellow`) |
| `pain_intro` | No | Optional subheading under “Sound familiar?” |
| `pain_points` | Yes | List of `{ emoji, title, description }` |
| `helps_intro` | No | Optional intro under “Where AI actually helps” |
| `helps` | Yes | List of `{ title, description }` |
| `tools` | No | List of `{ title, description }`. Leave as `[]` to hide the section |
| `tools_intro` | No | Optional intro for tools section |
| `closer_heading` | No | Defaults to “Let's find where AI can help you” |
| `closer_body` | No | Defaults to low-pressure CTA copy |

### Example: minimal new page

```yaml
---
layout: vertical
title: Just enough AI for solicitors
description: Practical AI for law firms — without the hype.
audience: solicitors
tagline: You're billing for advice, not retyping the same email.
hero_image: images/verticals/solicitors-hero.svg
hero_glow: yellow
pain_points:
  - emoji: "📄"
    title: Document review eats the day
    description: "..."
helps:
  - title: First-draft client updates
    description: "..."
tools: []
---
```

### Showing the “Tools we've already built” section

Populate `tools` with one or more items:

```yaml
tools:
  - title: Listing draft assistant
    description: Turns viewing notes into Rightmove-ready copy in your agency's tone.
```

Remove the section again by setting `tools: []`.

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
