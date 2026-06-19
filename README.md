# Web開発ワークフロー Skills / Commands

Web開発の **「要件定義 → 設計 → 実装 → テスト → レビュー・監査 → README整備」** を Claude Code の
スラッシュコマンド・スキル・専門ペルソナ（サブエージェント）として体系化したものです。対象は Web エンジニア。
[agent-skills](https://github.com/addyosmani/agent-skills) を参考にしています。

## 特徴: 設計フェーズで「思想」を明確化する

参考プロジェクトとの最大の違いは、**設計フェーズで「設計思想・実装思想・テスト思想」を
インタビュー形式でユーザーから引き出し、`docs/philosophy/` に Markdown として永続化する** 点です。
あわせて **Git運用方針**（管理の有無・コミット粒度・メッセージ規約）もヒアリングして
`docs/GIT_CONVENTIONS.md` にまとめます。以降の実装・テストフェーズは、これらを必ず参照して準拠します。

```
docs/philosophy/
├── PLAN_PHILOSOPHY.md     … 設計思想（アーキ・構造をどう考えるか）
├── CODING_PHILOSOPHY.md   … 実装思想（コードをどう書くか）
└── TEST_PHILOSOPHY.md     … テスト思想（品質をどう担保するか）
docs/GIT_CONVENTIONS.md    … Git運用方針（管理の有無・コミット粒度・メッセージ規約）
```

## ワークフロー全体像

**コマンドは番号順に実行します（番号＝実行順）。**

```
/1-requirements → /2-design → /3-claude-md → /4-implement → /5-test → /6-review → /7-readme
   要件定義         設計        CLAUDE.md生成    実装          テスト    レビュー監査   README整備
      │              │              │             │            │            │            │
      ▼              ▼              ▼             ▼            ▼            ▼            ▼
 REQUIREMENTS    設計成果物群     CLAUDE.md     コード ─準拠─▶ テスト      REVIEW.md     README.md
    .md          (下記⓪〜⑥)   （ドキュメント索引）                                  （技術スタック等8要素）

  迷ったら　▶ 現在地と次の一手を表示: /status　／　次のフェーズへ自動で進む: /next
```

> 「今どこまで終わっていて次に何をすべきか」は **`/status`**（docs/ の成果物から自動判定）。
> 順番を覚えていなくても **`/next`** が次のフェーズを判定して進めます。

`/2-design` は次の7ステップを通して実行します（各ステップは単体コマンドでも実行可）。

```
⓪ 実現可能性 → ① 思想＋Git運用 → ② 技術選定 → ③ アーキ設計 → ④ DB設計（任意） → ⑤ ディレクトリ構造 → ⑥ タスク分解
   FEASIBILITY    philosophy/*       TECH_STACK    DESIGN.md      DATABASE.md(ER図)   DIRECTORY_STRUCTURE   TASKS.md
   /2a-feasibility GIT_CONVENTIONS   /2b-tech-stack               /2c-db-design       /2d-directory-structure /2e-tasks
        │
  困難なら要件へ差し戻し   思想(philosophy/*)とGit運用が実装・テスト・監査を駆動／構造はDIRECTORY_STRUCTURE、実装の駆動表はTASKS.md
```

ディレクトリ構造（⑤）は**常に必ず可読性と拡張性を最優先**で設計し、命名・責務・依存方向・配置ルールを
`docs/DIRECTORY_STRUCTURE.md` に確定します。実装フェーズはこの配置規約に従ってコードを置きます。

実装は **1機能=1ブランチ=1PR**。`docs/TASKS.md` のタスク順に、`main` からブランチを切って実装し、
完了したら `main` へ PR を出し、**人間が承認・マージ**してから次タスクに移ります（`main` は常にグリーン）。

```
実装イテレーション（TASKS.md のタスクごとに繰り返し）
  タスク選択 → main最新化 → feature/<ID> 作成 → 実装(/4) → テスト(/5) → レビュー(/6)
            → push & gh pr create → 【人間がマージ】 → 次タスクへ
```

| 順 | フェーズ | コマンド | 起動スキル | 担当ペルソナ | 主な成果物 |
|---|---|---|---|---|---|
| 1 | 要件定義 | `/1-requirements` | requirements-definition | requirements-analyst | `docs/REQUIREMENTS.md` |
| 2 | 設計（一括） | `/2-design` | feasibility-study → philosophy-definition → technology-selection → design-and-architecture →（必要なら）database-design → directory-structure → task-breakdown | feasibility-researcher → software-architect → tech-selector → software-architect → database-designer → structure-designer → task-planner | 下記すべて |
| 2-⓪ | └ 実現可能性 | `/2a-feasibility` | feasibility-study | feasibility-researcher | `docs/FEASIBILITY.md` |
| 2-① | └ 思想＋Git運用 | （`/2-design` 内） | philosophy-definition | software-architect | `docs/philosophy/*.md`, `docs/GIT_CONVENTIONS.md` |
| 2-② | └ 技術選定 | `/2b-tech-stack` | technology-selection | tech-selector | `docs/TECH_STACK.md` |
| 2-③ | └ アーキ設計 | （`/2-design` 内） | design-and-architecture | software-architect | `docs/DESIGN.md` |
| 2-④ | └ DB設計（任意） | `/2c-db-design` | database-design | database-designer | `docs/DATABASE.md`（ER図含む） |
| 2-⑤ | └ ディレクトリ構造 | `/2d-directory-structure` | directory-structure | structure-designer | `docs/DIRECTORY_STRUCTURE.md`（可読性・拡張性最優先） |
| 2-⑥ | └ タスク分解 | `/2e-tasks` | task-breakdown | task-planner | `docs/TASKS.md`（実装の駆動表） |
| 3 | CLAUDE.md生成 | `/3-claude-md` | claude-md-generation | claude-md-author | `CLAUDE.md`（ルート） |
| 4 | 実装 | `/4-implement` | implementation | implementer | コード（1機能=1ブランチ、CODING_PHILOSOPHY 準拠） |
| 5 | テスト | `/5-test` | testing | test-engineer | テスト（TEST_PHILOSOPHY 準拠） |
| 6 | レビュー・監査 | `/6-review` | review-and-audit | code-reviewer / security-auditor / web-performance-auditor / philosophy-compliance-reviewer | `docs/REVIEW.md` |
| 7 | README整備 | `/7-readme` | readme-generation | readme-author | `README.md`（技術スタック等8要素） |
| — | 現在地確認 | `/status` | workflow-status | （メインループ） | 進捗の一覧表示 |
| — | 次へ進む | `/next` | workflow-status | （メインループ） | 次フェーズの判定・起動 |

## Git運用（1機能=1ブランチ＝1PR）

`main` は常にグリーン（動く状態）を保ちます。実装は `docs/TASKS.md` の機能タスク単位で進めます。

- **設計⑥ タスク分解（`/2e-tasks`）** で、設計を機能タスクへ分解し、実装順・依存・推奨ブランチ名
  （`feature/<ID>-<slug>`）を `docs/TASKS.md` に確定します。
- **実装（`/4-implement`）** は次タスクを選び、`main` からそのブランチを切って実装します。
  `main` には直接コミットしません。
- **レビュー（`/6-review`）** がマージ可になったら、`gh pr create` で `main` への PR を提出します。
- **マージは必ず人間** が承認して行います（AIはマージしません）。マージ後に次タスクへ移ります。

ブランチ命名・PR必須・マージ方針は `docs/GIT_CONVENTIONS.md` に明文化されます（Git を使わない選択も可能）。

## 使い方

このリポジトリ（または `.claude/` 一式）を対象プロジェクト直下に置いて Claude Code を開くと、
コマンドとスキルが自動で認識されます。

```bash
/status         # 【迷ったらこれ】今どこまで終わって次に何をすべきかを表示
/next           # 次のフェーズを自動判定して提案・起動（順番を覚えなくてOK）

/1-requirements # 要件をヒアリングし docs/REQUIREMENTS.md を作成
/2-design       # ⓪実現可能性 → ①思想+Git運用 → ②技術選定 → ③アーキ設計 → ④DB設計（必要時）→ ⑤ディレクトリ構造 → ⑥タスク分解 を通しで実行
  /2a-feasibility # （単体）実現可能性調査 → docs/FEASIBILITY.md
  /2b-tech-stack  # （単体）技術選定 → docs/TECH_STACK.md
  /2c-db-design   # （単体）DB設計（必要時）→ docs/DATABASE.md（Mermaid ER図含む）
  /2d-directory-structure # （単体）ディレクトリ構造設計（可読性・拡張性最優先）→ docs/DIRECTORY_STRUCTURE.md
  /2e-tasks       # （単体）タスク分解 → docs/TASKS.md（1機能=1ブランチ=1PR の駆動表）
/3-claude-md    # 設計完了後・実装前に、各ドキュメント索引と運用ルールを記した CLAUDE.md を生成
/4-implement    # TASKS.md の次タスクを main からブランチを切って実装（1機能=1ブランチ、各規約に準拠）
/5-test         # TEST_PHILOSOPHY.md に準拠してテストを設計・実装
/6-review       # 4ペルソナでレビュー・監査し docs/REVIEW.md を作成 → マージ可ならPR提出（人間がマージ）
/7-readme       # 【最終】技術スタック(shields)等8要素を含む README.md を整える
```

各コマンドは独立して呼べるので、途中フェーズからの再開も可能です。**現在地が分からなくなったら `/status`、
次に進みたいときは `/next`** を使ってください。要件・思想・設計のドキュメントは「生きた文書」として、
変更が生じたら更新してください。

## 実行ペルソナ（サブエージェント）

各フェーズには担当ペルソナを `.claude/agents/` に定義しています。コマンドはこのペルソナに `Agent` ツールで
委譲するか、その役割を引き受けて進めます。レビューフェーズは4つの監査ペルソナを並行起動します。

| ペルソナ | 役割 | 使うフェーズ |
|---|---|---|
| `requirements-analyst` | 要件アナリスト | 要件定義 |
| `feasibility-researcher` | 技術調査担当（Web検索で裏取り） | 設計⓪ 実現可能性調査 |
| `tech-selector` | 技術選定担当 / テックリード（Web検索で裏取り） | 設計② 技術選定 |
| `software-architect` | 思想ヒアリング＋アーキ設計 | 設計①③ |
| `database-designer` | DB設計担当（スキーマ＋Mermaid ER図） | 設計④ DB設計（必要時） |
| `structure-designer` | ディレクトリ構造設計担当（可読性・拡張性最優先） | 設計⑤ ディレクトリ構造設計 |
| `task-planner` | タスクプランナー（機能単位に分解・順序付け） | 設計⑥ タスク分解 |
| `claude-md-author` | CLAUDE.md 作成担当（ドキュメント索引＋運用ルール） | 設計後・実装前 |
| `readme-author` | README 整備担当（技術スタック・思想・構築手順等） | README整備（最終） |
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
    ├── commands/                     # 番号＝実行順
    │   ├── 1-requirements.md
    │   ├── 2-design.md
    │   ├── 2a-feasibility.md
    │   ├── 2b-tech-stack.md
    │   ├── 2c-db-design.md
    │   ├── 2d-directory-structure.md
    │   ├── 2e-tasks.md
    │   ├── 3-claude-md.md
    │   ├── 4-implement.md
    │   ├── 5-test.md
    │   ├── 6-review.md
    │   ├── 7-readme.md
    │   ├── status.md                 # 現在地確認（順序外）
    │   └── next.md                   # 次フェーズへ誘導（順序外）
    ├── skills/
    │   ├── requirements-definition/SKILL.md
    │   ├── feasibility-study/SKILL.md
    │   ├── philosophy-definition/
    │   │   ├── SKILL.md
    │   │   ├── question-bank.md      # 3思想＋Git運用のインタビュー質問集
    │   │   └── templates.md          # 思想＋GIT_CONVENTIONS の出力テンプレート
    │   ├── technology-selection/SKILL.md
    │   ├── design-and-architecture/SKILL.md
    │   ├── database-design/SKILL.md
    │   ├── directory-structure/SKILL.md
    │   ├── task-breakdown/SKILL.md
    │   ├── claude-md-generation/SKILL.md
    │   ├── implementation/SKILL.md
    │   ├── testing/SKILL.md
    │   ├── review-and-audit/SKILL.md
    │   ├── readme-generation/SKILL.md
    │   └── workflow-status/SKILL.md  # /status・/next が共用
    └── agents/                       # 実行ペルソナ（サブエージェント）
        ├── requirements-analyst.md
        ├── feasibility-researcher.md
        ├── tech-selector.md
        ├── software-architect.md
        ├── database-designer.md
        ├── structure-designer.md
        ├── task-planner.md
        ├── claude-md-author.md
        ├── readme-author.md
        ├── implementer.md
        ├── test-engineer.md
        ├── code-reviewer.md
        ├── security-auditor.md
        ├── web-performance-auditor.md
        └── philosophy-compliance-reviewer.md
```

> `docs/REQUIREMENTS.md` / `docs/FEASIBILITY.md` / `docs/philosophy/*.md` / `docs/GIT_CONVENTIONS.md` /
> `docs/TECH_STACK.md` / `docs/DESIGN.md` / `docs/DATABASE.md` / `docs/DIRECTORY_STRUCTURE.md` / `docs/TASKS.md` / `docs/REVIEW.md`、および設計完了後に
> 生成する **プロジェクトルートの `CLAUDE.md`** は、コマンド実行時に対象プロジェクト側に生成される成果物です
> （このリポジトリにはスキル・ペルソナ本体のみが含まれます）。

## 設計思想（このツール自体の方針）

各スキルは参考プロジェクトのアナトミーに従い、以下を備えています。

- **Overview / When to Use** — 何のためのスキルか、いつ使うか
- **Process** — 具体的で実行可能な手順
- **Common Rationalizations** — 「やらない言い訳」と「実際」の対比表
- **Red Flags** — 逸脱の兆候
- **Verification** — 証拠ベースの完了チェックリスト
