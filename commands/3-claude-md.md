---
description: STEP3 CLAUDE.md生成｜設計完了後・実装着手前にプロジェクトルートの CLAUDE.md を生成。各設計ドキュメントの説明と「実装はドキュメントに沿う」「修正時はドキュメントも一緒に更新する」ルールを記述する。設計(STEP2)の後に実行
---

> **STEP3 / 全6フェーズ**　前提: 設計成果物（`docs/DESIGN.md` ほか／STEP2完了）　｜　現在地確認: `/status`

担当ペルソナは `claude-md-author`（CLAUDE.md 作成担当）。`Agent` ツールでこのサブエージェントに
委譲するか、その役割を引き受けて `claude-md-generation` スキルを起動する。

次の手順で進める:

1. `docs/` 配下の設計成果物（REQUIREMENTS / FEASIBILITY / philosophy/* / GIT_CONVENTIONS / TECH_STACK /
   DESIGN / DATABASE / DIRECTORY_STRUCTURE / TASKS）の有無を確認する
2. 各ドキュメント冒頭を読み、役割と参照タイミングを1行で要約する
3. プロジェクトルートに `CLAUDE.md` を生成する（既存があれば上書きせず索引・ルールの節を更新し手書き記述は保持）:
   - **ドキュメント体系の表**（実在するドキュメントの説明・参照タイミング）
   - **実装の原則**: 実装はドキュメントに沿って行う／食い違いは確認する
   - **変更時のルール**: コードを変更したら関連ドキュメントも一緒に更新し整合を保つ／仕様変更は先にドキュメントを直す
   - Git運用（GIT_CONVENTIONS）・テスト（TEST_PHILOSOPHY）・言語などの基本ルールへの参照
4. 人間のレビューを受ける

---
- 完了の目印: ルートの `CLAUDE.md`
- ▶ 次: `/4-implement`（実装フェーズ）
- 現在地確認: `/status` ／ 次へ自動で進む: `/next`
