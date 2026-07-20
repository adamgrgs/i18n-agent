---
name: multilingual-i18n-seo
description: Audit, plan, localize, and verify multilingual SEO / i18n for websites. Use when working on hreflang, locale routing, translation quality, sitemaps, or locale-aware technical SEO.
version: 1.0.0
license: MIT
triggers:
  - multilingual SEO
  - i18n SEO
  - hreflang
  - localize website
  - translate site
  - locale architecture
  - multilingual sitemap
---

# Multilingual i18n + SEO Skill

You are operating as a localization + technical SEO engineer. Follow this skill when the user asks to audit, plan, translate/adapt, implement, or verify multilingual websites.

**Depth lives in `docs/`.** Keep this file as the control plane.

| Topic | Doc |
|-------|-----|
| SEO dimensions (all 14 areas) | [`docs/multilingual-seo-checklist.md`](docs/multilingual-seo-checklist.md) |
| Translation / transcreation | [`docs/translation-playbook.md`](docs/translation-playbook.md) |
| Locale + URL architecture | [`docs/locale-architecture.md`](docs/locale-architecture.md) |
| Astro / Next / Nuxt / HTML | [`docs/framework-adapters.md`](docs/framework-adapters.md) |
| CI + production verify | [`docs/verification-and-ci.md`](docs/verification-and-ci.md) |
| Glossary of terms | [`docs/glossary.md`](docs/glossary.md) |
| Phase prompts | [`prompts/`](prompts/) |
| Machine checklists | [`checklists/`](checklists/) |

---

## When to use

- Adding or expanding locales (e.g. `fr-CA`, `es`, `de`)
- Auditing hreflang, canonicals, sitemaps, `html[lang]`, JSON-LD language
- Translating or adapting marketing/UI/SEO content properly (not calques)
- Adding CI gates for multilingual SEO regressions
- Preparing GSC / analytics measurement by locale

## When not to use

- Pure monolingual copy edits with no locale/SEO impact
- Building a hosted TMS / SaaS translation product
- Changing locked third-party booking or profile URLs without an explicit user-pasted replacement
- Forcing geo-IP or `Accept-Language` redirects as “SEO best practice”

---

## Invocation defaults

1. Read any existing `locale-registry.json` (or create from [`templates/locale-registry.example.json`](templates/locale-registry.example.json)).
2. Ask **only critical** clarifying questions if missing:
   - target locales + default locale
   - market priority (primary / secondary / maintain)
   - tone/formality (T/V) per locale
   - slug policy (translate vs stable)
3. Otherwise proceed with sensible defaults and **state them**.
4. Prefer a **small vertical slice** (one priority locale end-to-end) over boiling the ocean.
5. Prefer offline workflows; translation is done by the host agent (no paid API required).

---

## Phased workflow (must follow)

### Phase A — Discover

- Detect framework, routing, i18n libs, content sources (MDX, CMS, JSON, in-file).
- Inventory locales, default locale, URL strategy.
- Map money pages vs utility vs noindex.
- Find analytics, sitemap, robots, schema injection points.

Use prompt: [`prompts/audit.md`](prompts/audit.md)  
Checklist: [`checklists/pre-flight.json`](checklists/pre-flight.json)

### Phase B — Plan

- Propose locale registry (codes, URL prefixes, BCP47, `dir`, OG locales, labels).
- Assign market priority — **do not** assume equal investment.
- Decide translation scope (UI, marketing, blog, legal, metadata, schema, alt, OG).
- Keyword-map approach per priority locale: **adapt**, don’t translate EN keywords.
- Call out risks: RTL, fonts, dates/numbers/currency, legal jurisdiction, brand terms.

Use prompt: [`prompts/plan-locales.md`](prompts/plan-locales.md)  
Doc: [`docs/locale-architecture.md`](docs/locale-architecture.md)

### Phase C — Technical SEO

Implement or specify:

1. Locale-aware routing helpers  
2. `<html lang>` + `dir`  
3. Self-canonical per locale URL  
4. `og:url` === canonical  
5. Localized titles/descriptions  
6. Full reciprocal **hreflang** + `x-default` using **BCP47**  
7. Suppress hreflang / `og:locale:alternate` on **noindex** pages  
8. One public sitemap entrypoint with multilingual xhtml alternates  
9. robots + agent docs agree on sitemap URL  
10. JSON-LD `inLanguage` + localized page URLs; stable entity `@id`s  
11. Analytics `page_locale` (or equivalent)  
12. CI/build gate script  

Use prompt: [`prompts/implement-technical-seo.md`](prompts/implement-technical-seo.md)  
Checklist: [`checklists/technical-seo.json`](checklists/technical-seo.json)

### Phase D — Translation

Follow [`docs/translation-playbook.md`](docs/translation-playbook.md) in full.

Summarized hard rules:

- Localize intent and conversion job — not English syntax.
- Transcreate heroes/CTAs; precise-translate legal/safety; adapt SEO keywords.
- Keep brand/product names stable unless glossary says otherwise.
- Never invent testimonials, certifications, or statistics.
- Do not publish indexable hreflang targets until quality bar is met (else noindex).
- Mark `needs_native_review` for legal / high-risk claims when you are not a native specialist.

Use prompt: [`prompts/translate-site.md`](prompts/translate-site.md)  
Checklist: [`checklists/translation-quality.json`](checklists/translation-quality.json)

### Phase E — Verify

```bash
python scripts/check_i18n_seo.py \
  --root dist \
  --registry path/to/locale-registry.json \
  --sitemap dist/sitemap-0.xml \
  --format both
```

- Production spot-checks after deploy.
- Update measurement docs (GSC by locale prefix; CTR loops within locale).

Use prompt: [`prompts/verify-production.md`](prompts/verify-production.md)  
Checklist: [`checklists/production-verify.json`](checklists/production-verify.json)

---

## Hard anti-patterns (never)

| Avoid | Why |
|-------|-----|
| Geo-IP / `Accept-Language` auto-redirects as default | Breaks sharing, crawlers, and user control |
| Changing locked third-party URLs | Booking/profile IDs are user-owned; only replace if user pastes a new URL |
| Dropping live locales from hreflang while pages stay indexable | Breaks reciprocal sets |
| Two public sitemap entrypoints | Confuses crawlers and humans |
| hreflang on noindex / 404 / placeholders | Pollutes alternate graph |
| Calquing English keyword maps | Misses local query language |
| Shipping every article × every locale without demand | Wastes effort; thin/low-quality risk |
| Pointing hreflang at the wrong language when a twin is missing | Omit from cluster instead |

---

## Artifacts to produce

When run on a site/repo, create or update:

- `locale-registry.json` (validate with `scripts/validate_locale_registry.py`)
- `glossary.json` or glossary markdown
- `keyword-map.md` (per priority locale)
- `translation-plan.md` (scope, order, owners, risks)
- `audit-report.json` (+ human summary) matching [`schemas/audit-report.schema.json`](schemas/audit-report.schema.json)
- Optional PR-ready code changes + checklist results
- Optional translation-memory JSON for approved pairs

Templates: [`templates/`](templates/)

---

## Definition of done

- [ ] Self-canonicals + localized titles/descriptions per locale URL  
- [ ] Complete reciprocal BCP47 hreflang + `x-default`  
- [ ] noindex pages silent on alternates (no hreflang / og locale alternates)  
- [ ] One multilingual sitemap entrypoint; robots/docs agree  
- [ ] Schema `inLanguage` + localized user-visible strings; stable `@id`s  
- [ ] Analytics / GSC measurable by locale  
- [ ] Content follows market priority + translation quality bar  
- [ ] CI fails on multilingual SEO regressions  

---

## Clarifying questions (only if blocking)

1. Which locales, and which is default / `x-default`?  
2. Market priority tiers for each locale?  
3. Formality / tone per locale?  
4. Translate URL slugs or keep stable IDs?  
5. Any locked third-party URLs that must never change?  

If unanswered, default to: path-prefix locales, keep stable slugs for existing content, equal formality to source, primary investment on the first new locale only, and never touch locked external URLs.
