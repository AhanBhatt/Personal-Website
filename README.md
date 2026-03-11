# Personal Website

Single-page portfolio website for Ahan Bhatt, built as a static site with plain HTML, CSS, and JavaScript.

The project is intentionally lightweight:

- No framework
- No build step
- No package manager
- No server-side code
- No generated assets

Everything that controls layout, styling, content, and behavior lives in [`index.html`](./index.html), with images and icons stored in [`assets/`](./assets/).

## What This Project Includes

- Sticky top navigation with in-page section links
- Hero section with animated typewriter text
- Profile image and "Now" summary card
- About section
- Skills section
- CV timeline section
- Papers section
- Projects section
- Contact section
- Footer
- Scroll progress bar
- Scroll-triggered reveal animations
- Full-card click behavior for paper/project cards
- Structured metadata via JSON-LD
- Responsive layout for desktop and mobile
- Reduced-motion support for users who prefer less animation

## Tech Stack

- HTML5
- CSS3
- Vanilla JavaScript
- Google Fonts (`Inter` and `Newsreader`)
- Static image and SVG assets

## Repository Structure

```text
Personal-Website/
|-- index.html
|-- assets/
|   |-- ahan.jpeg
|   `-- Icons/
|       |-- github.svg
|       |-- linkedin.svg
|       `-- scholar.svg
`-- docs/
    |-- ARCHITECTURE.md
    |-- CONTENT_GUIDE.md
    `-- DEPLOYMENT_AND_CUSTOMIZATION.md
```

## How The Site Is Organized

The site uses a single HTML document with three major layers:

1. `head`
   Contains metadata, font loading, all CSS, and JSON-LD structured data.
2. `body`
   Contains the visible page structure: decorative layers, header, hero, content sections, and footer.
3. Inline script
   Adds reveal-on-scroll, scroll progress updates, typewriter animation, and clickable paper/project cards.

Because the site is implemented in one file, content changes and behavior changes are both made directly in [`index.html`](./index.html).

## Quick Start

### Option 1: Open directly

Open [`index.html`](./index.html) in a browser.

This is enough for most content edits because the site does not depend on a local build process.

### Option 2: Run a lightweight local server

If you prefer previewing through a local server, use one of these:

```bash
python -m http.server 8000
```

or

```bash
py -m http.server 8000
```

Then open:

```text
http://localhost:8000
```

## Editing Workflow

For most updates:

1. Open [`index.html`](./index.html).
2. Edit the relevant section content or styles.
3. Refresh the browser.
4. Verify desktop and mobile layouts.
5. Commit the change.

If the update is content-only, you will usually touch:

- page metadata
- hero copy
- section cards
- external links
- assets

If the update changes behavior, you will usually touch:

- inline CSS in the `<style>` block
- inline JavaScript near the bottom of the file

## Documentation Map

The README is the overview. Detailed docs are split out so the project remains readable.

- [docs/ARCHITECTURE.md](./docs/ARCHITECTURE.md)
  Full implementation breakdown: page structure, CSS system, JavaScript behavior, external dependencies, and runtime flow.
- [docs/CONTENT_GUIDE.md](./docs/CONTENT_GUIDE.md)
  How to update text, links, papers, projects, timeline entries, metadata, and assets without breaking the site.
- [docs/DEPLOYMENT_AND_CUSTOMIZATION.md](./docs/DEPLOYMENT_AND_CUSTOMIZATION.md)
  Local preview, hosting, browser considerations, visual customization, and maintenance checklist.

## Key Implementation Notes

- The project currently has no `README.md`-driven code generation or automation.
- The `Papers` and `Projects` cards are clickable as whole cards, but the implementation still relies on each card keeping its primary link inside `.meta a`.
- The site loads fonts from Google Fonts, so the final visual result assumes network access in the browser.
- The portrait image is stored at [`assets/ahan.jpeg`](./assets/ahan.jpeg).
- External profile icons are stored in [`assets/Icons/`](./assets/Icons).
- Structured data for search engines is embedded in the page as a `Person` JSON-LD block.

## When To Read Which Doc

- If you want to understand how the site works internally, read [docs/ARCHITECTURE.md](./docs/ARCHITECTURE.md).
- If you want to update projects, papers, text, links, or images, read [docs/CONTENT_GUIDE.md](./docs/CONTENT_GUIDE.md).
- If you want to deploy, restyle, or maintain the site over time, read [docs/DEPLOYMENT_AND_CUSTOMIZATION.md](./docs/DEPLOYMENT_AND_CUSTOMIZATION.md).

## Maintenance Summary

The project is easy to maintain because it is small, but that simplicity comes with one tradeoff: a single large `index.html` file contains nearly everything. The documentation in `docs/` exists to offset that by making the layout, behaviors, and editing rules explicit.
