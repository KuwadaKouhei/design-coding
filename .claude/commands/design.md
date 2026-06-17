---
description: 設計フェーズ。まず設計/実装/テスト思想をインタビューで確定し docs/philosophy/ に保存、その後アーキテクチャを設計し docs/DESIGN.md に保存する
---

担当ペルソナは `software-architect`（ソフトウェアアーキテクト）。`Agent` ツールでこのサブエージェントに
委譲するか、その役割を引き受けて進める。設計フェーズは「思想定義」→「アーキテクチャ設計」の2段で進める。

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

1. `docs/REQUIREMENTS.md` と `docs/philosophy/PLAN_PHILOSOPHY.md` を読む
2. アーキテクチャ概要・コンポーネント・データモデル・API/IF・処理フロー・トレードオフを設計する
3. 思想からの逸脱があれば理由を明記する
4. `docs/DESIGN.md` に保存し、人間のレビューを受ける

完了したら `/implement` で実装フェーズに進むよう案内する。
