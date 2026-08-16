# dumoleb.github.io

The developer website for **dumoleb**, served by GitHub Pages at
<https://dumoleb.github.io/>. Plain static HTML, no build step, no dependencies -
edit a file, commit, push, and it is live in about a minute.

Built for Google Play (EPIC-253 / OpenProject #6088 in the Yuki repo).

## What Play needs, and where it lives here

| Play Console field | URL |
|---|---|
| Store listing ▸ **Privacy policy** (App content) | `https://dumoleb.github.io/yuki/privacy/` |
| Store listing ▸ **Website** | `https://dumoleb.github.io/` |
| Store listing ▸ **Support** / contact | `https://dumoleb.github.io/support/` and `dumoleb@gmail.com` |

The privacy policy URL is the one that matters most: **it gates rollout to any
Play track, including internal testing.** Without it you cannot upload at all.

## Why the repo is named `dumoleb.github.io`

Because `app-ads.txt` only works from a domain **root**.

A GitHub *user site* (`<username>.github.io`) serves at `https://dumoleb.github.io/`.
A *project site* would serve at `https://dumoleb.github.io/<repo>/`, putting the
file at `/<repo>/app-ads.txt` - which ad crawlers never look for. They read the
website URL from your Play listing and fetch `/app-ads.txt` from its root, full
stop. Renaming this repo breaks that permanently, so don't.

## Structure

```
/                     landing - who I am, what I make
/yuki/                Yuki product page
/yuki/privacy/        Yuki privacy policy      <- the Play "Privacy policy" URL
/support/             support + contact
/assets/              shared CSS and images
/app-ads.txt.template NOT live - see below
404.html  robots.txt  sitemap.xml  .nojekyll
```

**Per-app, under one domain.** Each future app gets `/<app>/` and
`/<app>/privacy/`. Play asks for a privacy policy URL *per app*, so game #2 gets
its own page here rather than its own domain, its own hosting and its own
`app-ads.txt`.

## `app-ads.txt` is deliberately NOT live

It ships as `app-ads.txt.template`. An app-ads.txt that exists but lists no valid
seller tells every exchange "nobody is authorised to sell my inventory", which
**blocks** ad serving - while a *missing* file is the neutral state. Publishing an
empty one is strictly worse than publishing none, and it fails silently: zero fill,
no error.

Activate it only once a Google Ad Manager / AdSense publisher ID exists (the ad
go-live epic, #3630). The template carries the steps.

## Editing

No build. `assets/site.css` holds everything; the palette mirrors the Yuki design
system (obsidian `#0D0D0F`, sakura `#D4A5A5` / `#FADADD`) so site and app read as
one thing. Light and dark are both handled via `prefers-color-scheme`.

## Two things to revisit

**1. The named data controller.** The policy currently identifies the controller as
**dumoleb**, matching the Play developer name, with `dumoleb@gmail.com` as the
contact. That is the honest reflection of today's setup - a personal account, no
registered business.

Be aware of the limit: data-protection law expects an *identifiable* controller,
and a handle is weaker than a legal or registered business name. If the Play
trader-status step ends up publishing a legal name and address on the listing,
**update this page to match** - a privacy policy that names a different entity from
the store listing is worse than either one alone. If a business is registered later,
the business becomes the controller and this page should say so.

**2. A custom domain.** To move to one, add a `CNAME` file containing the domain,
point DNS at GitHub Pages, and **update the Play listing's Website field** - the
app-ads.txt crawler follows the store listing, so a domain change that skips that
step silently breaks ad verification.
