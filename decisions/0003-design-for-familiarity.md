# 0003 — Design for familiarity and the client's actual buyers

**Date:** 2026-10-22
**Decided by:** Kris (standing default from the client template, kept for ACN)
**Status:** accepted

## Context

Polished, award-style design is not the goal if it makes things harder for the client's buyers to use. Every client's buyers are different, so the design follows who they actually are.

## Decision

Design for the ideal customer profile (ICP), following Jakob's law: users spend most of their time on other sites and expect this one to work the same way. Utilitarian by choice:

- Plain, visible navigation with words, not icons. No hidden menus on desktop, no nested mega menus.
- The main action (call, quote, book, buy, whatever fits this client) and contact details always easy to find.
- Readable type, clear contrast, few clicks to the main action.
- Familiar page patterns over novel ones.

The aim is to compete on "it just works and feels familiar", reducing the visitor's effort, rather than on visual flash.

## How it is applied

1. Confirm the ICP with the client: who buys, their role, industry, country, language, how they get in touch.
2. Check who actually arrives, once data exists. See below.
3. Borrow competitors' substance (credentials, content, how they convert), not their visual style.
4. Record anything client-specific (the ICP, exceptions to this default) in a new decision for this client.

## What the data can and cannot tell us

- **Search Console** shows search queries, countries, devices and landing pages. It does not show age or gender.
- **GA4** can show age, gender and interests only with Google signals on, and hides them below privacy thresholds. Low-traffic sites show little at first.
- For a B2B buyer, **firmographics** matter more than demographics: industry, company size, the buyer's role, country, language. These come mostly from the client and from form fields, not from analytics.
