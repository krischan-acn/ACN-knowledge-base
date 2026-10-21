# 2026-10-22 — Homepage accessibility and structure (www.acnnewswire.com)

**Method:** Claude fetched the live homepage HTML and response headers, measured the weight of every image, script and stylesheet on it, and ran axe-core 4 (the open-source accessibility engine many audit tools use) on the HTML. This is **not** a PageSpeed Insights or WAVE run: the sandbox had no browser and could not reach either service. Because no browser ran, there are no speed timings (LCP, CLS), no color-contrast checks, and nothing that only appears after JavaScript runs. Raw output in [files/](files/) with today's date.

## Findings

**Mobile**
- No viewport tag. Phones show the full desktop page zoomed out. This alone fails Google's mobile-friendliness and drags down the PageSpeed mobile score. Biggest single problem.

**Structure and SEO**
- No headings at all: zero `<h1>` to `<h4>`. Search engines and screen readers get no outline of the page.
- No landmarks (`header`, `nav`, `main`, `footer`). Layout is 33 nested tables.
- `<html>` has no `lang` attribute, so screen readers and translation tools have to guess the language, on a site that publishes in five.
- 101 inline `style` attributes and old `<font>`/`<center>` tags.

**Accessibility (axe-core: 7 rules failed, 75 elements)**

| Impact | Count | Problem |
| --- | --- | --- |
| critical | 50 | Images without alt text: every image on the page |
| critical | 1 | The news-type dropdown (`#newsType`) has no label |
| serious | 4 | Social icon links with no text (icon only) |
| serious | 1 | The calendar iframe has no title |
| serious | 1 | Missing `lang` on `<html>` |
| serious | 1 | Search box has no visible label |
| moderate | 17 | Content outside any landmark |

**Weight and loading**
- Page HTML is small (about 59 KB, 14 KB compressed); own images, scripts and CSS add about 155 KB. **Page weight is not the problem.** Guess: PageSpeed will score the desktop version reasonably and the mobile version poorly, mainly because of the missing viewport.
- 48 of 50 images have no width/height and none lazy-load, so the layout can jump while loading.
- Two scripts block rendering in `<head>`. The Font Awesome kit is loaded **twice**.
- jQuery 1.10.2 (2013) and Bootstrap 2.2.2 (2012): both long out of support.
- Cloudflare serves the page but does not cache it (`cf-cache-status: DYNAMIC`), so every visit hits the IIS server.
- Three third-party files could not be measured from here (Font Awesome, the zzazz widget, the featured photo). Guess: they are real but not huge.

## What this means for the project

- The live site's problems are structural, not about weight. Fixing them properly means rebuilding, which is what the newsroom is. That supports keeping the focus on the frontend.
- These findings are the newsroom's "must not repeat" checklist: viewport and responsive layout, one `<h1>` and real headings, landmarks, `lang` per language version, alt text from the data, labelled form fields, image sizes set.
- Cheap fixes possible on the live site in the meantime (additive, per [decision 0005](../decisions/0005-legacy-changes-are-additive.md)): add `lang`, add the viewport tag (check the layout first: on a fixed-width table layout it can make things worse), remove the duplicate Font Awesome load, add `aria-label`s to the icon links, the dropdown and the search box. **Not done; needs a yes.**
- Updated: [client/brand.md](../client/brand.md), [systems/legacy-site.md](../systems/legacy-site.md), [systems/newsroom.md](../systems/newsroom.md).

## Next steps

- Kris: run PageSpeed Insights (mobile and desktop) and WAVE on the homepage, and add the screenshots here. That fills in speed timings and color contrast.
- Run the same checks on the newsroom and on Business Wire for comparison.
- Decide whether to do the cheap live-site fixes above.
