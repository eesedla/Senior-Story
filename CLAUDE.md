# CLAUDE.md — Signal Cleveland scrollytelling stories

Guidance for building pinned-photo "scrolly" stories for Signal Cleveland (signalcleveland.org, a Newspack/WordPress site).

## Design conventions (Signal brand)

- Fonts: **Inter** for body, **Roboto Condensed Bold** for headlines/subheads (`--font-body`, `--font-heading`). Headline sizes mirror Newspack's `.entry-title` scale via `--newspack-theme-font-size-*`. Not all-caps except note-card eyebrow labels.
- Palette used so far: text/background slate `#404f54`, teal `#23685b` / light `#51a89a`, accent yellow `#f4c913` (highlights, note rule), accent red `#d64d4d` (emphasis), gray `#879599` (meta text), cream `#f4f0e6` (note card), border `#ccd8db`. Reddit avatar gradients vary per commenter.
- Byline markup copies the live site's `.entry-subhead` / `.entry-meta` classes so it matches theme styles. Photo credit sits directly under it.
- Cards: white, `box-shadow: 0 20px 50px rgba(0,0,0,.4)`, body copy `clamp(18px,1.9vw,21px)` / 1.75. Desktop cards are capped near `46vw` so the photo stays visible; mobile widens to 82–90vw.
- Accessibility: photo layers use `role="img"` + `aria-label`; static-fallback photos are `aria-hidden`; the real title stays in the DOM (visually hidden) for screen readers.
