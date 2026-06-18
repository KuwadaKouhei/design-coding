---
description: 設計フェーズ。まず要件の技術的実現可能性を調査(docs/FEASIBILITY.md)、次に設計/実装/テスト思想をインタビューで確定(docs/philosophy/)、最後にアーキテクチャを設計(docs/DESIGN.md)する
---

設計フェーズは「実現可能性調査」→「思想定義」→「アーキテクチャ設計」の3段で進める。
ステップ0は `feasibility-researcher`、ステップ1〜2は `software-architect` が担当する（`Agent` ツールで
各サブエージェントに委譲するか、役割を引き受けて進める）。

## ステップ0: 実現可能性調査（feasibility-study スキル / feasibility-researcher）

`docs/FEASIBILITY.md` があるか確認する。なければ `feasibility-study` スキルを起動して作成する。

1. `docs/REQUIREMENTS.md` と既存コード・依存を読み、要件を検証可能な問いに分解する
2. 各要件の実現可能性を社内コードと外部情報（WebSearch/WebFetch）で裏取り調査する
3. リスク・不確実性・PoC要否を洗い出し、技術選定の比較・推奨を整理する
4. 各要件を 実現可能 / 条件付き可能 / 困難・要再検討 で判定し、根拠（出典つき）を残す
5. `docs/FEASIBILITY.md` に保存する。「困難」判定があれば `/requirements` への差し戻しを促し、
   実現性が確認できてからステップ1へ進む

## ステップ1: 思想の確定（philosophy-definition スキル）

`philosophy-definition` スキルを起動する。

1. `docs/philosophy/` に3思想ファイルがあるか確認する。あれば更新の要否をユーザーに確認する
2. なければ **インタビュー形式で1問ずつ** 設計思想・実装思想・テスト思想を引き出す
   （`question-bank.md` の質問を使い、各問いに推奨デフォルトを添える）
3. 回答を要約してユーザーの承認を得る
4. `templates.md` に沿って以下を書き出す:
   - `docs/philosophy/PLAN_PHILOSOPHY.md`（設計思想）
   - `docs/philosophy/CODING_PHILOSOPHY.md`（実装思想）
   - `docs/philosophy/TEST_PHILOSOPHY.md`（テスト思想）

## ステップ2: アーキテクチャ設計（design-and-architecture スキル）

`design-and-architecture` スキルを起動する。

1. `docs/REQUIREMENTS.md`・`docs/philosophy/PLAN_PHILOSOPHY.md`・`docs/FEASIBILITY.md`（調査で判明した
   制約・リスク・推奨技術）を読む
2. アーキテクチャ概要・コンポーネント・データモデル・API/IF・処理フロー・トレードオフを設計する
3. 思想からの逸脱があれば理由を明記する
4. `docs/DESIGN.md` に保存し、人間のレビューを受ける

完了したら `/implement` で実装フェーズに進むよう案内する。
