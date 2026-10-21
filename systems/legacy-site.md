# Legacy public site: www.acnnewswire.com

Last updated: 2026-10-22.

## What it is

- Classic ASP pages on IIS (`x-powered-by: ASP.NET`), served through Cloudflare (`server: cloudflare`, not cached: `cf-cache-status: DYNAMIC`) (response headers, 2026-10-22).
- Front end: table layout, Bootstrap 2.2.2, jQuery 1.10.2, fancybox 2.1.5, Font Awesome kit, a third-party widget from `cdn.zzazz.com` (homepage source, 2026-10-22).
- Per-language copies of pages live in separate folders, plus abandoned rebuild attempts (`new1`, `new2`, test variants) in `inetpub` (Kris, Sep 2026).
- Inherited with no documentation and no version control (Kris).

## Known problems

- No mobile layout, no headings, missing alt text on every image, no landmarks. Full list in [the 2026-10-22 research note](../research/2026-10-22-homepage-accessibility-structure.md).
- An `&` in a release title breaks the URL (ASP.NET request validation rejects it) (Kris, Sep 2026).
- Duplicated social links can push URLs past IIS's maximum URL length (Kris, Sep 2026).
- A separate `ACNNewswireMVC` app throws `NullReferenceException`; isolated, not the cause of the outages (Kris, Sep 2026).
- A .NET 8 runtime mismatch on the Swagger endpoint is open (Kris, Sep 2026).

## History

- **Aug 8, 2026 outage:** the app pool hit its memory limit, the worker process hung on shutdown, and IIS Rapid-Fail Protection switched the pool off. Two such site-wide outage windows were confirmed. The slowness and memory growth were traced to the homepage headline page and fixed; the pool's virtual memory limit was then set explicitly (Kris, Sep 2026). Treated as resolved (Kris, 2026-10-22).

## How we change it

Additive only, with backups first. See [decision 0005](../decisions/0005-legacy-changes-are-additive.md).
