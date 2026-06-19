---
description: STEP2-② 技術選定（設計の単体ステップ）｜要件と思想に紐づけ採用技術を確定し、選定理由と類似技術スタックとの比較・不採用理由を docs/TECH_STACK.md にまとめる。/2-design 内でも実行される
---

> **STEP2-②（設計の単体ステップ）**　前提: `docs/REQUIREMENTS.md`・`docs/philosophy/*`（できれば`docs/FEASIBILITY.md`）　｜　通しで進めるなら `/2-design`　｜　現在地確認: `/status`

担当ペルソナは `tech-selector`（技術選定担当 / テックリード）。`Agent` ツールでこのサブエージェントに
委譲するか、その役割を引き受けて `technology-selection` スキルを起動する。

次の手順で進める:

1. `docs/REQUIREMENTS.md`・`docs/philosophy/PLAN_PHILOSOPHY.md`・`docs/FEASIBILITY.md`・既存スタックを読む
2. 選定領域（言語 / フロント・バックエンドFW / DB / 状態管理 / 認証 / インフラ / テスト・CIツール 等）を洗い出す
3. 各領域で候補（類似技術スタック）を複数挙げ、評価軸（要件適合・エコシステム成熟度・学習/習熟・性能・
   コスト/ライセンス・ロックイン）で比較する。最新状況は WebSearch/WebFetch で裏取りし出典を残す
4. 各領域で採用技術を決定し、必ず次の3点を書く:
   - 選定した技術
   - 選定理由
   - 類似技術スタックと比べてなぜそれを選定したか（不採用理由を含む）
5. スタック全体の一貫性・相性、リスク・ロックイン、採用バージョンを確認する
6. `docs/TECH_STACK.md` に保存し、人間のレビューを受ける

---
- 完了の目印: `docs/TECH_STACK.md`
- ▶ 次: `/2-design` のアーキ設計（design-and-architecture）
- 現在地確認: `/status` ／ 次へ自動で進む: `/next`
