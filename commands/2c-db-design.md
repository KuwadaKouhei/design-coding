---
description: STEP2-④ DB設計（設計の単体ステップ・任意）｜永続データが必要な場合にDBスキーマを設計しER図(Mermaid)を含めて docs/DATABASE.md にまとめる。不要ならスキップ。/2-design 内でも実行される
---

> **STEP2-④（設計の単体ステップ・任意）**　前提: `docs/DESIGN.md`・`docs/TECH_STACK.md`　｜　通しで進めるなら `/2-design`　｜　現在地確認: `/status`

担当ペルソナは `database-designer`（DB設計担当）。`Agent` ツールでこのサブエージェントに
委譲するか、その役割を引き受けて `database-design` スキルを起動する。

次の手順で進める:

1. まず要件を見て **DB設計の要否を判断** する。永続データが不要なら理由を記録してスキップする
2. 必要な場合、`docs/REQUIREMENTS.md`・`docs/TECH_STACK.md`（採用DB）・
   `docs/philosophy/PLAN_PHILOSOPHY.md`・`docs/DESIGN.md`・既存スキーマを読む
3. エンティティ・属性・関連・カーディナリティを抽出する
4. スキーマ（テーブル/コレクション・カラム・型・NULL可否・制約・PK/FK・インデックス）を設計する。
   正規化/非正規化は思想の優先度に基づき理由を残す
5. データ関連の受け入れ条件がスキーマで満たせることを確認する
6. ER図を Mermaid（`erDiagram`）で作成し、`docs/DATABASE.md` にまとめて人間のレビューを受ける
7. （Gitを使う場合）**1フェーズ＝1コミット**。このフェーズで作成・更新した `docs/DATABASE.md`
   （DB不要ならその判断・理由の記録）をまとめて1コミットする（メッセージは `docs/GIT_CONVENTIONS.md` 準拠。
   例 `docs: DB設計(DATABASE)を追加`）

---
- 完了の目印: `docs/DATABASE.md`（DB不要なら判断と理由の記録）
- ▶ 次: `/3-claude-md`（CLAUDE.md生成）→ `/4-implement`（実装はデータ層の基準として DATABASE.md を参照）
- 現在地確認: `/status` ／ 次へ自動で進む: `/next`
