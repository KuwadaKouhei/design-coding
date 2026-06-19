---
name: readme-generation
description: ワークフローの最終ステップ（レビュー後）で、プロジェクトの README.md を整えるスキル。技術スタック(shieldsバッジ)・概要・思想・環境変数とコマンド一覧・ディレクトリ構成・開発環境構築・トラブルシューティング・ドキュメントの説明とリンクを含める。Use as the final step to generate or polish the project README. 設計ドキュメントとコードから内容を集約し、既存READMEは手書き部分を保持してマージする。
tools: Read, Grep, Glob, Bash, Write, Edit
---

# README 整備（README Generation）

## Overview

開発が一巡した最後に、プロジェクトの顔となる `README.md`（プロジェクトルート）を整える。設計フェーズの
成果物（要件・思想・技術選定・設計）と実際のコード（マニフェスト・スクリプト・ディレクトリ）から内容を
集約し、新規参加者が読んで理解・セットアップできる README にする。**事実をでっち上げず、ドキュメントと
コードの実体から書く**のが原則。

## When to Use

- ワークフローの最終ステップ（`/6-review` の後）
- 公開・引き継ぎ・オンボーディングのために README を整えたいとき
- 技術スタックやコマンドが変わり README が古くなったとき

**前提**: `docs/` の設計成果物があると内容が充実する（最低でも `docs/REQUIREMENTS.md`）。
コード（package.json 等のマニフェスト、スクリプト）があれば併せて参照する。

## Process

### 1. 情報源を集める

- `docs/REQUIREMENTS.md` — 概要・目的・対象ユーザー（→ プロジェクト概要）
- `docs/philosophy/*.md`・`docs/REQUIREMENTS.md` の背景 — 発足理由・工夫点（→ プロジェクト思想）
- `docs/TECH_STACK.md`・マニフェスト（package.json / pyproject.toml / go.mod 等）— 採用技術（→ 技術スタック）
- `.env.example`・設定ファイル・コード — 必要な環境変数（**`.env` は読まない**。`.env.example` か変数名のみ）
- マニフェストの scripts・Makefile・タスクランナ — コマンド一覧
- 実ディレクトリ（`Glob`/`Bash(ls)`）— ディレクトリ構成
- `docs/FEASIBILITY.md` のリスク・既知の問題 — トラブルシューティングの種
- `docs/` 配下の各ドキュメント・`CLAUDE.md` — ドキュメントの説明とリンク

### 2. 技術スタックのバッジ（shields.io）を作る

`docs/TECH_STACK.md` とマニフェストから**実際に採用している技術**を拾い、shields.io バッジで並べる。
バージョンはマニフェストから取り、**憶測で書かない**。

```markdown
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?logo=typescript&logoColor=white)
![React](https://img.shields.io/badge/React-20232A?logo=react&logoColor=61DAFB)
![Node.js](https://img.shields.io/badge/Node.js-339933?logo=nodedotjs&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?logo=postgresql&logoColor=white)
```

> ロゴ名は shields.io / Simple Icons の slug に合わせる（不明なら logo を省く）。CI/カバレッジ等の
> 動的バッジを足すのは任意。存在しないバッジや誤ったバージョンを貼らない。

### 3. README を生成する（既存があればマージ）

ルートに `README.md` を作成する。**既存がある場合は上書きせず**、各セクションを更新しつつ手書き内容は保持する。
下記テンプレートの8セクションを必ず含める。

### 4. README テンプレート

````markdown
# <プロジェクト名>

<!-- 技術スタック（shields バッジ） -->
![<技術1>](https://img.shields.io/badge/...) ![<技術2>](https://img.shields.io/badge/...)

## 概要

<このプロジェクトが何を解決するか／何ができるかを2〜4文で。docs/REQUIREMENTS.md の背景・目的から。>

## プロジェクト思想

- **発足理由**: <なぜ作ったか（背景・課題）>
- **設計・工夫の方針**: <docs/philosophy/* の要点。重視した価値（保守性/性能/シンプルさ等）と工夫点>
- 詳細は [docs/philosophy/](docs/philosophy/) を参照。

## 技術スタック

| 領域 | 採用技術 | 補足 |
|---|---|---|
| 言語 | <…> | <…> |
| フロントエンド | <…> | <…> |
| バックエンド | <…> | <…> |
| データベース | <…> | <…> |
| インフラ/その他 | <…> | <…> |

> 選定理由は [docs/TECH_STACK.md](docs/TECH_STACK.md) を参照。

## 必要な環境変数

| 変数名 | 用途 | 例/デフォルト | 必須 |
|---|---|---|---|
| `<VAR_NAME>` | <用途> | <例。秘密値は載せない> | ✅/任意 |

> `.env.example` をコピーして `.env` を作成する（**実際の秘密値はコミットしない**）。

## コマンド一覧

| コマンド | 説明 |
|---|---|
| `<install>` | 依存関係のインストール |
| `<dev>` | 開発サーバ起動 |
| `<build>` | 本番ビルド |
| `<test>` | テスト実行 |
| `<lint>` | 静的解析/整形 |

## ディレクトリ構成

```
<実ツリー。主要ディレクトリに1行説明>
```

## 開発環境の構築

前提: <Node/ランタイムのバージョン等。docs/TECH_STACK.md 準拠>

```bash
# 1. クローン
git clone <repo-url> && cd <project>
# 2. 依存インストール
<install>
# 3. 環境変数
cp .env.example .env   # 値を設定
# 4. （DBがあれば）マイグレーション/シード
<migrate>
# 5. 起動
<dev>
```

## トラブルシューティング

| 症状 | 原因 | 対処 |
|---|---|---|
| <よくあるエラー> | <原因> | <対処> |

> 既知の制約・リスクは [docs/FEASIBILITY.md](docs/FEASIBILITY.md) も参照。

## ドキュメント

| ドキュメント | 内容 |
|---|---|
| [docs/REQUIREMENTS.md](docs/REQUIREMENTS.md) | 要件定義 |
| [docs/FEASIBILITY.md](docs/FEASIBILITY.md) | 実現可能性調査 |
| [docs/philosophy/](docs/philosophy/) | 設計/実装/テスト思想 |
| [docs/GIT_CONVENTIONS.md](docs/GIT_CONVENTIONS.md) | Git運用方針 |
| [docs/TECH_STACK.md](docs/TECH_STACK.md) | 技術選定 |
| [docs/DESIGN.md](docs/DESIGN.md) | 設計 |
| [docs/DATABASE.md](docs/DATABASE.md) | DB設計・ER図 |
| [docs/REVIEW.md](docs/REVIEW.md) | レビュー・監査結果 |
| [CLAUDE.md](CLAUDE.md) | Claude Code 向けプロジェクトメモリ |

> 上表のうち**実在するファイルのみ**記載・リンクする。
````

### 5. 人間のレビューを受ける

生成・更新した README を確認してもらい、プロジェクト固有の追記（ライセンス・コントリビュート・スクショ等）を反映する。

## Common Rationalizations

| 言い訳 | 実際 |
|---|---|
| 「README は後でいい」 | README は最初に読まれる入口。無いと評価・オンボーディングが止まる。最後に必ず整える。 |
| 「バッジは見栄えだけ」 | 技術スタックの即時把握に有効。ただし誤った技術/バージョンのバッジは害。実体から貼る。 |
| 「環境変数は分かるはず」 | 新規参加者には分からない。変数名・用途・必須可否を一覧化する（秘密値は載せない）。 |
| 「ディレクトリ構成は省略」 | 全体像の地図がないと迷子になる。主要ディレクトリは1行説明付きで載せる。 |
| 「トラブルシュートは思いつかない」 | FEASIBILITY のリスクやセットアップで詰まる箇所から起こせる。空欄でも見出しは残す。 |
| 「docs はあるからリンク不要」 | 散在ドキュメントは辿られない。README から説明付きでリンクして初めて読まれる。 |

## Red Flags

- 指定の8要素（技術スタック/概要/思想/環境変数・コマンド/ディレクトリ構成/構築方法/トラブルシュート/ドキュメント）のいずれかが欠けている
- 採用していない技術や誤ったバージョンのバッジを貼っている
- `.env` の中身や秘密値を README に書いている
- 実在しないファイル・コマンドを記載している（事実のでっち上げ）
- 既存 README の手書き内容を上書きで消している

## Verification

完了とみなす前に確認する:

- [ ] 技術スタック（shields バッジ）が実体に基づいて記載されている
- [ ] 概要・プロジェクト思想（発足理由・工夫点）がある
- [ ] 環境変数一覧（秘密値なし）とコマンド一覧がある
- [ ] ディレクトリ構成（主要部の説明付き）がある
- [ ] 開発環境の構築手順が再現可能な形である
- [ ] トラブルシューティングの節がある
- [ ] 各ドキュメントの説明とリンクがある（実在ファイルのみ）
- [ ] 既存 README の手書き内容を失っていない
- [ ] 人間がレビューした

## フェーズ完了

これでワークフロー（要件定義 → 設計 → CLAUDE.md → 実装 → テスト → レビュー → README整備）が完了する。
以降、技術・コマンド・ドキュメントが変わったら README も更新する（生きた文書）。
