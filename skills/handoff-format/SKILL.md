---
name: handoff-format
description: This skill should be used when an agent needs to write or send a handoff document to Cowork or another agent. Activates when creating handoff reports, progress updates, review requests, or inter-agent communication documents.
version: 0.1.0
---

# ASG handoff 作成ルール

## 送信先別フォーマット

### → 会社Cowork（設計参謀）

```markdown
# YYYY-MM-DD_【発信者ラベル】_件名

## 発信者
【自宅Code】

## 件名
（一行で）

## 内容
（本文）

## 確認事項
（Coworkに見てほしいポイント）
```

### → 秘書AI（CHANGELOG経由）

```markdown
## YYYY/MM/DD【発信者ラベル】タイトル
### 完了
- 何をしたか1行で
### 次回TODO
- 次にやるべきこと
### ブロッカー
- 坂井さんの判断/操作が必要なこと（なければ「なし」）
```

## 送信手順

```bash
# Cowork宛 handoff（Google Docs送信）
uv run python -X utf8 send_handoff_doc.py --title "<タイトル>" --body-file <mdファイルパス> --target Cowork
```

または `/send-handoff` スキルを使う。

## handoff命名（自動化）

- `send_handoff_doc.py` が自動で命名統一を行う
- 手動命名禁止（必ずスクリプト経由で送出）
- 命名形式: `YYYYMMDD_<target>宛_<sanitized_件名>`（括弧除去・スペース→アンダースコア）

## ルール

- **同日1枚統合**: 複数トピックは1枚にまとめる。分割起票しない
- **発信者ラベル必須**: 【自宅Code】【秘書AI】【365-agent】【rakuten-agent】【cybozu-agent】
- **Google Docs形式で送る**: mdファイルをそのまま送付しない（send_handoff_doc.py経由）
- **至急時**: 🔴CHANGELOG🔴マーカー + メール + handoff の3重送信
- **道B経路2 レビュー依頼の場合**: commit hash, N files changed, 確認事項を必ず含める

## 送信先パス

- → 会社Cowork: `E:/その他のパソコン/マイ ノートパソコン/Cowork運用設計/handoff/`
- ← 会社Cowork: 同パスから受信
