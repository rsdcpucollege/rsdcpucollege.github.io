## Repo snapshot (big picture)

- This is a small, static marketing/site repo. The single entry point is `index.html`.
- No build system or server; content is pure HTML/CSS/JS intended to be served as a GitHub Pages site.

## What to know before editing

- All styles and scripts are inline inside `index.html`. Expect changes to CSS/JS to be made directly in that file.
- Key interactive features live in the inline script near the bottom of `index.html`:
  - `scriptURL` — Google Apps Script endpoint used to receive form submissions.
  - Modal handling functions: `openModal(id)`, `closeModal(id)`, and the `.modal` markup pattern.
  - Forms: `applyForm` and `referForm` that call `submitForm(event, 'apply'|'refer')`.
  - Accordion pattern: elements with class `course-item`, `course-header`, and `course-content` controlled by `toggleAccordion`.
  - Reveal-on-scroll: elements use the `reveal` class and an `IntersectionObserver`.

## Important filenames & patterns

- `index.html` — single source of truth.
  - Look for class markers when editing behavior: `modal`, `course-item`, `reveal`, `nav-menu-overlay`, `btn`, `form-input`.
  - Form field names are literal keys sent via `new FormData(form)`. If you rename an input, update the receiving Apps Script or keep the same name.
  - The Google Script endpoint is assigned to `scriptURL` constant. Tests or local debugging often replace this with a mock endpoint.

## Developer workflows (quick)

- Local preview: open `index.html` in a browser (double-click or use a live-server extension). No build required.
- Debugging: open browser devtools (console/network) to inspect event handlers, fetch requests (form POSTs), and JS errors.
- Deploy: push to `main` — GitHub Pages can publish from the `main` branch. There is no CI configured in this repo.

## Integrations and external dependencies

- Google Apps Script: form submissions go to the `scriptURL` value in `index.html`. Keep this URL accurate; changing it will break submissions.
- Embedded Google Map iframe uses a static `src` URL — no API key handling in repo.
- External images referenced but not tracked (e.g., `college.jpg`, `logo.png`) should be added to the repo root or an `assets/` folder; update paths when moved.

## Editing conventions & small rules for AI agents

- Minimal, conservative edits: prefer small, targeted changes to `index.html`. This repo uses no formatter or build step.
- When changing form fields, preserve the existing input `name` attributes unless you also update the Apps Script receiver.
- Use the existing CSS variables at top of `index.html` (e.g., `--primary`, `--secondary`). If adding classes, follow the current naming style: kebab-case, semantic (e.g., `course-item`, `modal-content`).
- For accessibility/UX fixes: prefer adding `aria-*` attributes to existing elements rather than completely restructuring markup.

## Examples (concrete patterns to follow)

- Open modal from anywhere: use openModal('applyModal') — the ID is the modal's DOM id.
- Form submit flow: `submitForm(e, 'apply')` -> sends `new FormData(form)` to `scriptURL`. If you need to mock during tests, set `scriptURL` to a local endpoint or to `https://httpbin.org/post`.
- Accordion: toggleAccordion(this) is attached to `.course-header` elements; to add a new course copy the `course-item` block and keep the nested structure.

## Edge cases & gotchas

- Empty repo assets: referenced images may be missing; check the repo root if visual elements fail to load.
- CORS / network: form POST uses fetch to an external Apps Script; locally this will fail if the script is not configured to accept cross-origin requests. Use real endpoint or mock when testing.

## When to ask the repo owner

- If you plan to migrate scripts off Apps Script (e.g., to a serverless API) — ask for the receiver schema and whether stored submissions are relied on.
- If adding new pages, ask whether they want an `assets/` folder and a lightweight structure for future pages.

---
If anything above is unclear or missing (for example, where images live or the intended GitHub Pages settings), tell me and I will update this file to include those specifics.
