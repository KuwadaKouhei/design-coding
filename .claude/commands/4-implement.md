---
description: STEP4 実装｜CLAUDE.md を入口に DESIGN/CODING_PHILOSOPHY 等に準拠し、垂直スライス単位で小さくコミットしながら実装する。CLAUDE.md生成(STEP3)の後に実行
---

> **STEP4 / 全6フェーズ**　前提: ルートの `CLAUDE.md`（STEP3完了）＋設計成果物　｜　現在地確認: `/status`

担当ペルソナは `implementer`（ソフトウェアエンジニア）。`Agent` ツールでこのサブエージェントに
委譲するか、その役割を引き受けて `implementation` スキルを起動する。

実装は `docs/TASKS.md` を駆動表に、**1タスク=1機能=1ブランチ=1PR** で進める。`main` には直接
コミットせず、常にグリーン（動く状態）を保つ。1回の `/4-implement` では**1タスク**を実装する。

次の手順で進める:

1. 着手前に **プロジェクトルートの `CLAUDE.md`** を確認する（無ければ `/3-claude-md` で生成してから進む）。
   CLAUDE.md を入口に `docs/TASKS.md`（駆動表）・`docs/GIT_CONVENTIONS.md`（ブランチ/コミット規約）・
   `docs/philosophy/CODING_PHILOSOPHY.md`・`docs/DESIGN.md`・`docs/DIRECTORY_STRUCTURE.md`（あれば／配置規約）・
   `docs/DATABASE.md`（あれば）・`docs/REQUIREMENTS.md` の受け入れ条件を読む
2. `docs/TASKS.md` から**次の未完了タスクを1つ**選ぶ（依存が満たされているもの）
3. `main` を最新化してブランチを切る: `git switch main && git pull` → `git switch -c feature/<ID>-<slug>`
   （タスクの推奨ブランチ名。Git未使用なら `GIT_CONVENTIONS.md` に従いスキップ）
4. 既存コードから再利用できる関数・ユーティリティ・パターンを探す（新規作成より再利用を優先）
5. そのタスクを垂直スライス（1つの完結した機能パス）に分割する
6. スライスごとに: 実装 → 動作確認 → 思想準拠の自己チェック → `GIT_CONVENTIONS.md` に沿って
   **そのブランチ上で**小さくコミット（`main` 直コミット禁止）
7. 思想・設計どおりに進められない場合は黙って変えず、停止してユーザーに相談する
8. **コードを変更したら、影響する設計ドキュメント（DESIGN/DATABASE/TECH_STACK/TASKS の状態列 等）も
   一緒に更新し整合を保つ**
9. 最後にどこを・なぜ変更したかを日本語で説明する

> このタスクのテスト（`/5-test`）・レビュー（`/6-review`）まで終えると、`/6-review` の最後で
> PR提出（`gh pr create`）→ **人間がマージ** → 次タスクへ、というサイクルになる。

---
- 完了の目印: 当該タスクの実装＋ブランチ上のコミット（`docs/TASKS.md` の状態を更新）
- ▶ 次: `/5-test`（このタスクのテスト）→ `/6-review`（レビュー＆PR提出）
- 現在地確認: `/status` ／ 次へ自動で進む: `/next`
