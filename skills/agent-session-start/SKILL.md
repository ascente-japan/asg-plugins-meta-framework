---
name: agent-session-start
description: This skill should be used at the very start of each agent session to process pending inter-agent messages. Activates automatically via SessionStart hook output (system reminder shows pending messages). Required for rakuten-agent / kuchikomi365-agent / cybozu-agent / peakmanager-agent.
version: 0.1.0
---

# agent セッション開始ルーティン

各agent（rakuten / 365 / cybozu / peakmanager）がセッション開始時に未読 inter-agent message を処理するための行動規範。

## 自動配達の仕組み

各agentの `.claude/settings.json` に **SessionStart hook** が設定されている。
hook が `check_pending_messages.py <agent_id>` を実行し、pending メッセージを system reminder として Claude に渡す。

→ **agentは「気づく」必要なし。届く**。

## 受信時の行動

### 1. 内容把握
hook出力に `=== inter-agent message: N件 pending ===` が含まれていたら、それぞれの本文を読む。

### 2. 処理
- 指示メッセージ → 該当タスクを通常通り遂行
- 通知メッセージ → 認識のみで完了
- 緊急メッセージ → 即時着手 or 親（home-code）に逆送信で確認

### 3. 既読化（**処理完了後に明示呼出**）
処理が完了したら、mark_read を呼ぶ:

```
mcp__asg-internal__mark_read([id1, id2, ...])
```

**自動既読化はしない**。理由: 処理失敗時の再読み込みを確保するため（silent failure による未読消失を防ぐ）。

### 4. 必要なら返信
親や他agentへ返信が必要なら send_message:

```
mcp__asg-internal__send_message(
    target="home-code",  # or 別agent / 'all'
    content="...",
    source="<自分のagent_id>"
)
```

## 遵守事項

- **未読をスキップしない**: hook で見えたものは必ず処理する
- **mark_read は処理完了後**: スキップ・失敗時は既読化しない
- **agent_id を間違えない**: 自分のidで read／自分以外の id でsend する
- **broadcast (target='all') も自分宛てに含まれる**: 受信時は他agentと共有された通知

## トラブル時

- hook出力に WARN（DB読み失敗等）が出た場合: asg-internal MCP の状態確認、必要なら親（home-code）へエスカレート
- mark_read で対象IDが見つからない場合: 既に他の何かで既読化された可能性、無視してOK

## 関連

- 通信路: asg-internal-mcp-server （`send_message` / `read_messages` / `mark_read`）
- ストレージ: SQLite (`asg-internal-mcp-server/data/messages.db`)
- 設計文書: 2026-04-25 案X handoff（本機能の起源）
