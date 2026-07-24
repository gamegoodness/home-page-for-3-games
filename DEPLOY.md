# Deploy and domain wiring

Status:
- The hub is a static site hosted on Vercel.
- The cards link to the games at their existing deploy URLs.
- The custom domain gamegoodness.com is NOT purchased yet, so the DNS work in
  Part B is DEFERRED. Do none of Part B until the domain exists.

## Part A - Publish the hub on Vercel (do now)

1. Push this repo to GitHub (remote origin is already set).
2. Vercel -> New Project -> Import github.com/gamegoodness/home-page-for-3-games.
3. Settings:
   - Framework Preset: Other
   - Build Command: empty
   - Output Directory: empty (site is at the repo root)
4. Deploy. You get a URL like home-page-for-3-games.vercel.app.
5. Open it and confirm the page loads, all three cards work, and /404 shows the
   friendly not-found page.

Every future push to the main branch redeploys automatically.

## Part B - Domain wiring (DEFERRED until gamegoodness.com is bought)

Plan: the hub sits on the apex, and each game keeps living on its own host, each
reachable at a subdomain.

```
gamegoodness.com        -> the hub (Vercel)
www.gamegoodness.com    -> 301 redirect to the apex
milo.gamegoodness.com   -> Milo and the Good Angel (Vercel)     [name to confirm]
play.gamegoodness.com   -> Text Runner (Cloudflare Pages)       [name to confirm]
snakes.gamegoodness.com -> Snake and Ladder (Vercel)            [name to confirm]
```

You can manage DNS either on Vercel or on Cloudflare. Two clean options:

### Option 1 - DNS on Vercel (simplest, since the hub and two games are on Vercel)

1. Vercel -> hub project -> Settings -> Domains -> add gamegoodness.com and
   www.gamegoodness.com. Vercel shows the nameservers or A/CNAME records to set
   at your registrar. Follow exactly what Vercel prints.
2. For each Vercel game, open that project -> Settings -> Domains -> add its
   subdomain (for example milo.gamegoodness.com), then add the record Vercel
   prints.
3. For Text Runner (Cloudflare Pages), add a CNAME record
   `play -> text-to-game-mvp.pages.dev`, and in the Cloudflare Pages project add
   play.gamegoodness.com as a custom domain.

### Option 2 - DNS on Cloudflare

1. Add gamegoodness.com as a site in Cloudflare (Free plan). Set the two
   Cloudflare nameservers at your registrar. Check with
   `nslookup -type=NS gamegoodness.com 8.8.8.8`.
2. Records:

   | Type  | Name   | Target                       | Proxy            | Why |
   |-------|--------|------------------------------|------------------|-----|
   | A/CNAME| @     | (value Vercel prints for apex)| DNS only (grey) | hub on Vercel |
   | CNAME | www    | (value Vercel prints)         | DNS only (grey) | redirect to apex |
   | CNAME | milo   | (value Vercel prints)         | DNS only (grey) | Vercel, Milo |
   | CNAME | play   | text-to-game-mvp.pages.dev    | Proxied (orange)| Cloudflare, Text Runner |
   | CNAME | snakes | (value Vercel prints)         | DNS only (grey) | Vercel, Snake and Ladder |

   Critical: any record that points at Vercel MUST be grey cloud (DNS only). If
   Cloudflare proxies a Vercel target (orange) you get SSL handshake errors or
   redirect loops. This is the most common way this setup breaks. Use the exact
   target Vercel prints; do not use a value from memory.

### After the subdomains resolve

Edit the three `data-url` values in `index.html` to the subdomain URLs, commit,
and push. Vercel redeploys the hub automatically.

## Things to confirm before go-live

- Final subdomain names (milo / play / snakes are proposals).
- The registrar, so the nameserver steps can be made exact.
- Whether Text Runner should link to production (password gated) or a staging
  build once it is on a subdomain.
- Whether any game hard-codes its own hostname, sets X-Frame-Options or a strict
  CSP, or relies on OAuth callbacks or CORS allowlists that a new hostname would
  break. Report these; do not edit a game repo to fix them.
- Moving a game to a new hostname resets its cookies and localStorage for
  existing players. If a game stores progress or scores locally, decide whether
  to keep the old URL alive too.
