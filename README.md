# UxNote

[English](README.md) | [Français](README.fr.md)

<p align="center">
  <a href="https://uxnote.ninefortyone.studio">
    <img alt="Docs and custom install on the landing page" src="assets/badges/landing-page.svg" height="40">
  </a>
  <a href="https://www.buymeacoffee.com/ninefortyonestudio">
    <img alt="Buy me a coffee" src="assets/badges/bmc-button%20yallow.svg" height="40">
  </a>
</p>

Uxnote is an annotation bar for mockups and websites. Drop a single script to get text highlights, element pins, numbered cards, color theming, a dimmed focus mode, import/export, and email handoff. No plugin and no backend required.

## Who it is for
- Agencies and freelancers: clients comment directly on the page, then export a clean review file.
- Product and UX teams: review in the browser where interfaces live, without touching existing code.

## Core features
- Text highlights and element pins with numbered badges.
- Element annotation works inside modals and dialogs. Use `↑` / `↓` to pick the layer you mean when several elements are stacked.
- Reviewer name is optional: the comment is what matters.
- Unified or per-type highlight colors. The dim overlay is off by default and can be enabled.
- Import and export to a single JSON file (title + date), with re-import support.
- **Export for AI**: a Markdown brief that tells an AI coding assistant where each note lives and what to change (see below).
- Email handoff for sharing feedback with developers.

## Keyboard shortcuts
- `Alt+V` (`Option+V` on macOS): show or hide the whole Uxnote layer.
- `↑` / `↓` in element mode: move between the elements stacked under the pointer (for example a modal backdrop and the content behind it).

The notes panel starts hidden on every page load. Open it with the panel button in the toolbar.

## Export for AI
The AI export writes a Markdown file (`*-ai.md`) and copies it to the clipboard. It is available in three places:
- the **sparkle** button in the toolbar (all notes);
- the **Export for AI** button in the export dialog (filtered by reviewer and priority);
- the copy button on each note card (one note).

Each note gives the page URL, the element text, a CSS selector, an XPath, the ancestor path, stable attributes (`id`, `data-testid`, `aria-label`, ...), the nearest heading, the enclosing modal when there is one, and the comment. The file also tells the assistant how to locate the element, evaluate the comment, make the smallest change and report back.

## How it works
1. Inject the script on each page (or via a global tag manager).
2. Share the URL with your client.
3. Clients annotate text or elements; everything appears in the Uxnote panel.
4. Export JSON or send by email to collect and process feedback.

## Install (copy/paste)
Place the script right before `</body>` so the DOM is ready. If you must place it in `<head>`, add `defer`.

```html
<script src="https://cdn.jsdelivr.net/gh/marouane-tabib/uxnote@main/dist/uxnote.min.js"></script>
```

The link is served by [jsDelivr](https://www.jsdelivr.com/) from the `dist/uxnote.min.js` file on the `main` branch of `marouane-tabib/uxnote`. No release or version is needed. The file updates when you push a new build to `main`.

## Build
The script is built from `uxnote-tool/uxnote.js` with esbuild.

Requirements: Node.js 18 or newer.

```bash
npm install      # once, installs esbuild
npm run build    # writes dist/uxnote.min.js and dist/uxnote.min.js.map
```

To publish a new build:
1. Run `npm run build`.
2. Commit `uxnote-tool/uxnote.js` and `dist/uxnote.min.js` (and the `.map`), then push to `main`.
3. The link above serves the new file. jsDelivr can keep a copy for a few hours; to refresh it at once, open `https://purge.jsdelivr.net/gh/marouane-tabib/uxnote@main/dist/uxnote.min.js`.

To use the file in a project without the CDN, copy `dist/uxnote.min.js` into your project and reference it locally, for example `<script src="/js/uxnote.min.js"></script>`.

## Script tag options
The landing page builder exposes these options:
- `colorForHighlight` or `colorForTextHighlight` + `colorForElementHighlight`
- `isBackdropVisible` (off by default; set `"true"` to dim the page behind the toolbar)
- `isToolOnTopAtLaunch`
- `isToolVisibleAtFirstLaunch`
- `data-mailto` (recipient for email export)

You can also block areas from annotations with `data-uxnote-ignore`, and re-enable a child with `data-uxnote-allow`.

## Storage and data
Annotations are stored in `localStorage` for the current origin and per URL. No data is sent to a server unless you export a JSON file or send annotations by email.

## Compatibility notes
- Works on staging, previews, or localhost as long as the script loads and `localStorage` is allowed.
- For SPAs, route changes might require a reload or re-init to render annotations for the new URL.
- If CSP is strict, allow the Uxnote script origin and inline styles (or add a nonce/hash).
- Same-origin iframes work if you inject Uxnote inside the iframe document.

## License
Uxnote is released under the MIT License. See `LICENSE`.

## Project layout
- `index.html` - landing page and documentation copy.
- `assets/` - landing styles and language data.
- `uxnote-tool/uxnote.js` - Uxnote tool script (source).
- `scripts/build.js` - esbuild step that produces the minified file in `dist/`.
- `dist/uxnote.min.js` - built script, served by the install link.
- `CHANGELOG.md` - version history.
