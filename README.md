# Game Goodness hub

The single home page ("the hub") for Game Goodness. It shows three cards, one
per game, and each card links straight to that game where it already lives.

Plain HTML and CSS. No build step, no framework, no runtime dependencies. The
files in this repo are the site exactly as shipped, served from the repo root.

## Hosting: Vercel (static, zero config)

This repo is a static site. To publish it:

1. Push this repo to GitHub (remote is already set to
   github.com/gamegoodness/home-page-for-3-games).
2. In Vercel: New Project -> Import this GitHub repo.
3. Framework Preset: Other. Build Command: leave empty. Output Directory: leave
   empty (the site is at the repo root). Root Directory: leave as is.
4. Deploy. Vercel serves index.html and uses 404.html for not-found pages
   automatically.

You get a URL like `home-page-for-3-games.vercel.app`. Add the real domain later
(see DEPLOY.md).

## Local preview

No dependencies to install. Either open `index.html` in a browser, or run any
static server, for example:

```
npx serve .
```

## What the buttons point at

The site is live on gamegoodness.gg and each game has its own subdomain:

| Game                    | Host             | Link                                      |
|-------------------------|------------------|-------------------------------------------|
| Milo and the Good Angel | Vercel           | https://milo7story.gamegoodness.gg/       |
| Text Runner             | Cloudflare Pages | https://textrunner.gamegoodness.gg/       |
| Snake and Ladder        | Vercel           | https://laddersandslides.gamegoodness.gg/ |

Heads up: Text Runner sits behind a site password on production, and its
Cloudflare custom-domain certificate can take up to 24 hours to activate. Until
it does, that button will not load. The password gate is intentional and is not
touched here.

## Changing a link later

All three links live in one place. Open `index.html`, find the block labelled
"GAME LINKS - SINGLE SOURCE OF TRUTH", and edit the `data-url` on the row you
want to change. The whole row is one link and the Play button is inside it, so
the two can never drift apart. See DEPLOY.md.

## Files

```
index.html          the hub page
styles.css          single stylesheet, light and dark
404.html            friendly not-found page
robots.txt
sitemap.xml
assets/
  logo.png          the real Game Goodness logo
  og-image.png      1200x630 social preview card (uses the real logo)
  favicon.svg
```

## Notes

- The games are shown as a simple list of rows (title, description, Play button),
  not image tiles, so there are no game thumbnails to maintain.
- Canonical and social (Open Graph) tags point at https://gamegoodness.gg/.
- No long dashes (em or en) appear anywhere in this repo. Plain hyphens only.
