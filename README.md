# Blue Bison Media — site source

The complete source for **blue-bison-media.netlify.app**. This repo exists so the
site can be rebuilt or redeployed from scratch without pulling it back off the live
server.

```
public/          everything served at the site root
  *.html         the 12 pages
  vid/           sample videos + their poster frames
  looks/         look-book stills
  adkit/         ad kit images, thumbnails and the downloadable zip
  og/            social preview images
  *.pdf          brochures and one-pagers
netlify.toml     redirects, security headers and cache rules
```

## Deploying

Netlify site ID: `8f13223e-aa19-4635-bcfe-d58b42bea1c5`

**A deploy replaces the whole site.** Netlify builds the deploy from exactly the
files you upload — anything missing from `public/` disappears from the live site.
Never deploy a partial folder.

From this directory:

```bash
npx -y @netlify/mcp@latest --site-id 8f13223e-aa19-4635-bcfe-d58b42bea1c5 \
    --proxy-path "<short-lived token>"
```

## Before every deploy

1. **Reference check** — every `src`/`href`/`poster` starting with `/` must exist
   under `public/`. This is the check that catches a missing asset; file counts and
   tag balance will not.
2. **Containment check** — count cards *inside* their grid in a real browser, not
   `<div>` opens against closes. Cards have twice landed outside a grid with the
   div count perfectly balanced.
3. **Spelling** — American throughout the marketing copy (`license`, not `licence`).

## Things that are easy to break

- `/formats` is a rewrite to `shows.html`, not a page of its own. Netlify maps
  other extensionless paths to `.html` automatically; this one does not match.
- Forms (`visibility-intake`, `license`, `reserve`) are registered by Netlify
  scanning the deployed HTML at build time. A form needs `data-netlify="true"`,
  a hidden `form-name` input, and the honeypot field, or it silently will not
  appear in the Netlify dashboard.
- Several sample videos and posters are hotlinked from the individual show sites
  (`the-verdict-board.netlify.app`, `school-district-tv.netlify.app`,
  `newzapp-tv.netlify.app`, and others). They are not in this repo, so a check
  that only looks at local paths will not notice if one of those sites moves.
- Video lives in ordinary git, not LFS — deliberately, because LFS bandwidth is
  metered per clone. Replacing a video keeps the old copy in history, so if the
  repo gets heavy, prune then rather than paying the LFS tax now.
