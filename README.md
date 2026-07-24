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

## What the buttons point at right now

The domain gamegoodness.com is not bought yet, so the cards link to the real
deploy URLs the games already have:

| Game                    | Host             | Link used today                                 |
|-------------------------|------------------|-------------------------------------------------|
| Milo and the Good Angel | Vercel           | https://goodness-dialogs-game.vercel.app/       |
| Text Runner             | Cloudflare Pages | https://text-to-game-mvp.pages.dev/             |
| Snake and Ladder        | Vercel           | https://slides-and-ladders-frontend.vercel.app/ |

Heads up: Text Runner sits behind a site password on production. A visitor who
taps that card today may see a password prompt. That gate is intentional and is
not touched here.

## Changing a link later

All three links live in one place. Open `index.html`, find the block labelled
"GAME LINKS - SINGLE SOURCE OF TRUTH", and edit the `data-url` on the card you
want to change. The whole card is one link and the Play button is inside it, so
the two can never drift apart. When the domain goes live you swap each `data-url`
for the matching subdomain. See DEPLOY.md.

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
  game1-thumb.svg   placeholder art, replace with a real screenshot later
  game2-thumb.svg   placeholder art
  game3-thumb.svg   placeholder art
```

## Notes

- The three `gameN-thumb.svg` files are clean brand-coloured placeholders, not
  real screenshots. Replace them with real screenshots when you have them (keep
  the same file names and a 16:10 shape, roughly 640x400).
- Canonical and social (Open Graph) tags point at https://gamegoodness.com/.
  Until that domain is attached, WhatsApp and Slack previews will not fetch the
  card image; they start working once the domain is live.
- No long dashes (em or en) appear anywhere in this repo. Plain hyphens only.
