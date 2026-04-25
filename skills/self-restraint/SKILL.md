---
name: self-restraint
description: This skill should be used when an agent is about to call a skill, MCP tool, or capability that may be outside its own responsibility domain. Activates when there is ambiguity about whether a tool or action falls within the agent's defined scope, or when another agent's boundary might be crossed.
version: 0.1.0
---

# 責務外スキル・MCP 自己抑制ルール（全agent共通）

出典: メタ設計層 Phase 2（2026-04-24、会社Cowork統合回答）

## 原則

各エージェントは自身の責務領域外のスキル・MCPを自発的に呼び出さない。
責務境界は `system-role.md` / `boundary.md` で定義される担当システムに準拠する。

## 呼び出し前判断

1. このスキル/MCPは自身の責務領域か？
2. 責務外の場合、**呼び出しを差し戻す**。親（自宅Code）または坂井さん経由で再確認する
3. 汎用ツール（Read/Write/Edit/Grep/Glob/Bash）および責務領域内MCPは常時利用可

## 例外

- セッション起動時の自己観測・設定確認
- 明示的に坂井さん/親から依頼された一時的な責務外操作

## 背景

Claude Codeの仕様上、親セッションで有効なプラグイン・スキルはサブエージェントに自動継承される（サブエージェント単位の遮断機構は現時点で未実装）。
公式機能で止められない部分は本ルールの自制で補う。
