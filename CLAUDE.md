# CLAUDE.md — Signal Cleveland scrollytelling stories

Guidance for building pinned-photo "scrolly" stories for Signal Cleveland (signalcleveland.org, a Newspack/WordPress site).

## Design conventions (Signal brand)

- Fonts: **Inter** for body, **Roboto Condensed Bold** for headlines/subheads (`--font-body`, `--font-heading`). Headline sizes mirror Newspack's `.entry-title` scale via `--newspack-theme-font-size-*`. Not all-caps except note-card eyebrow labels.
- Palette used so far: text/background slate `#404f54`, teal `#23685b` / light `#51a89a`, accent yellow `#f4c913` (highlights, note rule), accent red `#d64d4d` (emphasis), gray `#879599` (meta text), cream `#f4f0e6` (note card), border `#ccd8db`. Reddit avatar gradients vary per commenter.
- Byline markup copies the live site's `.entry-subhead` / `.entry-meta` classes so it matches theme styles. Photo credit sits directly under it.
- Cards: white, `box-shadow: 0 20px 50px rgba(0,0,0,.4)`, body copy `clamp(1.125rem, 1rem + 0.5vw, 1.3125rem)` (18–21px at default text size) / 1.75. Set every font size in rem/em, never px, so text follows the browser's text-size setting. Desktop cards are capped near `46vw` so the photo stays visible; mobile widens to 82–90vw.
- Accessibility: photo layers use `role="img"` + `aria-label`; static-fallback photos are `aria-hidden`; the real title stays in the DOM (visually hidden) for screen readers.

## Publishing to WordPress (pinned iframe)

Each story is two files: the story itself, uploaded to the Media Library and read as a plain file, and `wordpress-embed-snippet.html`, pasted into a Custom HTML block on the post. The snippet pins a full-screen iframe of the story inside a tall invisible scroll track, and relays how far through that track the reader is; the story does the rest. Its theme overrides — un-sticking the masthead, visually hiding `.entry-header`, hiding the AddToAny row, restoring `overflow: visible` under the single-wide/single-feature templates, and measuring the full-bleed offset in JS rather than the 50vw trick — are all load-bearing on this site and shouldn't be trimmed.

What the two sides say to each other, over pym.js:

- `trackHeight` (story → post): the story's own measured height, so the track is exactly as long as the story and the scroll stays 1:1. Never hand-maintain this number; it goes stale the moment a section is added and the embed then feels far twitchier than the standalone preview.
- `progress` (post → story): 0→1 through the track. A story built on CSS sticky scrolls *itself* to match (easing toward the figure on its own frame loop, since these messages arrive in bursts on a real post); a story built on JS animation draws its frame from it instead, as the John Williams piece does.
- `postMeta` (post → story): the real author, avatar and published date, read off the post's hidden `.entry-header`. Always take the byline from here rather than hardcoding a date.
- `jumpTo` (story → post): a "Jump to" link, or the browser having scrolled the frame on its own to show a keyboard-focused link. The story can't scroll itself to a section — pinned, its scroll position belongs to the track.
- `viewport` (post → story): the reader's real screen height, which the story can't measure from inside a frame that isn't screen-sized.

Two things to get right in a scrolling (rather than JS-animated) story: take the frame's own scrollbar away once connected, or a wheel over the photo scrolls the story out from under the track; and do it from JS, not CSS, so a reader who never got pym.js still has a page that scrolls.

Under `prefers-reduced-motion` there's no rail at all, pinned or otherwise: every photo break drops into normal flow and the post sizes the iframe to the story's full height. That inverts what `vh` means inside the frame — it becomes a share of the story, not of a screen — so every viewport-relative size has to be restated in px or the page grows the frame, which grows the page. The aspect-ratio media queries invert with it and resolve portrait at every width.
