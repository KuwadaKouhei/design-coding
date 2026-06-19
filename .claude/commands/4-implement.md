---
description: STEP4 実装｜CLAUDE.md を入口に DESIGN/CODING_PHILOSOPHY 等に準拠し、垂直スライス単位で小さくコミットしながら実装する。CLAUDE.md生成(STEP3)の後に実行
---

> **STEP4 / 全6フェーズ**　前提: ルートの `CLAUDE.md`（STEP3完了）＋設計成果物　｜　現在地確認: `/status`

担当ペルソナは `implementer`（ソフトウェアエンジニア）。`Agent` ツールでこのサブエージェントに
委譲するか、その役割を引き受けて `implementation` スキルを起動する。

次の手順で進める:

1. 着手前に **プロジェクトルートの `CLAUDE.md`** を確認する（無ければ `/3-claude-md` で生成してから進む）。
   CLAUDE.md を入口に `docs/philosophy/CODING_PHILOSOPHY.md`・`docs/DESIGN.md`・`docs/DATABASE.md`（あれば）・
   `docs/REQUIREMENTS.md` の受け入れ条件を読む
2. 既存コードから再利用できる関数・ユーティリティ・パターンを探す（新規作成より再利用を優先）
3. 作業を垂直スライス（1つの完結した機能パス）に分割する
4. スライスごとに: 実装 → 動作確認 → 思想準拠の自己チェック → `GIT_CONVENTIONS.md` に沿って小さくコミット
5. 思想・設計どおりに進められない場合は黙って変えず、停止してユーザーに相談する
6. **コードを変更したら、影響する設計ドキュメント（DESIGN/DATABASE/TECH_STACK 等）も一緒に更新し整合を保つ**
7. 最後にどこを・なぜ変更したかを日本語で説明する

---
- 完了の目印: 受け入れ条件を満たす実装＋コミット（緩い判定）
- ▶ 次: `/5-test`（テストフェーズ）
- 現在地確認: `/status` ／ 次へ自動で進む: `/next`
