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
