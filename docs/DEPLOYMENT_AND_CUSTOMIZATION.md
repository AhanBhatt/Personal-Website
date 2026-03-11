# Deployment And Customization

This document covers how to preview, deploy, restyle, and maintain the site.

## Local Preview

Because this is a static site, there is no build command.

### Fastest preview

Open `index.html` directly in your browser.

### Recommended preview

Run a local static server:

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

This more closely matches deployed behavior.

## Deployment Options

The site can be hosted anywhere that serves static files.

### GitHub Pages

Good fit because the project is already static and root-based.

Typical setup:

1. push the repository to GitHub
2. enable GitHub Pages in repository settings
3. publish from the appropriate branch/folder
4. verify `index.html` is served at the site root

### Netlify / Vercel

Also a good fit. Since there is no build command:

- build command: none
- publish directory: repository root

### Traditional web hosting

Upload:

- `index.html`
- entire `assets/` directory
- entire `docs/` directory if you want documentation published too

## Deployment Requirements

To render correctly, deployment must preserve:

- root `index.html`
- relative `assets/` paths
- UTF-8 encoding

The page also assumes modern browser support for CSS and JavaScript features described in the architecture doc.

## What To Check After Deploying

1. portrait image loads
2. GitHub/LinkedIn/Scholar icons load
3. sticky header works
4. section anchor links work
5. paper cards open their links
6. project cards open their links
7. resume link works
8. mobile layout stacks correctly
9. fonts load as expected

## Customization Guide

## Colors and Surfaces

Most visual tuning starts in the `:root` CSS block.

Main variables:

- `--bg`
- `--fg`
- `--muted`
- `--muted2`
- `--card`
- `--card2`
- `--stroke`
- `--shadow`

Change these if you want to:

- alter the background tone
- increase/decrease contrast
- soften or sharpen card appearance
- change border visibility

## Spacing and Layout

Adjust these variables or selectors:

- `--max` for maximum content width
- `.container` for horizontal page gutters
- `.heroGrid` for hero-column proportions
- `.grid` for card grid spacing
- `section` for vertical section spacing

## Typography

Typography is currently split across:

- `Inter` for most text
- `Newsreader` for the hero headline

You can customize typography by:

1. changing the Google Fonts import
2. updating `font-family` declarations
3. adjusting heading/body sizes in CSS

If you remove remote fonts entirely, define a stronger local fallback strategy.

## Motion

Motion is driven by:

- card hover transforms
- button/nav transitions
- reveal animation
- blinking caret
- typewriter timing
- progress bar width transitions

You can reduce or intensify motion by editing:

- `--ease`
- `.card:hover`
- `.reveal`
- `@keyframes blink`
- the typewriter timing values in `tick()`

Remember to keep `prefers-reduced-motion` behavior intact.

## Card Click Behavior

Paper/project card navigation is powered by JavaScript and a content convention.

To keep it working:

- each clickable card must contain a primary anchor in `.meta`
- external URLs should remain valid
- card markup should remain semantically consistent

If you want a different clickable-card model in the future, the likely alternatives are:

1. wrap the whole card in a single anchor
2. add a dedicated overlay link element
3. drive cards from structured data and render them consistently

The current approach was chosen because it works with the existing markup and preserves the inline link.

## SEO and Metadata Maintenance

Update these when the site positioning changes:

- page title
- meta description
- Open Graph title and description
- JSON-LD `Person` data

This matters for:

- search result snippets
- social sharing previews
- structured-data consistency

## Browser Support Notes

The site is intended for modern browsers.

Potentially sensitive features include:

- `backdrop-filter`
- `color-mix()`
- `IntersectionObserver`
- optional chaining

If you need older-browser support, you may need a simpler CSS/JS approach.

## Maintenance Checklist

Run this checklist after any non-trivial change:

1. open the site on desktop width
2. open the site on a narrow/mobile width
3. confirm no overflowing text
4. confirm cards align cleanly
5. confirm all external links still open correctly
6. confirm paper/project card click behavior still works
7. confirm the scroll progress bar updates
8. confirm reveal animations still trigger
9. confirm reduced-motion fallback remains in place

## Suggested Future Improvements

If the site keeps growing, these are the most practical next steps:

1. move CSS into `styles.css`
2. move JavaScript into `script.js`
3. store papers/projects/timeline entries in a data file
4. introduce a simple generation step for repeated card markup
5. add a lightweight HTML validation and link-check workflow

At the current scale, those changes are optional rather than necessary.
