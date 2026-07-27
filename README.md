# marketplace-data-compliance

A reusable **Claude Code / Claude Agent skill** that assesses whether collecting data
(scraping, crawling, private APIs, third-party reuse) from marketplaces or websites is
permitted by their **Terms of Service, robots.txt, and official APIs** — and, if not,
what the compliant alternatives are.

マーケットプレイスや Web サイトからのデータ収集（スクレイピング／クローリング／非公開API
の利用／取得データの二次利用）が、各サイトの**利用規約・robots.txt・公式API**に照らして
許容されるかを評価し、許容されない場合は**規約適合的な代替手段**を提示する、再利用可能な
**Claude Code / Claude Agent スキル**です。

> ⚠️ **Not legal advice / 法的助言ではありません.**
> It produces a sourced risk assessment to inform a conversation with a lawyer. It
> separates verified facts from interpretation and never presents guesses as quotes.
> 一次情報に基づくリスク評価を出力し、弁護士に相談するための材料にするものです。事実と
> 解釈を明確に分け、推測を引用として提示しません。最終判断は必ず弁護士にご確認ください。

---

## What it does / できること

Given a list of target sites, the access method, the data you want, and how you'll use
it, the skill:

対象サイト・アクセス方法・取得したいデータ・利用目的を与えると、本スキルは：

1. Fetches each site's **robots.txt** (including separate API hosts) and checks the paths
   you actually target. — 各サイトの **robots.txt**（API ホスト含む）を取得し、実際に叩く
   パスの `Disallow` を確認。
2. Reads the **ToS / User Agreement / guidelines** and quotes clauses on automated access,
   private-API use, reproduction, and commercial/secondary use. — **利用規約・ガイドライン**
   を読み、自動アクセス・非公開API利用・複製・商用/二次利用に関する条項を原文引用。
3. Checks whether an **official API / feed / affiliate / licensed vendor** can supply the
   data legitimately. — **公式API・データ提供・アフィリエイト・正規ライセンス**で取得可能か
   を確認。
4. Notes relevant **legal frameworks** at a high level. — 関連する**法的枠組み**を概説。
5. Outputs a per-site **risk table**, verbatim quotes with sources, an overall risk
   rating, and **ranked compliant alternatives**. — サイト別**リスク表**・出典付き原文引用・
   総合リスク評価・**規約適合的な代替案（優先順）**を出力。

## Install / 導入

**Claude Code** — copy into your skills directory / スキルディレクトリへコピー:

```bash
# project-scoped / プロジェクト単位
cp -r . <your-repo>/.claude/skills/marketplace-data-compliance
# or personal / 個人単位
cp -r . ~/.claude/skills/marketplace-data-compliance
```

Then invoke it by describing the task ("check if we can legally scrape sold prices from
X and Y"), or by name. / タスクを説明するか名前で呼び出します（例：「X と Y から成約価格を
スクレイピングして良いか調べて」）。

## Files / ファイル

- `SKILL.md` — the skill itself (methodology, output format, guardrails). / スキル本体
  （手順・出力フォーマット・ガードレール）。
- `references/report-template.md` — the report skeleton the skill fills in. / 出力レポートの雛形。

## Guardrails / ガードレール

This skill helps you **understand and reduce** risk and find compliant paths. It does
**not** help evade bot detection (IP rotation, UA spoofing, CAPTCHA solving) or otherwise
optimize prohibited access.

本スキルはリスクの**把握と低減**、規約適合的な手段の発見を支援します。ボット検知の回避
（IP ローテーション・UA 偽装・CAPTCHA 回避）など、禁止されたアクセスの最適化は**支援しません**。

## License / ライセンス

Apache License 2.0 — see `LICENSE` and `NOTICE`. / Apache License 2.0（`LICENSE`・`NOTICE`
参照）。Permissive like MIT, with an explicit patent grant and a trademark clause. /
MIT 同様に緩い許容型で、特許ライセンスの明示と商標条項を含みます。

Copyright 2026 Elderithm, Inc.
