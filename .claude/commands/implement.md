---
description: 実装フェーズ。DESIGN.md と CODING_PHILOSOPHY.md に準拠し、垂直スライス単位で小さくコミットしながらコードを実装する
---

担当ペルソナは `implementer`（ソフトウェアエンジニア）。`Agent` ツールでこのサブエージェントに
委譲するか、その役割を引き受けて `implementation` スキルを起動する。

次の手順で進める:

1. 着手前に **プロジェクトルートの `CLAUDE.md`** を確認する（無ければ `/claude-md` で生成してから進む）。
   CLAUDE.md を入口に `docs/philosophy/CODING_PHILOSOPHY.md`・`docs/DESIGN.md`・`docs/DATABASE.md`（あれば）・
   `docs/REQUIREMENTS.md` の受け入れ条件を読む
2. 既存コードから再利用できる関数・ユーティリティ・パターンを探す（新規作成より再利用を優先）
3. 作業を垂直スライス（1つの完結した機能パス）に分割する
4. スライスごとに: 実装 → 動作確認 → 思想準拠の自己チェック → `GIT_CONVENTIONS.md` に沿って小さくコミット
5. 思想・設計どおりに進められない場合は黙って変えず、停止してユーザーに相談する
6. **コードを変更したら、影響する設計ドキュメント（DESIGN/DATABASE/TECH_STACK 等）も一緒に更新し整合を保つ**
7. 最後にどこを・なぜ変更したかを日本語で説明する

完了したら `/test` でテストフェーズに進むよう案内する。
