# marketplace-data-compliance

A reusable **Claude Code / Claude Agent skill** that assesses whether collecting data
(scraping, crawling, private APIs, third-party reuse) from marketplaces or websites is
permitted by their **Terms of Service, robots.txt, and official APIs** — and, if not,
what the compliant alternatives are.

Built after repeatedly needing to answer "are we actually allowed to scrape this?" for a
data-driven product. The methodology is generic and works for any set of sites.

> ⚠️ **Not legal advice.** This skill produces a sourced risk assessment to inform a
> conversation with a lawyer. It separates verified facts from interpretation and never
> presents guesses as quotes.

## What it does

Given a list of target sites, the access method, the data you want, and how you'll use
it, the skill:

1. Fetches each site's **robots.txt** (including separate API hosts) and checks the paths
   you actually target.
2. Reads the **ToS / User Agreement / guidelines** and quotes clauses on automated
   access, private-API use, reproduction, and commercial/secondary use.
3. Checks whether an **official API / feed / affiliate / licensed vendor** can supply the
   data legitimately.
4. Notes relevant **legal frameworks** at a high level.
5. Outputs a per-site **risk table**, verbatim quotes with sources, an overall risk
   rating, and **ranked compliant alternatives**.

## Install

**Claude Code** — copy into your skills directory:

```bash
# project-scoped
cp -r . <your-repo>/.claude/skills/marketplace-data-compliance
# or personal
cp -r . ~/.claude/skills/marketplace-data-compliance
```

Then invoke it by describing the task ("check if we can legally scrape sold prices from
X and Y"), or by name.

## Files

- `SKILL.md` — the skill itself (methodology, output format, guardrails).
- `references/report-template.md` — the report skeleton the skill fills in.

## Guardrails

This skill helps you **understand and reduce** risk and find compliant paths. It does
**not** help evade bot detection (IP rotation, UA spoofing, CAPTCHA solving) or otherwise
optimize prohibited access.

## License

MIT — see `LICENSE`.
