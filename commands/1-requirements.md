---
description: STEP1 要件定義｜機能/非機能要件・ユーザーストーリー・受け入れ条件・スコープを明確化し docs/REQUIREMENTS.md に保存する。最初に実行
---

> **STEP1 / 全6フェーズ**　前提: なし（ここから開始）　｜　現在地確認: `/status`

担当ペルソナは `requirements-analyst`（要件アナリスト）。`Agent` ツールでこのサブエージェントに
委譲するか、その役割を引き受けたうえで `requirements-definition` スキルを起動する。

次の手順で進める:

1. 既存の `docs/REQUIREMENTS.md` があれば読み、なければ新規作成する
2. 背景・目的・対象ユーザーをヒアリングする
3. 機能要件・非機能要件を洗い出す（曖昧な要望はテスト可能な受け入れ条件へ変換する）
4. スコープ（やること / やらないこと）と前提・制約・未決事項を明確化する
5. `docs/REQUIREMENTS.md` に保存し、人間のレビューを受ける
6. （Gitを使う場合）**1フェーズ＝1コミット**。このフェーズで作成・更新した `docs/REQUIREMENTS.md` を
   まとめて1コミットする。この時点では `docs/GIT_CONVENTIONS.md` が未作成のため、メッセージは
   Conventional Commits の `docs:` を既定とする（例 `docs: 要件定義(REQUIREMENTS)を追加`）

---
- 完了の目印: `docs/REQUIREMENTS.md`
- ▶ 次: `/2-design`（設計フェーズ）
- 現在地確認: `/status` ／ 次へ自動で進む: `/next`
