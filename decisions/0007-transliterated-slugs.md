# 0007 — Article URLs use transliterated Latin slugs, not native script

**Date:** 2026-10-22
**Decided by:** Krischan
**Status:** accepted

## Context

Newsroom article URLs look like `/article/{language}/{id}/{slug}`. The slug comes from the headline. For Chinese, Japanese and Korean headlines there were two choices:

- write the slug in the headline's own script, or
- transliterate it into Latin letters (the `transliteration` package).

A review on 2026-10-22 found the transliterated slugs messy: words cut mid-way, stray dots, Japanese kana romanised wrongly (ジャパン → `ziyapan`).

## Decision

Keep transliterated Latin slugs, and make them presentable:

- cut at a whole word, at most 80 characters
- only a–z and 0–9
- Hepburn romanisation for Japanese kana
- break words where Latin and CJK text meet

The slug is cosmetic: the route resolves articles by ID, so any slug, old or new, reaches the right release.

## Why

Native-script slugs show up as `%E7%8D%BA%E7%A5%AD…` when a link is pasted into many apps, emails and chats. Clients who pay for distribution should not see that. Latin slugs stay readable everywhere, and the ID keeps routing correct.

What we gave up: Chinese and Japanese readers and search engines would recognise native-script slugs better. Kanji still get Mandarin readings (三菱 → `san-ling`). Fixing that needs a Japanese reading dictionary (kuroshiro/kuromoji), which is deferred.

## Alternatives considered

- Native-script slugs: readable in the address bar, but percent-encoded wherever links are shared.
- No slug for CJK articles (`/article/japanese/106701`): clean, but less descriptive than the English URLs.
- Use the English edition's headline as the slug for its translations: likely the most presentable option. It needs the translation-grouping data, which the API does not have yet. Worth revisiting.
