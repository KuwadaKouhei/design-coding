# Web開発ワークフロー Skills / Commands

Web開発の **「要件定義 → 設計 → 実装 → テスト → レビュー・監査」** を Claude Code のスラッシュコマンド・
スキル・専門ペルソナ（サブエージェント）として体系化したものです。対象は Web エンジニア。
[agent-skills](https://github.com/addyosmani/agent-skills) を参考にしています。

## 特徴: 設計フェーズで「思想」を明確化する

参考プロジェクトとの最大の違いは、**設計フェーズで「設計思想・実装思想・テスト思想」を
インタビュー形式でユーザーから引き出し、`docs/philosophy/` に Markdown として永続化する** 点です。
以降の実装・テストフェーズは、この思想ドキュメントを必ず参照して準拠します。

```
docs/philosophy/
├── PLAN_PHILOSOPHY.md     … 設計思想（アーキ・構造をどう考えるか）
├── CODING_PHILOSOPHY.md   … 実装思想（コードをどう書くか）
└── TEST_PHILOSOPHY.md     … テスト思想（品質をどう担保するか）
```

## ワークフロー全体像

```
/requirements      /design                                   /implement      /test        /review
  要件定義     →     設計                                →     実装      →   テスト   →  レビュー・監査
     │         ┌──────┬─────────┬─────────┐                   │            │            │
     │         │⓪実現可能性 ①思想   ②アーキ設計│                   │            │            │
     │         │  調査    ヒアリング         │                   │            │            │
     ▼         ▼      ▼         ▼         ▼                   ▼            ▼            ▼
REQUIREMENTS FEASIBILITY philosophy/* DESIGN.md ──準拠──▶ コード ──準拠──▶ テスト       REVIEW.md
   .md         .md       (3 docs)            CODING_PHILOSOPHY   TEST_PHILOSOPHY    （4ペルソナ監査）
                  │          ▲                                                            │
        困難なら要件へ差し戻し └──── 思想が後工程を駆動し、最後に思想準拠も監査される ──────┘
```

| フェーズ | コマンド | 起動スキル | 担当ペルソナ | 主な成果物 |
|---|---|---|---|---|
| 要件定義 | `/requirements` | requirements-definition | requirements-analyst | `docs/REQUIREMENTS.md` |
| 設計⓪ 実現可能性 | `/feasibility` | feasibility-study | feasibility-researcher | `docs/FEASIBILITY.md` |
| 設計①②  思想・アーキ | `/design` | feasibility-study → philosophy-definition → design-and-architecture | feasibility-researcher → software-architect | `docs/FEASIBILITY.md`, `docs/philosophy/*.md`, `docs/DESIGN.md` |
| 実装 | `/implement` | implementation | implementer | コード（CODING_PHILOSOPHY 準拠） |
| テスト | `/test` | testing | test-engineer | テスト（TEST_PHILOSOPHY 準拠） |
| レビュー・監査 | `/review` | review-and-audit | code-reviewer / security-auditor / web-performance-auditor / philosophy-compliance-reviewer | `docs/REVIEW.md` |

> `/design` は実現可能性調査（⓪）から思想定義（①）・アーキ設計（②）まで通して実行します。
> `/feasibility` は調査だけを単体で回したいときに使えます。

## 使い方

このリポジトリ（または `.claude/` 一式）を対象プロジェクト直下に置いて Claude Code を開くと、
コマンドとスキルが自動で認識されます。

```bash
/requirements   # 要件をヒアリングし docs/REQUIREMENTS.md を作成
/feasibility    # 要件の技術的実現可能性を裏取り調査し docs/FEASIBILITY.md を作成（/design の冒頭でも実行）
/design         # ⓪実現可能性調査 → ①思想を確定 → ②docs/DESIGN.md 作成
/implement      # DESIGN.md + CODING_PHILOSOPHY.md に準拠して実装
/test           # TEST_PHILOSOPHY.md に準拠してテストを設計・実装
/review         # 4ペルソナでレビュー・監査し docs/REVIEW.md を作成
```

各コマンドは独立して呼べるので、途中フェーズからの再開も可能です。
要件・思想・設計のドキュメントは「生きた文書」として、変更が生じたら更新してください。

## 実行ペルソナ（サブエージェント）

各フェーズには担当ペルソナを `.claude/agents/` に定義しています。コマンドはこのペルソナに `Agent` ツールで
委譲するか、その役割を引き受けて進めます。レビューフェーズは4つの監査ペルソナを並行起動します。

| ペルソナ | 役割 | 使うフェーズ |
|---|---|---|
| `requirements-analyst` | 要件アナリスト | 要件定義 |
| `feasibility-researcher` | 技術調査担当（Web検索で裏取り） | 設計⓪ 実現可能性調査 |
| `software-architect` | 思想ヒアリング＋アーキ設計 | 設計①② |
| `implementer` | ソフトウェアエンジニア | 実装 |
| `test-engineer` | テストエンジニア | テスト |
| `code-reviewer` | 5観点の総合コードレビュー | レビュー |
| `security-auditor` | セキュリティ監査（OWASP 等） | レビュー |
| `web-performance-auditor` | Web性能監査（Core Web Vitals 等） | レビュー |
| `philosophy-compliance-reviewer` | 思想準拠の点検（本ワークフロー固有） | レビュー |

## ディレクトリ構成

```
.
├── README.md
└── .claude/
    ├── commands/
    │   ├── requirements.md
    │   ├── feasibility.md
    │   ├── design.md
    │   ├── implement.md
    │   ├── test.md
    │   └── review.md
    ├── skills/
    │   ├── requirements-definition/SKILL.md
    │   ├── feasibility-study/SKILL.md
    │   ├── philosophy-definition/
    │   │   ├── SKILL.md
    │   │   ├── question-bank.md      # 3思想のインタビュー質問集
    │   │   └── templates.md          # 思想ドキュメントの出力テンプレート
    │   ├── design-and-architecture/SKILL.md
    │   ├── implementation/SKILL.md
    │   ├── testing/SKILL.md
    │   └── review-and-audit/SKILL.md
    └── agents/                       # 実行ペルソナ（サブエージェント）
        ├── requirements-analyst.md
        ├── feasibility-researcher.md
        ├── software-architect.md
        ├── implementer.md
        ├── test-engineer.md
        ├── code-reviewer.md
        ├── security-auditor.md
        ├── web-performance-auditor.md
        └── philosophy-compliance-reviewer.md
```

> `docs/REQUIREMENTS.md` / `docs/FEASIBILITY.md` / `docs/philosophy/*.md` / `docs/DESIGN.md` /
> `docs/REVIEW.md` は、コマンド実行時に対象プロジェクト側に生成される成果物です
> （このリポジトリにはスキル・ペルソナ本体のみが含まれます）。

## 設計思想（このツール自体の方針）

各スキルは参考プロジェクトのアナトミーに従い、以下を備えています。

- **Overview / When to Use** — 何のためのスキルか、いつ使うか
- **Process** — 具体的で実行可能な手順
- **Common Rationalizations** — 「やらない言い訳」と「実際」の対比表
- **Red Flags** — 逸脱の兆候
- **Verification** — 証拠ベースの完了チェックリスト
