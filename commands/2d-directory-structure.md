---
description: STEP2-⑤ ディレクトリ構造設計（設計の単体ステップ）｜アーキ設計・技術選定・DB設計を入力に、物理的なディレクトリ構造と階層構造の思想（可読性・拡張性を最優先）を確定し docs/DIRECTORY_STRUCTURE.md にまとめる。/2-design 内でも実行される
---

> **STEP2-⑤（設計の単体ステップ）**　前提: `docs/DESIGN.md`（必要なら `docs/DATABASE.md`・`docs/TECH_STACK.md`）　｜　通しで進めるなら `/2-design`　｜　現在地確認: `/status`

担当ペルソナは `structure-designer`（ディレクトリ構造設計担当）。`Agent` ツールでこのサブエージェントに
委譲するか、その役割を引き受けて `directory-structure` スキルを起動する。

ディレクトリ構造は**常に必ず可読性と拡張性を最優先**で設計する。次の手順で進める:

1. `docs/DESIGN.md`（論理構成）・`docs/TECH_STACK.md`（FW規約）・`docs/DATABASE.md`（あれば）・
   `docs/philosophy/PLAN_PHILOSOPHY.md`（境界・依存方向）・既存コードの実構造を読む
2. 構造の基調（レイヤー型 / 機能型 / ドメイン型）を決め、採用FWの標準レイアウトと整合させる
3. **可読性のルール**を定義する（命名規約・1ディレクトリ1責務・階層の深さ・テスト/型の同居or分離）
4. **拡張性のルール**を定義する（新要素の追加先が一意に決まる配置・依存方向・`shared`/`common` の肥大化抑制）
5. ディレクトリツリーと各ディレクトリの責務表を書き、「新しい〇〇を足す」例で配置に迷いが出ないか自己検証する
6. 思想・FW規約からの逸脱があれば理由を明記し、`docs/DIRECTORY_STRUCTURE.md` にまとめて人間のレビューを受ける

このドキュメントが実装フェーズの配置規約になる。`/4-implement` は本ドキュメントの命名・依存方向・配置ルールに
準拠してコードを置く。

---
- 完了の目印: `docs/DIRECTORY_STRUCTURE.md`（構造方針・ツリー・責務表・命名規約・依存/配置ルールを含む）
- ▶ 次: `/2e-tasks`（タスク分解）→ `/3-claude-md`（CLAUDE.md生成）→ `/4-implement`
- 現在地確認: `/status` ／ 次へ自動で進む: `/next`
