---
description: STEP2-⑥ タスク分解（設計の単体ステップ）｜アーキ設計を機能単位のタスクへ分解し、実装順序・依存・ブランチ名を決めて docs/TASKS.md にまとめる。実装フェーズはこの一覧を基に 1機能=1ブランチ=1PR で進める。/2-design 内でも実行される
---

> **STEP2-⑥（設計の単体ステップ）**　前提: `docs/DESIGN.md`（必要なら `docs/DATABASE.md`・`docs/DIRECTORY_STRUCTURE.md`）　｜　通しで進めるなら `/2-design`　｜　現在地確認: `/status`

担当ペルソナは `task-planner`（タスクプランナー）。`Agent` ツールでこのサブエージェントに
委譲するか、その役割を引き受けて `task-breakdown` スキルを起動する。

次の手順で進める:

1. `docs/DESIGN.md`・`docs/REQUIREMENTS.md`（受け入れ条件）・`docs/DATABASE.md`（あれば）・
   `docs/DIRECTORY_STRUCTURE.md`（あれば／配置先の参考）・`docs/GIT_CONVENTIONS.md`（ブランチ命名規約）を読む
2. 機能を洗い出し、**1タスク=1機能=1ブランチ=1PR** の粒度（縦スライス）に分解して ID を採番する
3. 各タスクに 概要・対応する受け入れ条件・依存タスク・推奨ブランチ名（`feature/<ID>-<slug>`）・状態を付ける
4. 依存を満たす実装順に並べる（`main` を常にグリーンに保てる順）
5. `docs/TASKS.md` にまとめ、人間のレビューを受ける

このタスク一覧が実装フェーズの駆動表になる。`/4-implement` は次の未完了タスクを選び、その推奨ブランチ名で
`main` からブランチを切って実装する。

---
- 完了の目印: `docs/TASKS.md`（ID・受け入れ条件リンク・依存・ブランチ名・状態を含む）
- ▶ 次: `/3-claude-md`（CLAUDE.md生成）→ `/4-implement`（TASKS.md を駆動表に 1機能=1ブランチで実装）
- 現在地確認: `/status` ／ 次へ自動で進む: `/next`
