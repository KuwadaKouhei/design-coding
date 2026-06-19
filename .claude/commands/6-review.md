---
description: STEP6 レビュー・監査｜テスト後にコード/セキュリティ/性能/思想準拠を専門ペルソナで点検し、重大度別の指摘を docs/REVIEW.md にまとめる。テスト(STEP5)の後に実行
---

> **STEP6 / 全7フェーズ**　前提: 実装・テスト（STEP4・5）　｜　現在地確認: `/status`

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
6. Blocker が残る場合は該当フェーズ（`/4-implement`・`/5-test`）へ戻して是正し、再レビューする

---
- 完了の目印: `docs/REVIEW.md`（判定: マージ可 / 要修正）
- ▶ 次: `/7-readme`（README整備・最終）。差し戻しがあれば `/4-implement`・`/5-test` へ
- 現在地確認: `/status` ／ 次へ自動で進む: `/next`
