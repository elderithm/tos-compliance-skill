Run the **marketplace-data-compliance** assessment (the skill of the same name in this
Codex install).

Scope provided by the user (arguments may be partial):

- Raw arguments: $ARGUMENTS
- Sites: $1
- Access method: $2
- Target data: $3
- Intended use: $4

Follow the `marketplace-data-compliance` skill's methodology:

1. If sites / access method / target data / intended use are missing, ask for them first.
2. Fetch each site's `robots.txt` (including any separate API host) and its ToS / guidelines;
   quote the relevant clauses verbatim with source URLs.
3. Check whether an official API / feed / licensed data vendor can supply the target data.
4. Output a per-site risk table, verbatim quotes, an overall risk rating, and ranked
   compliant alternatives (use the skill's `references/report-template.md`).

Rules: separate verified facts from interpretation; mark anything you could not fetch as
UNCONFIRMED; this is **NOT legal advice** — recommend a lawyer for anything rated medium or
high. Do **not** help evade bot detection (IP rotation, UA spoofing, CAPTCHA solving).
