# Portfolio Site V3

A responsive single-page portfolio for technical writing samples, CV content, and contact links.

## Tech Stack
- HTML5 (semantic sections, ARIA tab pattern)
- CSS3 (custom properties, grid/flex, responsive breakpoints)
- Vanilla JavaScript (tab state, keyboard nav, modal preview)

## Current Features
- Accessible tabbed interface with 5 sections: Overview, Portfolio, Contact, CV, Other works
- Side navigation (wide screens) synced with top tab controls
- Modal preview system for PDFs and images
- Keyboard navigation for tabs (Arrow keys, Home/End, Enter/Space)
- Contact-only cat ears decoration on the profile photo
- Responsive layout for desktop/tablet/mobile

## Project Files
- `index.html`: page structure + inline interaction script
- `styles.css`: all styling and responsive behavior
- `HTML_DOCUMENTATION.md`: HTML structure and semantics
- `CSS_DOCUMENTATION.md`: style system and layout rules
- `JAVASCRIPT_DOCUMENTATION.md`: behavior and event logic

## Run Locally
Open `index.html` in a browser.

## Notes
- Preview buttons use `data-pdf` and route to modal image or iframe preview by file extension.
- File names with spaces are URL-encoded in links (e.g., `%20`).

Last updated: 2026-07-11
