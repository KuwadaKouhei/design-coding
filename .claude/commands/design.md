---
description: 設計フェーズ。要件の技術的実現可能性を調査(docs/FEASIBILITY.md)→思想とGit運用方針をインタビューで確定(docs/philosophy/・docs/GIT_CONVENTIONS.md)→技術選定(docs/TECH_STACK.md)→アーキテクチャを設計(docs/DESIGN.md)→必要ならDB設計(docs/DATABASE.md)する
---

設計フェーズは「実現可能性調査」→「思想定義」→「技術選定」→「アーキテクチャ設計」→「DB設計（必要な場合）」
の5段で進める。ステップ0は `feasibility-researcher`、ステップ2は `tech-selector`、ステップ4は
`database-designer`、ステップ1と3は `software-architect` が担当する（`Agent` ツールで各サブエージェントに
委譲するか、役割を引き受けて進める）。

## ステップ0: 実現可能性調査（feasibility-study スキル / feasibility-researcher）

`docs/FEASIBILITY.md` があるか確認する。なければ `feasibility-study` スキルを起動して作成する。

1. `docs/REQUIREMENTS.md` と既存コード・依存を読み、要件を検証可能な問いに分解する
2. 各要件の実現可能性を社内コードと外部情報（WebSearch/WebFetch）で裏取り調査する
3. リスク・不確実性・PoC要否を洗い出し、技術選定の比較・推奨を整理する
4. 各要件を 実現可能 / 条件付き可能 / 困難・要再検討 で判定し、根拠（出典つき）を残す
5. `docs/FEASIBILITY.md` に保存する。「困難」判定があれば `/requirements` への差し戻しを促し、
   実現性が確認できてからステップ1へ進む

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

## ステップ2: 技術選定（technology-selection スキル / tech-selector）

`docs/TECH_STACK.md` があるか確認する。なければ `technology-selection` スキルを起動して作成する。

1. `docs/REQUIREMENTS.md`・`docs/philosophy/PLAN_PHILOSOPHY.md`・`docs/FEASIBILITY.md`・既存スタックを読む
2. 選定領域（言語 / FW / DB / 状態管理 / 認証 / インフラ / テスト・CIツール 等）を洗い出す
3. 各領域で候補（類似技術スタック）を評価軸で比較する。最新状況は WebSearch/WebFetch で裏取りする
4. 採用技術を決定し、「選定技術」「選定理由」「類似技術と比べてなぜ選んだか（不採用理由含む）」を残す
5. `docs/TECH_STACK.md` に保存し、人間のレビューを受ける

## ステップ3: アーキテクチャ設計（design-and-architecture スキル）

`design-and-architecture` スキルを起動する。

1. `docs/REQUIREMENTS.md`・`docs/philosophy/PLAN_PHILOSOPHY.md`・`docs/FEASIBILITY.md`（制約・リスク・
   推奨技術）・`docs/TECH_STACK.md`（採用技術）を読む
2. アーキテクチャ概要・コンポーネント・データモデル（概念）・API/IF・処理フロー・トレードオフを設計する
3. 思想からの逸脱があれば理由を明記する
4. `docs/DESIGN.md` に保存し、人間のレビューを受ける

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

## 設計完了後（実装に入る前）: CLAUDE.md 生成

設計フェーズが完了したら、**実装に入る前に** `/claude-md`（`claude-md-generation` スキル /
`claude-md-author`）を実行し、プロジェクトルートに `CLAUDE.md` を生成する。CLAUDE.md には各設計
ドキュメントの説明と、「実装はドキュメントに沿う」「修正時はコードとドキュメントを一緒に更新する」運用ルールを
記述する。これにより実装フェーズが常にドキュメント駆動になる。

その後 `/implement` で実装フェーズに進むよう案内する。
