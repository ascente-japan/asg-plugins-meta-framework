# ASG Templates

ASG repo 全体で配布する標準ファイル。`.gitignore` 等の標準セット。

## 配布対象

- ASG agent repo（rakuten-agent / kuchikomi365-agent / cybozu-agent / peakmanager-agent 等）
- ASG plugin repo（asg-plugins-meta-framework 等）
- ASG infrastructure repo（asg-marketplace / asg-business-connectors / asg-agent-ops / asg-internal-mcp-server）
- scripts（自宅Code 管制塔）

## ファイル

### `gitignore.standard`

標準 `.gitignore`。配布先 repo にコピーして `.gitignore` にrenameまたは既存に追記マージ。

主要セクション:
- 認証情報（必須・絶対push禁止）
- Python / uv / venv
- OS / IDE
- Claude Code ローカル設定
- Google Drive リンクファイル
- Handoff
- ログ / Temp / Debug
- Playwright artifacts

## 改訂履歴

- v1 (2026-04-27): scripts/.gitignore ベースで初版策定（M2 §2-13 配布前セキュリティチェック層 Step 2 関連）
