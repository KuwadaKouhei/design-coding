---
description: STEP6 レビュー・監査｜テスト後にコード/セキュリティ/性能/思想準拠を専門ペルソナで点検し、重大度別の指摘を docs/REVIEW.md にまとめる。テスト(STEP5)の後に実行
---

> **STEP6 / 全7フェーズ**　前提: 実装・テスト（STEP4・5）　｜　現在地確認: `/status`

`review-and-audit` スキルを起動する。レビュー対象は**当該機能ブランチの差分**（`main...feature/<ID>`）。
これは PR をマージする前の最終ゲートで、マージ可になったら AI が PR を提出し、**マージは人間が行う**。

次の手順で進める:

1. レビュー対象の差分（`git diff main...feature/<ID>`）と、`docs/REQUIREMENTS.md` / `docs/DESIGN.md` /
   `docs/TASKS.md`（当該タスク）/ `docs/philosophy/*.md` を確認する
2. `Agent` ツールで次の専門ペルソナ（サブエージェント）を **並行起動** し、各観点を点検させる:
   - `code-reviewer`（正しさ・可読性・設計・保守性）
   - `security-auditor`（セキュリティ監査）
   - `web-performance-auditor`（性能監査）
   - `philosophy-compliance-reviewer`（思想準拠 = 本ワークフロー固有の観点）
3. 指摘を統合し、Blocker / Should / Consider の重大度で分類する
4. 受け入れ条件の充足状況を確認する
5. `docs/REVIEW.md` にまとめ、判定（マージ可 / 要修正）を明記する
6. Blocker が残る場合は該当フェーズ（`/4-implement`・`/5-test`）へ戻して是正し、再レビューする
7. **マージ可なら PR を提出する**: ブランチを push → `gh pr create`（PR本文 = タスク概要・対応受け入れ条件・
   レビュー結果サマリ・テスト結果）。**マージは AI が行わず人間に委ねる**（AIは `main` にマージしない）。
   GitHubリモート/gh CLI が無い場合は、ブランチ名・PRタイトル・本文・実行コマンドを提示して人間に委ねる
8. 人間がPRを承認・マージしたら `main` を最新化し、`docs/TASKS.md` の状態を「マージ済み」に更新して
   次タスク（`/4-implement`）へ戻る。残タスクが無くなったら `/7-readme` へ進む

---
- 完了の目印: `docs/REVIEW.md`（判定: マージ可 / 要修正）＋ マージ可なら提出済みPR
- ▶ 次: 人間がマージ → 次タスクは `/4-implement`。全タスク完了なら `/7-readme`。差し戻しは `/4-implement`・`/5-test`
- 現在地確認: `/status` ／ 次へ自動で進む: `/next`
