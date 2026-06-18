---
description: レビュー・監査フェーズ。テスト後にコード/セキュリティ/性能/思想準拠を専門ペルソナで点検し、重大度別の指摘を docs/REVIEW.md にまとめる
---

`review-and-audit` スキルを起動する。

次の手順で進める:

1. レビュー対象の差分と、`docs/REQUIREMENTS.md` / `docs/DESIGN.md` / `docs/philosophy/*.md` を確認する
2. `Agent` ツールで次の専門ペルソナ（サブエージェント）を **並行起動** し、各観点を点検させる:
   - `code-reviewer`（正しさ・可読性・設計・保守性）
   - `security-auditor`（セキュリティ監査）
   - `web-performance-auditor`（性能監査）
   - `philosophy-compliance-reviewer`（思想準拠 = 本ワークフロー固有の観点）
3. 指摘を統合し、Blocker / Should / Consider の重大度で分類する
4. 受け入れ条件の充足状況を確認する
5. `docs/REVIEW.md` にまとめ、判定（マージ可 / 要修正）を明記する
6. Blocker が残る場合は該当フェーズ（`/implement`・`/test`）へ戻して是正し、再レビューする

これで「要件定義 → 設計 → 実装 → テスト → レビュー・監査」の一巡が完了する。
