---
description: 設計フェーズ。要件の技術的実現可能性を調査(docs/FEASIBILITY.md)→思想をインタビューで確定(docs/philosophy/)→技術選定(docs/TECH_STACK.md)→アーキテクチャを設計(docs/DESIGN.md)する
---

設計フェーズは「実現可能性調査」→「思想定義」→「技術選定」→「アーキテクチャ設計」の4段で進める。
ステップ0は `feasibility-researcher`、ステップ2は `tech-selector`、ステップ1と3は `software-architect` が
担当する（`Agent` ツールで各サブエージェントに委譲するか、役割を引き受けて進める）。

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
2. アーキテクチャ概要・コンポーネント・データモデル・API/IF・処理フロー・トレードオフを設計する
3. 思想からの逸脱があれば理由を明記する
4. `docs/DESIGN.md` に保存し、人間のレビューを受ける

完了したら `/implement` で実装フェーズに進むよう案内する。
