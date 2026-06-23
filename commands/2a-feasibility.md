---
description: STEP2-⓪ 実現可能性調査（設計の単体ステップ）｜要件が技術的に実現可能かを裏取り調査しリスク・PoC要否・根拠付き判定を docs/FEASIBILITY.md にまとめる。/2-design 内でも実行される
---

> **STEP2-⓪（設計の単体ステップ）**　前提: `docs/REQUIREMENTS.md`　｜　通しで進めるなら `/2-design`　｜　現在地確認: `/status`

担当ペルソナは `feasibility-researcher`（技術調査担当）。`Agent` ツールでこのサブエージェントに
委譲するか、その役割を引き受けて `feasibility-study` スキルを起動する。

次の手順で進める:

1. `docs/REQUIREMENTS.md` と既存コード・依存・インフラを読み、要件を検証可能な問いに分解する
2. 各要件の実現可能性を調査する（必要技術の有無・成熟度、プラットフォーム/ブラウザ制約、性能・スケール限界、
   既存整合、ライセンス・コスト）。社内は Read/Grep/Glob/Bash、外部は WebSearch/WebFetch で裏取りし出典を残す
3. リスク・不確実性を洗い出し、PoC/スパイクが必要な箇所と検証方法を特定する
4. 主要な技術選定の候補・トレードオフ・推奨を比較する
5. 各要件を 実現可能 / 条件付き可能 / 困難・要再検討 で判定し、根拠（出典つき）を添える
6. `docs/FEASIBILITY.md` に保存し、人間のレビューを受ける
7. （Gitを使う場合）**1フェーズ＝1コミット**。このフェーズで作成・更新した `docs/FEASIBILITY.md` を
   まとめて1コミットする（メッセージは `docs/GIT_CONVENTIONS.md` 準拠。未確定なら `docs:` 既定。
   例 `docs: 実現可能性調査(FEASIBILITY)を追加`）

---
- 完了の目印: `docs/FEASIBILITY.md`
- ▶ 次: `/2-design`（思想定義 → 技術選定 → アーキ設計）。「困難」判定があれば `/1-requirements` に戻って見直す
- 現在地確認: `/status` ／ 次へ自動で進む: `/next`
