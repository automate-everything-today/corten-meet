# Corten Meet

Corten Logistics' booking service at [meet.cortenlogistics.com](https://meet.cortenlogistics.com). It is a modified version of [Tymeslot](https://github.com/Tymeslot/tymeslot), used under the GNU AGPL v3. This repository is the complete corresponding source, as section 13 of the licence requires. "Tymeslot" is a trademark of Diletta Luna OÜ. Corten Meet is not affiliated with or endorsed by them.

## What is changed

Everything Corten-specific is kept small so upstream releases merge cleanly.

| Area | Change | Files |
|---|---|---|
| Styling | Corten CRM design canon: brand pink `#ff3366`, the CRM's warm neutrals, Inter text, Space Grotesk headings, 0.625rem radius | `assets/css/corten/brand.css` (imported last in `assets/css/app.css`) |
| Booking page | New "Corten" palette and font for the Paper theme, made Paper's defaults | `lib/tymeslot/theme_customizations/presets/colour_schemes.ex`, `presets/fonts.ex`, `paper_spec.ex`, `assets/css/scheduling/shared/fonts.css`, `lib/tymeslot_web/themes/shared/font_preload.ex` |
| Fonts | Space Grotesk (SIL OFL) | `priv/static/fonts/space-grotesk-*` |
| Logos and icons | Corten logo, app icon, favicon, social image, email logo | `priv/static/images/brand/corten-*`, `priv/static/favicon.ico`, `lib/tymeslot_web/components/core_components/brand.ex`, layouts |
| Name | "Tymeslot" in user-facing text reads "Corten Meet"; tab titles end "· Corten Logistics" | interface strings across `lib/` |
| Email footer | "© Corten Logistics · Source", linking here | `lib/tymeslot/emails/shared/layouts.ex` |
| Build | Image built by GitHub Actions and published to `ghcr.io/automate-everything-today/corten-meet` | `.github/workflows/corten-image.yml` |

Functional identifiers are deliberately unchanged: module names, webhook headers (`X-Tymeslot-*`), calendar `PRODID`s, user agents and event fingerprints.

## Branches

- `corten`: what runs in production. Pushing to it builds a new image.
- `main`: mirrors upstream Tymeslot.

## Taking an upstream release

1. Sync `main` with upstream (`gh repo sync automate-everything-today/corten-meet -b main`).
2. On a branch from `corten`, merge the release tag (for example `git merge v1.47.0`) and resolve conflicts. Re-run the "Tymeslot" to "Corten Meet" text pass on any new strings.
3. Open a pull request into `corten`, check the screenshots, then merge. The image builds, and Railway is redeployed onto the new tag.

Database migrations run on start-up, so check the upstream changelog before moving more than one release at a time.

## Tests

The upstream suite still asserts upstream branding, so these fail on this
branch by design (44 in `test/tymeslot/emails` and
`test/tymeslot/theme_customizations` at v1.46.0):

- copy that names "Tymeslot" (now "Corten Meet") and the email footer
- the turquoise stock accent and email neutrals (now Corten pink and CRM greys)
- palette and font counts (the Corten palette and font are added)
- the AA check on filled controls: Corten pins white on #ff3366 (3.55:1),
  matching the CRM, rather than letting the palette darken the pink

Anything failing outside those categories is a real regression.
