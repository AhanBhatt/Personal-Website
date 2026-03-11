# Architecture

This document explains how the site is built, how the page is structured, and how the runtime behavior works.

## Architectural Model

The website is a static single-document application:

- One HTML file contains markup, styles, metadata, and JavaScript
- A small `assets/` folder contains the portrait and external-link icons
- The browser is the only runtime
- There is no API, no backend, no routing layer, and no bundling pipeline

This architecture favors:

- very fast iteration
- easy hosting on any static platform
- minimal operational complexity
- straightforward manual editing

This architecture also means:

- `index.html` grows large over time
- content, styling, and behavior are tightly colocated
- there is no component isolation beyond semantic sections and CSS classes

## File Responsibilities

### `index.html`

The main application file. It contains:

- document metadata
- Open Graph tags
- font imports
- CSS custom properties and component styles
- JSON-LD structured data
- all visible page content
- all client-side interaction logic

### `assets/ahan.jpeg`

Primary portrait image shown in the right column of the hero area.

### `assets/Icons/github.svg`

GitHub icon used in the external links row.

### `assets/Icons/linkedin.svg`

LinkedIn icon used in the external links row.

### `assets/Icons/scholar.svg`

Google Scholar icon used in the external links row.

## Document Head

The `<head>` section performs five jobs:

1. Defines document basics
   - character encoding
   - viewport behavior
   - theme color
2. Defines search/social metadata
   - page title
   - meta description
   - Open Graph title/description/type
3. Loads typography
   - `Inter` for UI/body text
   - `Newsreader` for the main hero headline
4. Defines all CSS
5. Publishes structured data
   - `Person` schema in JSON-LD format

## CSS System

All styles live in one `<style>` block. The CSS is organized by feature areas rather than by file/module boundaries.

## Design Tokens

The `:root` block defines reusable tokens:

- `--bg`
- `--fg`
- `--muted`
- `--muted2`
- `--card`
- `--card2`
- `--stroke`
- `--shadow`
- `--radius`
- `--radius2`
- `--max`
- `--ease`
- `--accent`

These drive the visual system across:

- page background
- borders
- card surfaces
- shadows
- motion timing
- container width

## Global Layout Rules

Global rules define:

- `box-sizing: border-box`
- smooth scrolling
- body background and foreground colors
- base font stack
- horizontal overflow clipping
- anchor reset behavior
- shared container width

The `.container` class constrains content to a centered max width while leaving fixed horizontal breathing room.

## Decorative Background Layers

The page background is composed from three pieces:

### `.bg`

Fixed-position radial gradients create large soft glows behind the content.

### `.grain`

A fixed SVG-noise texture overlays the page to avoid a flat background.

### `.progressWrap` and `.progressBar`

A fixed progress indicator sits at the top edge of the viewport and reflects scroll depth.

## Header and Navigation

The header is sticky and uses blur plus a semi-transparent background so it remains readable while scrolling.

Important pieces:

- `.nav`
  Main flex row containing brand, nav, and call-to-action button
- `.brand`
  Name plus short descriptor
- `nav ul`
  In-page section links
- `.actions`
  Right-side CTA wrapper
- `.btn`
  Shared button visual style

The navigation links target section IDs directly:

- `#about`
- `#timeline`
- `#pubs`
- `#projects`
- `#contact`

## Hero Area

The hero is the first major visible block and is split into two columns through `.heroGrid`.

### Left Column

Contained inside `.heroCard` and includes:

- research/topic pills
- headline
- animated typed sentence
- longer mission statement
- main call-to-action buttons
- external profile links

### Right Column

Contained inside `.stack` and includes:

- `.portrait`
  Portrait image panel with gradient overlay and label
- `.mini`
  "Now" card listing current focus areas

## Main Content Sections

Each section follows a similar pattern:

- `<section id="...">`
- `.container`
- `.sectionHead`
- section-specific layout content

### About

Two cards:

- Research themes
- Tooling

### Skills

Multiple cards for:

- Languages
- Libraries and Frameworks
- Cloud and Infrastructure
- MLOps and Deployment
- Data Engineering

### Timeline

A vertical timeline implemented with:

- `.timeline`
- `.tItem`
- `.tTop`
- `.bullets`

The left vertical line is created with a pseudo-element on `.timeline`. Each milestone dot is created with a pseudo-element on `.tItem`.

### Papers

Paper entries are rendered as cards inside `.grid`. Each card typically contains:

- title
- source tag
- summary
- keyword pills
- one primary external link inside `.meta a`

### Projects

Project entries are also rendered as cards inside `.grid`, using `.card.third` for a three-column desktop layout.

Each project card typically contains:

- title
- short tag
- short description
- keyword pills
- one primary external link inside `.meta a`

The Satellite Clock Prediction card includes an additional note embedded inside the description.

### Contact

The contact section contains:

- short collaboration prompt
- email address
- email CTA button
- LinkedIn button
- back-to-top button

### Footer

Footer content is static and minimal:

- copyright line
- implementation note

## Core Reusable CSS Components

### `.card`

The main reusable content panel.

Shared behavior:

- rounded corners
- border
- translucent gradient fill
- hover lift
- hover border enhancement

Variants:

- default card spans 6 columns
- `.wide` spans all 12 columns
- `.third` spans 4 columns

### `.tag`

Small pill label used in card headers and timeline metadata.

### `.meta`

Pill-like metadata row used for keywords and trailing links.

### `.reveal`

Animation helper class used with the Intersection Observer script.

## Responsive Behavior

There are three relevant responsive points:

### `@media (max-width: 980px)`

The hero layout collapses from two columns to one.

### `@media (max-width: 920px)`

Cards collapse to full width and portrait height is reduced.

### `@media (max-width: 520px)`

External link text in the hero can hide to keep the icon row compact.

## Motion and Accessibility

The project includes several motion/accessibility decisions:

- sticky navigation for easier section access
- `prefers-reduced-motion` support
- visible keyboard focus for clickable paper/project cards
- semantic headings and sections
- explicit `aria-label` values on important links and areas
- full-card keyboard activation for paper/project entries

Reduced motion mode disables:

- reveal transitions
- card/button/nav transitions
- caret animation
- progress bar transition smoothing

## JavaScript Runtime Behavior

The inline script at the bottom of the file runs after the DOM for the page has been parsed.

It currently implements four features.

### 1. Year Injection

The footer year is set dynamically:

- target element: `#year`
- value source: `new Date().getFullYear()`

This prevents manual yearly updates.

### 2. Reveal-on-Scroll

Behavior:

- all `.reveal` elements are collected
- an `IntersectionObserver` watches them
- when an element crosses the threshold, it gets the class `.in`

Visual result:

- elements fade in
- elements slide upward slightly

Important note:

- this is one-way activation; the code does not remove `.in` when elements leave the viewport

### 3. Scroll Progress Bar

Behavior:

- the script reads document scroll position
- it computes the ratio of current scroll to total scrollable height
- it writes the percentage width into `#progressBar`

Listeners:

- `scroll`
- `resize`

### 4. Clickable Papers and Projects Cards

Behavior:

- the script selects all cards inside `#pubs` and `#projects`
- it finds the primary anchor within `.meta a[href]`
- it makes the entire card clickable
- it adds keyboard support for `Enter` and `Space`
- it preserves direct anchor clicks so the inline link still works

Important implementation dependency:

- every clickable paper/project card must continue to contain its main destination link inside `.meta a`

If that link is removed or moved elsewhere, the whole-card navigation will stop working for that card.

### 5. Hero Typewriter Effect

Behavior:

- a phrase array stores rotating endings for the sentence
- the script types each phrase character-by-character
- pauses briefly when complete
- deletes it character-by-character
- advances to the next phrase
- loops forever

Key variables:

- `phrases`
- `pi` for current phrase index
- `ci` for current character count
- `deleting` for current direction

## Runtime Sequence

At a high level, the page loads like this:

1. Browser parses metadata, CSS, and JSON-LD
2. Browser renders static structure
3. External fonts begin loading
4. Browser loads local portrait and icon assets
5. Inline script initializes:
   - footer year
   - reveal observer
   - progress bar
   - clickable cards
   - typewriter loop
6. User interaction drives navigation, scrolling, and external-link behavior

## External Dependencies

The page depends on the browser being able to fetch:

- Google Fonts CSS
- Google Fonts font files
- external profile links
- paper links
- project links
- resume link

Without network access:

- local HTML/CSS/JS still works
- local assets still work
- hosted fonts may fall back to system fonts
- external links remain present but cannot load

## Browser Considerations

The CSS uses a few modern capabilities:

- `color-mix()`
- `backdrop-filter`
- CSS custom properties
- `clamp()`
- `prefers-reduced-motion`

The JavaScript uses:

- `IntersectionObserver`
- optional chaining
- `const` / `let`
- arrow functions

This site is intended for modern browsers.

## Architectural Tradeoffs

### Strengths

- easy to understand deployment model
- very low maintenance overhead
- fast to edit
- fast to load
- no toolchain risk

### Weaknesses

- one file mixes concerns
- scaling content makes the document harder to scan
- no reusable templating/component system
- no automated validation or tests

## If The Site Grows

If the project becomes significantly larger, the first refactors to consider are:

1. Move CSS into a standalone stylesheet
2. Move JavaScript into a standalone script file
3. Split repeated card content into data-driven templates or a simple generator
4. Introduce a clearer content source for papers/projects/timeline entries

At the current size, the single-file approach is still reasonable as long as the documentation is kept current.
