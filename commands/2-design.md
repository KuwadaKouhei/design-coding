---
description: STEP2 設計（一括）｜実現可能性調査(FEASIBILITY)→思想とGit運用(philosophy/・GIT_CONVENTIONS)→技術選定(TECH_STACK)→アーキ設計(DESIGN)→必要ならDB設計(DATABASE)。要件定義(STEP1)の後に実行
---

> **STEP2 / 全6フェーズ**　前提: `docs/REQUIREMENTS.md`（STEP1完了）　｜　現在地確認: `/status`
>
> このコマンドは設計を**通しで**実行する。各ステップは単体コマンドでも実行可能:
> ⓪`/2a-feasibility`　②`/2b-tech-stack`　④`/2c-db-design`　⑤`/2d-directory-structure`　⑥`/2e-tasks`
> （①思想・③アーキはこの一括コマンド内で実施）。

設計フェーズは「実現可能性調査」→「思想定義」→「技術選定」→「アーキテクチャ設計」→「DB設計（必要な場合）」
→「ディレクトリ構造設計」→「タスク分解」の7段で進める。ステップ0は `feasibility-researcher`、ステップ2は
`tech-selector`、ステップ4は `database-designer`、ステップ5は `structure-designer`、ステップ6は `task-planner`、
ステップ1と3は `software-architect` が担当する（`Agent` ツールで各サブエージェントに委譲するか、役割を引き受けて進める）。

## コミット運用（このフェーズ共通）

**1フェーズ＝1コミット。** 各ステップ完了時、そのフェーズで新規作成・更新した `docs/` のドキュメントを
**まとめて1つのコミット**にする（Gitを使う場合）。フェーズをまたいで変更を1コミットに混ぜない。
例: ステップ1は `docs/philosophy/*.md`（3ファイル）と `docs/GIT_CONVENTIONS.md` を**計4ファイルで1コミット**にまとめる。
コミットメッセージは `docs/GIT_CONVENTIONS.md` の規約に従う。まだ確定していない段階（⓪実現可能性など）は
Conventional Commits の `docs:` を既定とする（例: `docs: 実現可能性調査(FEASIBILITY)を追加`）。
コミット先ブランチ・`main` 直コミットの可否は `GIT_CONVENTIONS.md` の決定に従う。Git未使用ならコミットしない。

## ステップ0: 実現可能性調査（feasibility-study スキル / feasibility-researcher）

`docs/FEASIBILITY.md` があるか確認する。なければ `feasibility-study` スキルを起動して作成する。

1. `docs/REQUIREMENTS.md` と既存コード・依存を読み、要件を検証可能な問いに分解する
2. 各要件の実現可能性を社内コードと外部情報（WebSearch/WebFetch）で裏取り調査する
3. リスク・不確実性・PoC要否を洗い出し、技術選定の比較・推奨を整理する
4. 各要件を 実現可能 / 条件付き可能 / 困難・要再検討 で判定し、根拠（出典つき）を残す
5. `docs/FEASIBILITY.md` に保存する。「困難」判定があれば `/1-requirements` への差し戻しを促し、
   実現性が確認できてからステップ1へ進む
6. （Gitを使う場合）このフェーズの成果（`docs/FEASIBILITY.md`）をまとめて1コミットする（例 `docs: 実現可能性調査(FEASIBILITY)を追加`）

## ステップ1: 思想の確定 ＋ Git運用方針（philosophy-definition スキル）

`philosophy-definition` スキルを起動する。

1. `docs/philosophy/` に3思想ファイルがあるか確認する。あれば更新の要否をユーザーに確認する
2. なければ **インタビュー形式で1問ずつ** 設計思想・実装思想・テスト思想を引き出す
   （`question-bank.md` の質問を使い、各問いに推奨デフォルトを添える）
3. 回答を要約してユーザーの承認を得る
4. `templates.md` に沿って以下を書き出す:
   - `docs/philosophy/PLAN_PHILOSOPHY.md`（設計思想）
   - `docs/philosophy/CODING_PHILOSOPHY.md`（実装思想）
   - `docs/philosophy/TEST_PHILOSOPHY.md`（テスト思想）
5. 続けて **Git運用方針** をヒアリングする（`question-bank.md` のセクションD）。まず Git管理の有無を聞き、
   使う場合のみコミット粒度・コミットメッセージ規約（と関連してブランチ運用）を深掘りする
6. `docs/GIT_CONVENTIONS.md` を書き出す（シークレット・`.env` のコミット禁止を必ず明記。Git不使用なら理由を記録）
7. （Gitを使う場合）このフェーズの成果（`docs/philosophy/*.md` の3ファイル ＋ `docs/GIT_CONVENTIONS.md`）を
   **まとめて1コミット**にする（例 `docs: 設計/実装/テスト思想とGit運用方針を追加`）

## ステップ2: 技術選定（technology-selection スキル / tech-selector）

`docs/TECH_STACK.md` があるか確認する。なければ `technology-selection` スキルを起動して作成する。

1. `docs/REQUIREMENTS.md`・`docs/philosophy/PLAN_PHILOSOPHY.md`・`docs/FEASIBILITY.md`・既存スタックを読む
2. 選定領域（言語 / FW / DB / 状態管理 / 認証 / インフラ / テスト・CIツール 等）を洗い出す
3. 各領域で候補（類似技術スタック）を評価軸で比較する。最新状況は WebSearch/WebFetch で裏取りする
4. 採用技術を決定し、「選定技術」「選定理由」「類似技術と比べてなぜ選んだか（不採用理由含む）」を残す
5. `docs/TECH_STACK.md` に保存し、人間のレビューを受ける
6. （Gitを使う場合）このフェーズの成果（`docs/TECH_STACK.md`）をまとめて1コミットする（例 `docs: 技術選定(TECH_STACK)を追加`）

## ステップ3: アーキテクチャ設計（design-and-architecture スキル）

`design-and-architecture` スキルを起動する。

1. `docs/REQUIREMENTS.md`・`docs/philosophy/PLAN_PHILOSOPHY.md`・`docs/FEASIBILITY.md`（制約・リスク・
   推奨技術）・`docs/TECH_STACK.md`（採用技術）を読む
2. アーキテクチャ概要・コンポーネント・データモデル（概念）・API/IF・処理フロー・トレードオフを設計する
3. 思想からの逸脱があれば理由を明記する
4. `docs/DESIGN.md` に保存し、人間のレビューを受ける
5. （Gitを使う場合）このフェーズの成果（`docs/DESIGN.md`）をまとめて1コミットする（例 `docs: アーキテクチャ設計(DESIGN)を追加`）

## ステップ4: DB設計（database-design スキル / database-designer）※必要な場合のみ

`database-designer` に委譲し、要件達成に**永続データが必要かを判断**する。不要なら理由を残してスキップする。
必要な場合は `docs/DATABASE.md` を作成する。

1. `docs/REQUIREMENTS.md`・`docs/TECH_STACK.md`（採用DB）・`docs/philosophy/PLAN_PHILOSOPHY.md`・
   `docs/DESIGN.md`（概念データモデル）・既存スキーマを読む
2. エンティティ・属性・関連・カーディナリティを抽出する
3. スキーマ（テーブル/コレクション・カラム・型・NULL可否・制約・PK/FK・インデックス）を設計する。
   正規化/非正規化は思想の優先度に基づき理由を残す
4. データ関連の受け入れ条件がスキーマで満たせることを確認する
5. **ER図を Mermaid（`erDiagram`）で作成**し、`docs/DATABASE.md` にまとめて人間のレビューを受ける
6. （Gitを使う場合）このフェーズの成果（`docs/DATABASE.md`。不要判断時はその記録）をまとめて1コミットする（例 `docs: DB設計(DATABASE)を追加`）

## ステップ5: ディレクトリ構造設計（directory-structure スキル / structure-designer）

`structure-designer` に委譲し、確定した設計を**物理的なディレクトリ構造**へ落とす。ディレクトリ構造は
**常に必ず可読性と拡張性を最優先**で設計し、`docs/DIRECTORY_STRUCTURE.md` を作成する。

1. `docs/DESIGN.md`（論理構成）・`docs/TECH_STACK.md`（FW規約）・`docs/DATABASE.md`（あれば）・
   `docs/philosophy/PLAN_PHILOSOPHY.md`（境界・依存方向）・既存コードの実構造を読む
2. 構造の基調（レイヤー型 / 機能型 / ドメイン型）を決め、採用FWの標準レイアウトと整合させる
3. **可読性のルール**（命名規約・1ディレクトリ1責務・階層の深さ・テスト/型の同居or分離）を定義する
4. **拡張性のルール**（新要素の追加先が一意に決まる配置・依存方向・`shared`/`common` の肥大化抑制）を定義する
5. ディレクトリツリーと各ディレクトリの責務表を書き、「新しい〇〇を足す」例で配置に迷いが出ないか自己検証する
6. 思想・FW規約からの逸脱があれば理由を明記し、`docs/DIRECTORY_STRUCTURE.md` にまとめて人間のレビューを受ける
7. （Gitを使う場合）このフェーズの成果（`docs/DIRECTORY_STRUCTURE.md`）をまとめて1コミットする（例 `docs: ディレクトリ構造(DIRECTORY_STRUCTURE)を追加`）

## ステップ6: タスク分解（task-breakdown スキル / task-planner）

`task-planner` に委譲し、確定した設計を**機能単位のタスク**へ分解する。`docs/TASKS.md` を作成する。

1. `docs/DESIGN.md`・`docs/REQUIREMENTS.md`（受け入れ条件）・`docs/DATABASE.md`（あれば）・
   `docs/DIRECTORY_STRUCTURE.md`（あれば／配置先の参考）・`docs/GIT_CONVENTIONS.md`（ブランチ命名規約）を読む
2. 機能を **1タスク=1機能=1ブランチ=1PR** の粒度（縦スライス）に分解し、ID を採番する
3. 各タスクに 概要・対応する受け入れ条件・依存タスク・推奨ブランチ名（`feature/<ID>-<slug>`）・状態を付ける
4. 依存を満たす実装順に並べる（各タスクのマージ時点で `main` がグリーンに保てる順）
5. `docs/TASKS.md` にまとめ、人間のレビューを受ける。この一覧が実装フェーズの駆動表になる
6. （Gitを使う場合）このフェーズの成果（`docs/TASKS.md`）をまとめて1コミットする（例 `docs: タスク分解(TASKS)を追加`）

## 設計完了後（実装に入る前）: CLAUDE.md 生成

設計フェーズが完了したら、**実装に入る前に** `/3-claude-md`（`claude-md-generation` スキル /
`claude-md-author`）を実行し、プロジェクトルートに `CLAUDE.md` を生成する。CLAUDE.md には各設計
ドキュメントの説明と、「実装はドキュメントに沿う」「修正時はコードとドキュメントを一緒に更新する」運用ルールを
記述する。これにより実装フェーズが常にドキュメント駆動になる。

---
- 完了の目印: `docs/FEASIBILITY.md`・`docs/philosophy/*`・`docs/GIT_CONVENTIONS.md`・`docs/TECH_STACK.md`・`docs/DESIGN.md`・`docs/DIRECTORY_STRUCTURE.md`・`docs/TASKS.md`（＋必要なら`docs/DATABASE.md`）
- ▶ 次: `/3-claude-md`（CLAUDE.md生成）→ その後 `/4-implement`
- 現在地確認: `/status` ／ 次へ自動で進む: `/next`
