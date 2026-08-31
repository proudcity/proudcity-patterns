## 2026.08.31.1619

### Fix search overlay field trapped inside the hero by ancestor containing blocks
References: https://github.com/proudcity/wp-proudcity/issues/2897

On pages that render a Search Box widget inside the hero, `wp-proud-search` skips rendering a second box in the overlay, so `#overlay-search` comes out empty and `.search-active #wrapper-search { position: fixed; z-index: 1051 }` lifts the in-content form over the overlay instead. A `backdrop-filter`, `filter`, `transform`, `perspective`, `contain` or `container-type` on any ancestor makes that ancestor the containing block *and* stacking context for the fixed form, so it stays pinned inside the hero and renders behind the overlay. Cities trigger this from per-site Additional CSS (San Rafael: `backdrop-filter: blur(2px)` on `.jumbotron-bg`).

Changes in `_overlay.scss`:
- Added the `proud-release-fixed-containing-block` mixin, resetting `backdrop-filter`, `filter`, `perspective`, `transform`, `will-change`, `contain` and `container-type` to their initial values. `overflow` is deliberately excluded so wrapper clipping still applies.
- Applied it under `.search-active` across the hero widget subtree (`.panel-banner`, `.wr-element-jumbotronheader`, `.widget-proud-jumbotron-header`), excluding the `.jumbo-image-container` subtree — that branch is a sibling of the search box, never an ancestor of it, and is the one part of the hero that legitimately needs `transform` for the `-50%` image centring in `_jumbotron.scss`.
- Applied it again via `*:has(#wrapper-search)` to catch true ancestors above the hero widget (SiteOrigin rows and stretch wrappers, subtheme containers). Browsers without `:has()` still get the subtree rule, which covers every case seen so far.

## 2026-04-27

### Remove hover drop-shadow styles from navbar
References: https://github.com/proudcity/wp-proudcity/issues/2811

- `app/pattern-scss/_navbar-header.scss` — removed drop-shadow hover styles
- `app/pattern-scss/_navbar.scss` — removed drop-shadow hover styles

## 2026-04-16

### Feature: Mobile menu moved to header region (issue #2757)

On mobile (< 911px), the hamburger and action toolbar no longer pin to the bottom of the viewport. They are now rendered beside the logo in `.navbar-header-region`.

Changes in `_navbar.scss`:
- Split the combined `#main-menu, .menu-box` fixed-bottom rule — `.menu-box` and `.menu-button` are now hidden inside `.navbar-external` at `$mq-nav-xs-mode`
- Added `.menu-close-btn` styles: hidden by default, shown fixed at top-right when `menu-nav-open` is active, white colour

Changes in `_navbar-header.scss`:
- Added `.header-region-menu-box { display: none }` default
- Added `$mq-nav-xs-mode` block: flex layout on `.navbar-header > .container`, shows `.header-region-menu-box` beside logo, applies `hamburger-icon-animate` with reduced height (28px) for the header button, adds "Menu" label colour with light/extra-light background variants, adds `menu-nav-open` active state for hamburger-to-X animation
- Added phone-size logo shrink at `max-width: 480px`

References: https://github.com/proudcity/wp-proudcity/issues/2757
