# Multilingual i18n + SEO Skill

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

Installable **AI agent skill** for auditing, planning, localizing, and verifying multilingual websites—with technical SEO signals (hreflang, canonicals, sitemaps, schema, analytics) and a proper translation playbook.

Not a SaaS product. Not a hosted TMS. No paid API required for the core offline workflow.

## What it is

- Agent entrypoint: [`SKILL.md`](SKILL.md) + [`skill.json`](skill.json)
- Deep docs: SEO checklist, translation playbook, locale architecture, framework adapters
- Offline Python 3.11+ checkers over `dist/` HTML
- Schemas, checklists, prompts, and templates you can drop into any repo

## What it isn’t

- A ranking guarantee
- A substitute for qualified legal review
- Permission to invent testimonials, certifications, or stats
- An excuse for geo-IP / `Accept-Language` force redirects

## Install

### Cursor

```bash
git clone https://github.com/adamgrgs/i18n-agent.git .cursor/skills/multilingual-i18n-seo
# or: git submodule add https://github.com/adamgrgs/i18n-agent.git .cursor/skills/multilingual-i18n-seo
```

Point project rules / `AGENTS.md` at `.cursor/skills/multilingual-i18n-seo/SKILL.md`.  
Details: [`docs/install-cursor.md`](docs/install-cursor.md)

### Claude Code

```bash
mkdir -p .claude/skills
git clone https://github.com/adamgrgs/i18n-agent.git .claude/skills/multilingual-i18n-seo
```

Add a pointer in `CLAUDE.md`. Details: [`docs/install-claude-code.md`](docs/install-claude-code.md)

### Generic agents (Windsurf, Codex, etc.)

Clone into `skills/multilingual-i18n-seo` and `@` / mention [`SKILL.md`](SKILL.md) or [`AGENTS.md`](AGENTS.md).  
Details: [`docs/install-generic.md`](docs/install-generic.md)

## Invoke

Trigger phrases:

- “Use the multilingual i18n SEO skill to audit this repo”
- “Plan fr-CA + es localization with proper hreflang”
- “Translate marketing pages using the translation playbook”
- “Add CI checks for multilingual SEO”

## Quickstart workflow

```text
Discover → Plan → Technical SEO → Translate → Verify
```

1. **Audit** — follow [`prompts/audit.md`](prompts/audit.md); use the checklist in [`docs/multilingual-seo-checklist.md`](docs/multilingual-seo-checklist.md).
2. **Plan** — produce `locale-registry.json` from [`templates/locale-registry.example.json`](templates/locale-registry.example.json).
3. **Implement SEO signals** — [`prompts/implement-technical-seo.md`](prompts/implement-technical-seo.md) + [`docs/framework-adapters.md`](docs/framework-adapters.md).
4. **Translate** — [`docs/translation-playbook.md`](docs/translation-playbook.md) (localize / transcreate / adapt—don’t calque).
5. **Verify**:

```bash
python scripts/validate_locale_registry.py templates/locale-registry.example.json

python scripts/check_i18n_seo.py \
  --root dist \
  --registry path/to/locale-registry.json \
  --sitemap dist/sitemap-0.xml \
  --format both
```

Sample checker output: [`examples/sample-audit-report.md`](examples/sample-audit-report.md)

## Python tooling

Requires **Python 3.11+**. Core scripts use the standard library only.

```bash
pip install -r scripts/requirements.txt   # pytest for tests
python -m pytest tests/ -q
# or: python -m unittest discover -s tests -p 'test_*.py' -v
```

| Script | Purpose |
|--------|---------|
| `scripts/check_i18n_seo.py` | HTML/`dist/` audit (title, canonical, og:url, lang, hreflang, noindex rules, sitemap) |
| `scripts/validate_locale_registry.py` | Registry structure validation |
| `scripts/validate_hreflang.py` | Reciprocity / casing checks |
| `scripts/extract_translatables.py` | Best-effort string inventory helper |

CI template: [`templates/github-actions-seo-check.yml`](templates/github-actions-seo-check.yml)

## Repository layout

See the tree in the skill brief; key paths:

- [`SKILL.md`](SKILL.md) — agent control plane  
- [`docs/`](docs/) — depth  
- [`prompts/`](prompts/) — phase prompts  
- [`checklists/`](checklists/) — machine-readable gates  
- [`schemas/`](schemas/) — JSON schemas  
- [`templates/`](templates/) — copy-paste starters  
- [`scripts/`](scripts/) — offline checkers  
- [`tests/`](tests/) — fixtures + unit tests  

## Hard anti-patterns

- No geo-IP / `Accept-Language` auto-redirects as default  
- Don’t change locked third-party booking/profile URLs unless the user pastes a replacement  
- Don’t emit hreflang on noindex / 404 / placeholders  
- Don’t advertise two public sitemap entrypoints  
- Don’t calque English keyword maps  
- Don’t drop live locales from hreflang while pages stay indexable  

## Contributing

1. Keep `SKILL.md` lean; put depth in `docs/`.  
2. Add fixtures when introducing new checker rules.  
3. Run `python -m pytest tests/ -q` before submitting.  
4. Follow MIT licensing; avoid paid API hard dependencies.

## Disclaimer

Agents can err. Legal and jurisdiction-specific pages need human (often qualified legal) review. SEO outcomes are not guaranteed. This software is provided under the MIT License **as is**—see [`LICENSE`](LICENSE).

## License

MIT © 2026 Multilingual i18n SEO Skill Contributors
