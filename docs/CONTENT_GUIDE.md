# Content Guide

This document explains how to update the visible content safely.

## Editing Principle

Most edits in this project are direct edits to `index.html`. Because behavior and content are tightly connected, content changes should preserve the existing HTML patterns unless you also intend to update the related CSS or JavaScript.

## Before You Edit

Check these first:

- keep the file in UTF-8
- preserve section IDs used by the navigation
- preserve class names used by CSS/JavaScript
- preserve the `.meta a` link in paper/project cards
- preserve `target="_blank"` and `rel="noreferrer"` on external links when appropriate

## Metadata Updates

At the top of `index.html`, you can update:

- `<title>`
- `<meta name="description">`
- Open Graph tags:
  - `og:title`
  - `og:description`
  - `og:type`
- JSON-LD person data:
  - name
  - job title
  - affiliation
  - email
  - `sameAs` links

Update these when branding, role, affiliation, or profile links change.

## Navigation

The primary nav is an in-page anchor list. Each item must point to an existing section ID.

Current section anchors:

- `#about`
- `#timeline`
- `#pubs`
- `#projects`
- `#contact`

If you rename a section ID, update the matching nav link.

## Hero Section

The hero contains five editable parts:

### 1. Pills

These appear near the top of the hero and communicate focus areas.

### 2. Main headline

Large serif heading under the pills.

### 3. Typewriter sentence

The beginning of the sentence is static:

```text
I build systems that ...
```

The rotating endings come from the `phrases` array in the script. If you want to change the animated text, edit that array.

### 4. Mission paragraph

Longer explanation of research goals and priorities.

### 5. CTA and external links

Includes:

- See projects
- Read papers
- Resume
- GitHub
- LinkedIn
- Scholar
- email

If you change any URL, verify the link still opens correctly.

## Portrait Area

The portrait panel uses:

- `assets/ahan.jpeg`
- overlay styling in CSS
- a visible label inside the image block

To replace the portrait:

1. add the new image to `assets/`
2. update the `src` on the `<img>` in the `.portrait` block
3. verify cropping and alignment
4. adjust `object-position` in CSS if needed

## "Now" Card

This is a short list of current focus areas in the right column beside the hero.

Each item uses:

- a wrapper `<li>`
- a visual `.dot`
- descriptive text

Keep the list concise so the right column does not become visually heavier than the hero card.

## About Section

This section currently contains two cards:

- Research themes
- Tooling

Each card follows the pattern:

```html
<article class="card reveal">
  <div class="cardTop">
    <h3>...</h3>
    <span class="tag">...</span>
  </div>
  <p>...</p>
  <div class="meta">
    <span>...</span>
  </div>
</article>
```

You can add or remove tags inside `.meta` without affecting JavaScript because this section does not use clickable-card behavior.

## Skills Section

This section uses cards with short descriptive paragraphs. It is content-only and not interactive.

Keep these cards:

- compact
- skimmable
- grouped by category rather than chronology

If you add more skill cards, the responsive grid will handle the layout automatically.

## Timeline Section

Each timeline entry uses:

- title in `<b>`
- date pill in `<span>`
- summary paragraph
- bullet list

Recommended pattern:

```html
<div class="tItem">
  <div class="tTop">
    <b>Role - Organization</b>
    <span>Date range</span>
  </div>
  <p>Short summary.</p>
  <ul class="bullets">
    <li>Impact point</li>
  </ul>
</div>
```

Keep timeline bullets focused on outcomes, methods, and scope.

## Papers Section

This section is interactive. Every paper card should preserve the existing structure so the entire card remains clickable.

Recommended pattern:

```html
<article class="card reveal">
  <div class="cardTop">
    <h3>Paper title</h3>
    <span class="tag">Venue or source</span>
  </div>
  <p>Short summary.</p>
  <div class="meta">
    <span>Keyword</span>
    <span>Keyword</span>
    <span>Keyword</span>
    <span><a href="https://..." target="_blank" rel="noreferrer">Link</a></span>
  </div>
</article>
```

### Important rule for paper cards

Do not remove the final anchor from `.meta`.

The JavaScript that makes the whole paper card clickable looks for:

```text
.meta a[href]
```

If you move the main link outside `.meta`, the card will no longer navigate when clicked.

## Projects Section

Projects use `.card.third reveal` so that three cards can sit on one row on wide screens.

Recommended pattern:

```html
<article class="card third reveal">
  <div class="cardTop">
    <h3>Project title</h3>
    <span class="tag">Short label</span>
  </div>
  <p>Short project summary.</p>
  <div class="meta">
    <span>Tech/Theme</span>
    <span>Tech/Theme</span>
    <span>Tech/Theme</span>
    <span><a href="https://..." target="_blank" rel="noreferrer">Code</a></span>
  </div>
</article>
```

### Important rule for project cards

Just like papers, the whole-card click behavior depends on the main destination link staying inside `.meta a`.

### Adding a new project

1. Copy an existing project card
2. Update the title
3. Update the tag
4. Update the description
5. Update keyword pills
6. Update the final external link
7. Refresh and test that clicking anywhere on the card opens the intended destination

## Contact Section

The contact section contains:

- heading
- short collaboration message
- visible email address
- email CTA
- LinkedIn CTA
- back-to-top CTA

If you change the email address, update both:

- visible email text
- `mailto:` URLs

## Footer

The footer year is injected by JavaScript, so you do not need to update it manually.

The remaining footer text is static and can be edited directly.

## Asset Management

### Portrait

- file: `assets/ahan.jpeg`
- used in the hero portrait card

### Icons

- `assets/Icons/github.svg`
- `assets/Icons/linkedin.svg`
- `assets/Icons/scholar.svg`

These are referenced directly in the external links row.

If you replace an icon:

1. keep the filename the same, or
2. update the corresponding `src` path in `index.html`

## Content Style Recommendations

To keep the page visually balanced:

- prefer short paragraphs
- keep card summaries to one or two sentences
- keep tags concise
- avoid overloading one section with too many long bullets
- maintain consistent naming across titles and links

## Safe Editing Checklist

After any content change, verify:

1. nav links still scroll to the right sections
2. papers and projects cards still open correctly
3. no broken image paths
4. no broken external links
5. desktop and mobile layout still look balanced
6. no unintended spacing changes from malformed HTML

## Common Mistakes To Avoid

- deleting a closing tag in the middle of the single large HTML file
- moving a paper/project link outside `.meta`
- changing section IDs without updating navigation links
- replacing the portrait with an image that crops poorly
- making project descriptions much longer than neighboring cards
- introducing smart quotes or encoding issues accidentally
