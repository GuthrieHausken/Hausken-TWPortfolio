# HTML Documentation

## Document Structure
- `<main id="main-content">` wraps all primary page content.
- `<header class="hero">` contains portrait, name, tagline, and top tab controls.
- `<nav class="side-nav">` provides wide-screen section switching.
- Five `<section class="tab-panel">` regions are controlled by tabs:
  - `#overview`
  - `#writing`
  - `#contact`
  - `#cv`
  - `#other-works`

## Accessibility and Semantics
- Skip link: `<a class="skip-link" href="#main-content">`
- Tabs follow ARIA tab pattern:
  - tab list: `role="tablist"`
  - tabs: `role="tab"`, `aria-controls`, `aria-selected`
  - panels: `role="tabpanel"`, `aria-labelledby`, `hidden` when inactive
- Modal preview uses dialog semantics:
  - `role="dialog"`
  - `aria-modal="true"`
  - `aria-hidden` state updates when open/close
- Content blocks use semantic elements (`section`, `article`, `header`, `footer`, `ul`, `li`).

## Key Content Regions
- Overview: bio, mission statement, education, strengths
- Portfolio: writing/document cards with thumbnail, summary, preview/open/download
- CV: structured professional profile and sections
- Contact: information-only card with email/LinkedIn/GitHub
- Other works: extra artifacts including poster previews

## Media and File Linking
- Thumbnails are loaded from `thumbnails/`.
- Preview buttons store targets in `data-pdf`.
- The modal supports both image and PDF previews.

## Notable Behavioral Hooks
- Top tabs use `data-tab` values matching panel `id`s.
- Side-nav buttons use `data-sidenav-tab` values and share the same switch logic.
- `body.contact-active` is toggled by JS for contact-only visual effects (profile ears).

Last updated: 2026-07-11
