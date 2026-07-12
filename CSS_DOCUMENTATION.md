# CSS Documentation

## Design Tokens
Defined in `:root`:
- Colors: `--clr-bg`, `--clr-surface`, `--clr-surface-2`, `--clr-text`, `--clr-muted`, `--clr-line`, `--clr-accent`, `--clr-accent-2`, `--clr-accent-soft`
- Typography: `--font-display`, `--font-body`

## Layout System
- Main container: `main { width: min(100%, 1300px); }`
- Core cards and panels use grid/flex combinations.
- `side-nav` is fixed to left margin on very wide screens and hidden at narrower widths.

## Major Components
- Hero: gradient panel with portrait and tab controls
- Profile wrap: includes contact-only ear pseudo-elements
  - `body.contact-active .profile-wrap::before/::after` reveals ears
- Tab panels: bordered containers for section content
- Sample cards: thumbnail + content + actions
- Modal: backdrop + dialog + iframe/image preview area

## Animation and Motion
- Panel transition flash was softened by disabling full panel animation.
- Card reveal uses subtle opacity/translate transitions via `.is-visible`.
- Tab switches trigger staggered reveal timing through inline JS-set transition delays.

## Contact Section Styling
- Contact panel is information-only (no decorative image panel).
- Contact links are larger and spaced for readability:
  - `.contact-layout .contact-list a { font-size: 1.08rem; }`
  - `.contact-list { gap: 14px; }`

## Responsive Breakpoints
- `@media (max-width: 1640px)`: hide side-nav
- `@media (max-width: 900px)`: stack core grids to single column
- `@media (max-width: 760px)`: additional compacting for layout blocks
- `@media (max-width: 640px)`: full-width tab buttons and tighter paddings
- `@media (max-width: 560px)`: mobile-specific spacing and modal sizing adjustments

## Cleanup Status
Removed unused legacy selectors from prior iterations, including:
- `.tab-nav`, `.hero-btn`, `.sample-icon`, `.sample-details`, `.cv-card`, `.placeholder`, `.experience-layout`, `.large-card`, duplicate footer rules, and stale breakpoint-only helpers.

Last updated: 2026-07-11
