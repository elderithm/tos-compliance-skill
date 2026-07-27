---
name: marketplace-data-compliance
description: Assess whether collecting data (scraping, private APIs, third-party reuse) from marketplaces or websites is allowed by their Terms of Service, robots.txt, and official APIs. Use before building any scraper/crawler/data pipeline, or when reviewing the legal/ToS risk of an existing one. Produces a sourced, per-site risk table and compliant alternatives. Not legal advice.
---

# Marketplace / Website Data-Collection Compliance Check

Assess the **Terms-of-Service, robots.txt, and official-API** situation for collecting
data from one or more websites — **before** you build a scraper, and when auditing an
existing pipeline for legal/ToS risk.

> **This skill produces a risk assessment, not legal advice.** Always separate verified
> facts from interpretation, cite primary sources, and tell the user that a lawyer must
> make the final call. Respond in the user's language.

## When to use

- The user is about to scrape / crawl / hit a private API of a marketplace or site.
- The user asks "is it OK to collect data from X?", "does X allow scraping?", or wants a
  ToS/robots.txt review of a data source.
- You are auditing an existing data pipeline for compliance risk.

## Inputs to gather first

Ask (or infer from the codebase) before researching:

1. **Which sites/platforms** are in scope (exact hostnames, including any API hosts).
2. **How** data is (or will be) accessed: HTML scraping, official API, **private/internal
   API**, headless browser, bulk download, etc. — the access method drives the risk.
3. **What data** is targeted (e.g. sold/completed prices, listings, reviews, profiles).
4. **What use**: internal-only, or **redistributed / shown to third parties / commercial**.
   Third-party display and commercial reuse raise the risk materially.

## Method (per site)

Prefer **primary sources** (the site's own ToS/guidelines/robots.txt and official API
docs). Fetch them directly. Record the URL for every claim. Mark anything you could not
verify as **UNCONFIRMED** — do not guess.

1. **robots.txt** — fetch `https://<host>/robots.txt` (and any separate **API host**).
   Note `Disallow` for the exact paths you target (search, sold/closed, filtered queries).
   Absence of robots.txt ≠ permission.
2. **Terms of Service / User Agreement / Community Guidelines** — quote (verbatim) any
   clause on: automated access / bots / scraping / crawling / data mining, reverse
   engineering, use of "interfaces other than those we provide" (private-API angle),
   reproduction / secondary use / commercial use, and excessive server load.
3. **Official data access** — is there an official API / data feed / affiliate program
   that legitimately returns the target data? If yes, note access requirements
   (approval-gated? paid? redistribution allowed?). If the only official routes return
   *different* data (e.g. new/in-stock prices, not sold prices), say so explicitly.
4. **Case law / statute (jurisdiction-aware, high level)** — note relevant frameworks:
   contract breach (ToS), tort for ignoring access-control measures, unfair-competition /
   trade-secret-style statutes for circumventing access controls (esp. private APIs),
   copyright / database rights, computer-misuse offenses for high-volume access.
   Cite secondary sources and flag they are secondary.

## Output

Produce a Markdown report with, in order:

1. **Disclaimer** (not legal advice; facts vs interpretation; robots.txt as of date).
2. **Per-site summary table**: access-method treatment (prohibited / restricted /
   silent) · robots.txt Disallow for the targeted paths · official data route (yes/no) ·
   primary-source URL.
3. **Verbatim clause quotes** per site (with URLs). UNCONFIRMED where not fetched.
4. **Overall risk rating** per site (high / medium / low) with reasoning tied to the
   access method + intended use.
5. **Compliant alternatives** (official API / affiliate / licensed data vendors /
   deep-link + user-performed lookup / switching to a legitimately obtainable metric),
   ranked for the user's use case.
6. **Primary-source URL list** and an **UNCONFIRMED / follow-up** section.

## Guardrails

- **Do not help evade detection** (IP rotation, UA spoofing to bypass bot protection,
  CAPTCHA solving). Assessing risk and recommending compliant paths is in scope;
  optimizing prohibited access is not.
- Keep facts and interpretation clearly separated. Never present interpretation as a
  verified quote.
- Recommend legal review for any "medium" or "high" rating before shipping.
- Reproduce ToS text only as short quotes needed for the assessment (fair citation).

## Tips

- Use a broad web search to find the current ToS/robots URLs, then fetch the primary
  pages. Help centers behind bot protection may 403 — record that as UNCONFIRMED rather
  than guessing.
- For sites with a separate API host, check **both** hosts' robots.txt.
- See `references/report-template.md` for the output skeleton.
