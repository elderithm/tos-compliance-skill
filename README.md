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

This repo works with both **Claude Code** and **OpenAI Codex** — the same `SKILL.md`
format is shared by both. In Claude Code it is also a **plugin** (skill + `/tos-check`
command); in Codex it installs as a skill plus a matching `/tos-check` custom prompt.

このリポジトリは **Claude Code** と **OpenAI Codex** の両方で使えます（`SKILL.md` 形式は
両者共通）。Claude Code では**プラグイン**（スキル + `/tos-check` コマンド）として、
Codex ではスキル + `/tos-check` カスタムプロンプトとして導入できます。

### Claude Code

#### A) As a plugin (recommended) / プラグインとして（推奨）

In Claude Code / Claude Code 内で:

```text
/plugin marketplace add elderithm/tos-compliance-skill
/plugin install tos-compliance-skill@elderithm
```

This installs both the **skill** (auto-activates when relevant) and the **`/tos-check`
command**. / これで**スキル**（関連時に自動起動）と **`/tos-check` コマンド**の両方が入ります。

#### B) As a skill only (manual copy) / スキルのみ（手動コピー）

```bash
# project-scoped / プロジェクト単位
cp -r skills/marketplace-data-compliance <your-repo>/.claude/skills/marketplace-data-compliance
# or personal / 個人単位
cp -r skills/marketplace-data-compliance ~/.claude/skills/marketplace-data-compliance
```

### OpenAI Codex

Codex discovers skills in `$CODEX_HOME/skills/` (default `~/.codex/skills/`) and custom
prompts in `~/.codex/prompts/`. Copy the skill (and, for the `/tos-check` slash command,
the prompt):

Codex はスキルを `$CODEX_HOME/skills/`（既定 `~/.codex/skills/`）、カスタムプロンプトを
`~/.codex/prompts/` から読み込みます。スキル（と `/tos-check` を使う場合はプロンプト）を
コピーしてください：

```bash
# skill / スキル本体
cp -r skills/marketplace-data-compliance ~/.codex/skills/marketplace-data-compliance
# optional: /tos-check custom prompt / 任意: /tos-check カスタムプロンプト
mkdir -p ~/.codex/prompts && cp prompts/tos-check.md ~/.codex/prompts/tos-check.md
```

Or install straight from GitHub with Codex's own skill-installer / もしくは Codex の
skill-installer で GitHub から直接導入:

```text
Install the marketplace-data-compliance skill from
elderithm/tos-compliance-skill (path: skills/marketplace-data-compliance)
```

The skill **auto-activates** when relevant; `/tos-check` runs it explicitly with
arguments. / スキルは関連時に**自動起動**します。`/tos-check` は引数付きで明示的に実行します。

## Usage / 使い方

**Slash command with placeholders / プレースホルダ付きスラッシュコマンド:**

```text
/tos-check <sites> access=<method> data=<target> use=<internal|third-party/commercial>
```

Placeholders / プレースホルダ:

- `<sites>` — target hostnames (incl. API hosts) / 対象ホスト名（APIホスト含む）
- `access=<method>` — HTML scrape / official API / private API / headless …
- `data=<target>` — e.g. sold prices, listings / 例: 成約価格・出品一覧
- `use=<...>` — internal only / third-party display / commercial / 内部のみ・第三者表示・商用

Example / 例:

```text
/tos-check example.com, api.example.com access=private-api data=sold-prices use=commercial
```

Arguments are optional — if omitted, the command asks for them. You can also just describe
the task in natural language and the **skill** will activate on its own. Works the same in
Claude Code (`/tos-check`) and Codex (`/tos-check`).
引数は任意で、省略すると聞き返します。自然文でタスクを説明すれば**スキル**が自動起動します。
Claude Code・Codex どちらでも `/tos-check` で同じように動きます。

## Files / ファイル

- `.claude-plugin/plugin.json` — plugin manifest / プラグイン定義。
- `.claude-plugin/marketplace.json` — marketplace entry so the repo is installable / この
  リポジトリを marketplace として追加可能にする定義。
- `commands/tos-check.md` — the `/tos-check` slash command for Claude Code / Claude Code 用スラッシュコマンド。
- `prompts/tos-check.md` — the `/tos-check` custom prompt for Codex / Codex 用カスタムプロンプト。
- `skills/marketplace-data-compliance/SKILL.md` — the skill (methodology, output, guardrails)
  / スキル本体（手順・出力・ガードレール）。
- `skills/marketplace-data-compliance/references/report-template.md` — report skeleton / レポート雛形。

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
