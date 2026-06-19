---
name: workflow-status
description: ワークフローの現在地を判定するスキル。docs/ の成果物の有無から各フェーズの完了状況を自動判定し、完了済み・次にやること・次に実行するコマンドを一覧表示する。Use to report which phases are done and what to do next (/status), or to proceed to the next phase (/next). 手動の状態ファイルを持たず、成果物から常に最新を判定する。
tools: Read, Grep, Glob, Bash
---

# ワークフロー現在地（Workflow Status）

## Overview

「今どのフェーズまで終わっていて、次に何をすべきか」を、`docs/` 配下の成果物（と CLAUDE.md）の
**実在**から自動判定する。手動の進捗ファイルを持たないので状態が古くならない。
`/status` は判定結果を表示するだけ、`/next` は最初の未完了フェーズへ誘導・起動する。

## When to Use

- 今の進捗と次の一手を知りたいとき（`/status`）
- 順番を覚えていなくても次のフェーズに進みたいとき（`/next`）

## フェーズ → 完了判定アーティファクト

存在すれば「完了」とみなす（実装/テストは成果物が一意でないため緩い判定）。

| 順 | フェーズ | コマンド | 完了の目印 |
|---|---|---|---|
| 1 | 要件定義 | `/1-requirements` | `docs/REQUIREMENTS.md` |
| 2-⓪ | 実現可能性調査 | `/2a-feasibility` | `docs/FEASIBILITY.md` |
| 2-① | 思想＋Git運用 | （`/2-design` 内） | `docs/philosophy/PLAN_PHILOSOPHY.md`・`CODING_PHILOSOPHY.md`・`TEST_PHILOSOPHY.md`・`docs/GIT_CONVENTIONS.md` |
| 2-② | 技術選定 | `/2b-tech-stack` | `docs/TECH_STACK.md` |
| 2-③ | アーキ設計 | （`/2-design` 内） | `docs/DESIGN.md` |
| 2-④ | DB設計（任意） | `/2c-db-design` | `docs/DATABASE.md`（永続データがある場合のみ必須） |
| 2-⑤ | ディレクトリ構造設計 | `/2d-directory-structure` | `docs/DIRECTORY_STRUCTURE.md` |
| 2-⑥ | タスク分解 | `/2e-tasks` | `docs/TASKS.md` |
| 3 | CLAUDE.md生成 | `/3-claude-md` | ルートの `CLAUDE.md` |
| 4 | 実装 | `/4-implement` | `docs/TASKS.md` の全タスクがマージ済み（タスク状態で判定） |
| 5 | テスト | `/5-test` | テストファイルの存在（緩い判定） |
| 6 | レビュー・監査 | `/6-review` | `docs/REVIEW.md` |
| 7 | README整備 | `/7-readme` | ルートの `README.md`（技術スタック等8要素を網羅） |

## Process

### 1. 成果物の有無を調べる

- `Glob`/`Bash(ls)` で上表のファイルの実在を確認する（`docs/` と ルート `CLAUDE.md`）。
- **実装は `docs/TASKS.md` のタスク状態で判定する**（マージ済み数 / 全タスク数）。`docs/TASKS.md` があれば
  状態列（✅マージ済み）を数え、「タスク n/m 完了・次は T0x」を示す。無ければ緩く判定し「要手動確認」と書く。
- テストは緩く判定する（例: テストディレクトリ・テストファイルの有無）。
  確実に判定できない場合は **「要手動確認」** と明示し、「完了」と誇張しない。

### 2. 現在地を組み立てる

- 各フェーズを ✅完了 / 🔄一部完了 / ⬜未着手 / －対象外（任意で不要）に分類する。
- **最初の未完了（必須）フェーズ**を「次にやること」とする。DB設計（2-④）は、要件に永続データが
  あるときのみ必須。判断できなければユーザーに確認する。

### 3a. `/status` の場合: 表示するだけ

次のフォーマットで現在地を出力する（読み取り専用。ファイルは変更しない）。

```
ワークフロー現在地
✅ STEP1 要件定義        docs/REQUIREMENTS.md
🔄 STEP2 設計
   ✅ ⓪実現可能性        docs/FEASIBILITY.md
   ✅ ①思想+Git運用      docs/philosophy/*, docs/GIT_CONVENTIONS.md
   ⬜ ②技術選定          docs/TECH_STACK.md      ← 次はここ
   ⬜ ③アーキ設計        docs/DESIGN.md
   －  ④DB設計           （永続データが必要な場合のみ）
   ⬜ ⑤ディレクトリ構造  docs/DIRECTORY_STRUCTURE.md
   ⬜ ⑥タスク分解        docs/TASKS.md
⬜ STEP3 CLAUDE.md生成   CLAUDE.md
⬜ STEP4 実装            docs/TASKS.md（例: 2/5 タスクがマージ済み・次は T03）
⬜ STEP5 テスト          （要手動確認）
⬜ STEP6 レビュー・監査   docs/REVIEW.md
⬜ STEP7 README整備      README.md

▶ 次にやること: 技術選定 →  /2b-tech-stack
   （設計を通しで進めるなら /2-design、自動で次へ進むなら /next）
```

### 3b. `/next` の場合: 次フェーズへ誘導・起動する

- 「次にやること」を1〜2行で説明し、対応するコマンド/スキルを起動する
  （例: 技術選定が次なら `technology-selection` スキル / `tech-selector` に進む）。
- 任意の DB設計が次候補のときは「永続データが必要か」を確認してから進む。
- **実装フェーズの場合**は `docs/TASKS.md` の次の未完了タスクを示し、`/4-implement`（`main` から
  そのタスクのブランチを切って実装）へ誘導する。マージ待ちのPRがあればその旨を伝える。
- 全フェーズ完了済みなら「完了。次のイテレーションへ」と案内する。

## Red Flags

- 成果物が無いのに「完了」と表示している
- 実装/テストを成果物の裏付けなく「完了」と断定している（緩い判定なら「要手動確認」と書く）
- 次のコマンドを旧名（番号なし）で案内している（正: `/2b-tech-stack` 等の番号付き）

## Verification

- [ ] 各フェーズの完了判定が実在ファイルと一致している
- [ ] 「次にやること」と「次のコマンド（番号付き）」が明示されている
- [ ] 不確実な項目を「要手動確認」と正直に表示している（`/status`）
- [ ] `/next` は次フェーズを説明してから起動している
