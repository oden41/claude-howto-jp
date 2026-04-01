<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../resources/logos/claude-howto-logo.svg">
</picture>

# Hooks

Hooksは、Claude Codeセッション中に特定のイベントが発生した際に実行される自動化スクリプトです。自動化、バリデーション、パーミッション管理、カスタムワークフローを実現します。

## 概要

Hooksは、Claude Codeで特定のイベントが発生した際に自動的に実行される自動アクション（シェルコマンド、HTTPウェブフック、LLMプロンプト、またはサブエージェント評価）です。JSON入力を受け取り、終了コードとJSON出力を通じて結果を通知します。

**主な特徴:**
- イベント駆動の自動化
- JSONベースの入出力
- command、prompt、HTTP、agentの4つのhookタイプをサポート
- ツール固有のhookのためのパターンマッチング

## 設定

Hooksは設定ファイルで特定の構造で設定されます：

- `~/.claude/settings.json` - ユーザー設定（すべてのプロジェクト）
- `.claude/settings.json` - プロジェクト設定（共有可能、コミット対象）
- `.claude/settings.local.json` - ローカルプロジェクト設定（コミット対象外）
- マネージドポリシー - 組織全体の設定
- プラグイン `hooks/hooks.json` - プラグインスコープのhooks
- スキル/エージェントフロントマター - コンポーネントライフタイムのhooks

### 基本設定構造

```json
{
  "hooks": {
    "EventName": [
      {
        "matcher": "ToolPattern",
        "hooks": [
          {
            "type": "command",
            "command": "your-command-here",
            "timeout": 60
          }
        ]
      }
    ]
  }
}
```

**主要フィールド:**

| フィールド | 説明 | 例 |
|-----------|------|-----|
| `matcher` | ツール名にマッチするパターン（大文字小文字区別） | `"Write"`、`"Edit\|Write"`、`"*"` |
| `hooks` | hookの定義の配列 | `[{ "type": "command", ... }]` |
| `type` | hookの種類: `"command"`（bash）、`"prompt"`（LLM）、`"http"`（ウェブフック）、`"agent"`（サブエージェント） | `"command"` |
| `command` | 実行するシェルコマンド | `"$CLAUDE_PROJECT_DIR/.claude/hooks/format.sh"` |
| `timeout` | 省略可のタイムアウト（秒、デフォルト60） | `30` |
| `once` | `true` の場合、セッションにつき1回だけhookを実行 | `true` |

### マッチャーパターン

| パターン | 説明 | 例 |
|---------|------|-----|
| 完全一致文字列 | 特定のツールにマッチ | `"Write"` |
| 正規表現パターン | 複数のツールにマッチ | `"Edit\|Write"` |
| ワイルドカード | すべてのツールにマッチ | `"*"` または `""` |
| MCPツール | サーバーとツールのパターン | `"mcp__memory__.*"` |

## Hookの種類

Claude Codeは4つのhookの種類をサポートしています：

### Commandフック

デフォルトのhookの種類。シェルコマンドを実行し、JSON stdin/stdoutと終了コードを通じて通信します。

```json
{
  "type": "command",
  "command": "python3 \"$CLAUDE_PROJECT_DIR/.claude/hooks/validate.py\"",
  "timeout": 60
}
```

### HTTPフック

> v2.1.63で追加。

commandフックと同じJSON入力を受け取るリモートウェブフックエンドポイント。HTTPフックはURLにJSONをPOSTしてJSONレスポンスを受け取ります。HTTPフックはサンドボックスが有効な場合にサンドボックスを通じてルーティングされます。URLでの環境変数インターポレーションにはセキュリティのために明示的な `allowedEnvVars` リストが必要です。

```json
{
  "hooks": {
    "PostToolUse": [{
      "type": "http",
      "url": "https://my-webhook.example.com/hook",
      "matcher": "Write"
    }]
  }
}
```

**主要プロパティ:**
- `"type": "http"` -- HTTPフックとして識別
- `"url"` -- ウェブフックエンドポイントのURL
- サンドボックスが有効な場合にサンドボックスを通じてルーティング
- URLでの環境変数インターポレーションには明示的な `allowedEnvVars` リストが必要

### Promptフック

hookコンテンツがClaudeが評価するプロンプトであるLLM評価プロンプト。主に `Stop` と `SubagentStop` イベントでインテリジェントなタスク完了チェックに使用されます。

```json
{
  "type": "prompt",
  "prompt": "Claudeがリクエストされたすべてのタスクを完了したか評価してください。",
  "timeout": 30
}
```

LLMはプロンプトを評価し、構造化された決定を返します（詳細は[プロンプトベースのHooks](#プロンプトベースのhooks)を参照）。

### Agentフック

条件を評価または複雑なチェックを実行するために専用エージェントを生成するサブエージェントベースの検証フック。promptフック（単一ターンのLLM評価）とは異なり、agentフックはツールを使用してマルチステップの推論を実行できます。

```json
{
  "type": "agent",
  "prompt": "コードの変更がアーキテクチャガイドラインに従っているか確認してください。関連する設計ドキュメントを確認して比較してください。",
  "timeout": 120
}
```

**主要プロパティ:**
- `"type": "agent"` -- agentフックとして識別
- `"prompt"` -- サブエージェントのタスク説明
- エージェントはツール（Read、Grep、Bashなど）を使用して評価を実行できる
- promptフックと同様の構造化された決定を返す

## Hookイベント

Claude Codeは**25のhookイベント**をサポートしています：

| イベント | トリガーのタイミング | マッチャー入力 | ブロック可能 | 主な用途 |
|--------|----------------|-------------|-----------|---------|
| **SessionStart** | セッション開始/再開/クリア/コンパクト | startup/resume/clear/compact | いいえ | 環境のセットアップ |
| **InstructionsLoaded** | CLAUDE.mdまたはルールファイルが読み込まれた後 | （なし） | いいえ | 指示の修正/フィルタリング |
| **UserPromptSubmit** | ユーザーがプロンプトを送信した時 | （なし） | はい | プロンプトのバリデーション |
| **PreToolUse** | ツール実行の前 | ツール名 | はい（allow/deny/ask） | 入力のバリデーション・修正 |
| **PermissionRequest** | パーミッションダイアログが表示された時 | ツール名 | はい | 自動承認/拒否 |
| **PostToolUse** | ツールが成功した後 | ツール名 | いいえ | コンテキスト追加、フィードバック |
| **PostToolUseFailure** | ツール実行が失敗した時 | ツール名 | いいえ | エラーハンドリング、ログ |
| **Notification** | 通知が送信された時 | 通知の種類 | いいえ | カスタム通知 |
| **SubagentStart** | サブエージェントが生成された時 | エージェントタイプ名 | いいえ | サブエージェントのセットアップ |
| **SubagentStop** | サブエージェントが完了した時 | エージェントタイプ名 | はい | サブエージェントのバリデーション |
| **Stop** | Claudeが応答を完了した時 | （なし） | はい | タスク完了チェック |
| **StopFailure** | APIエラーでターンが終了した時 | （なし） | いいえ | エラー回復、ログ |
| **TeammateIdle** | エージェントチームのチームメイトがアイドル | （なし） | はい | チームメイトの調整 |
| **TaskCompleted** | タスクが完了とマークされた時 | （なし） | はい | タスク後のアクション |
| **TaskCreated** | TaskCreateでタスクが作成された時 | （なし） | いいえ | タスク追跡、ログ |
| **ConfigChange** | 設定ファイルが変更された時 | （なし） | はい（ポリシー除く） | 設定更新への対応 |
| **CwdChanged** | 作業ディレクトリが変更された時 | （なし） | いいえ | ディレクトリ固有のセットアップ |
| **FileChanged** | 監視ファイルが変更された時 | （なし） | いいえ | ファイル監視、リビルド |
| **PreCompact** | コンテキストコンパクション前 | manual/auto | いいえ | コンパクション前のアクション |
| **PostCompact** | コンパクション完了後 | （なし） | いいえ | コンパクション後のアクション |
| **WorktreeCreate** | Worktreeが作成される時 | （なし） | はい（パス返却） | Worktreeの初期化 |
| **WorktreeRemove** | Worktreeが削除される時 | （なし） | いいえ | Worktreeのクリーンアップ |
| **Elicitation** | MCPサーバーがユーザー入力を要求した時 | （なし） | はい | 入力バリデーション |
| **ElicitationResult** | ユーザーがelicitationに応答した時 | （なし） | はい | レスポンス処理 |
| **SessionEnd** | セッションが終了する時 | （なし） | いいえ | クリーンアップ、最終ログ |

### PreToolUse

Claudeがツールパラメーターを作成した後、処理の前に実行されます。ツール入力のバリデーションや修正に使用します。

**設定:**
```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "$CLAUDE_PROJECT_DIR/.claude/hooks/validate-bash.py"
          }
        ]
      }
    ]
  }
}
```

**よく使うマッチャー:** `Task`、`Bash`、`Glob`、`Grep`、`Read`、`Edit`、`Write`、`WebFetch`、`WebSearch`

**出力制御:**
- `permissionDecision`: `"allow"`、`"deny"`、または `"ask"`
- `permissionDecisionReason`: 決定の説明
- `updatedInput`: 修正されたツール入力パラメーター

### PostToolUse

ツール完了直後に実行されます。バリデーション、ログ、またはClaudeへのコンテキスト提供に使用します。

**設定:**
```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "type": "command",
            "command": "$CLAUDE_PROJECT_DIR/.claude/hooks/security-scan.py"
          }
        ]
      }
    ]
  }
}
```

**出力制御:**
- `"block"` 決定でClaudeにフィードバックとともにプロンプト
- `additionalContext`: Claudeに追加するコンテキスト

### UserPromptSubmit

ユーザーがプロンプトを送信した時、Claudeが処理する前に実行されます。

**設定:**
```json
{
  "hooks": {
    "UserPromptSubmit": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "$CLAUDE_PROJECT_DIR/.claude/hooks/validate-prompt.py"
          }
        ]
      }
    ]
  }
}
```

**出力制御:**
- `decision`: 処理を防ぐには `"block"`
- `reason`: ブロックされた場合の説明
- `additionalContext`: プロンプトに追加するコンテキスト

### StopとSubagentStop

Claudeが応答を完了した時（Stop）またはサブエージェントが完了した時（SubagentStop）に実行されます。インテリジェントなタスク完了チェックのためのプロンプトベースの評価をサポートします。

**追加入力フィールド:** `Stop` と `SubagentStop` の両方のhookは、JSON入力に `last_assistant_message` フィールドを受け取ります。このフィールドには停止前のClaudeまたはサブエージェントからの最後のメッセージが含まれます。タスク完了の評価に役立ちます。

**設定:**
```json
{
  "hooks": {
    "Stop": [
      {
        "hooks": [
          {
            "type": "prompt",
            "prompt": "Claudeがリクエストされたすべてのタスクを完了したか評価してください。",
            "timeout": 30
          }
        ]
      }
    ]
  }
}
```

### SubagentStart

サブエージェントが実行を開始する時に実行されます。マッチャー入力はエージェントタイプ名で、特定のサブエージェントタイプをターゲットにしたhookを設定できます。

**設定:**
```json
{
  "hooks": {
    "SubagentStart": [
      {
        "matcher": "code-review",
        "hooks": [
          {
            "type": "command",
            "command": "$CLAUDE_PROJECT_DIR/.claude/hooks/subagent-init.sh"
          }
        ]
      }
    ]
  }
}
```

### SessionStart

セッションが開始または再開する時に実行されます。環境変数を永続化できます。

**マッチャー:** `startup`、`resume`、`clear`、`compact`

**特別機能:** `CLAUDE_ENV_FILE` を使って環境変数を永続化（`CwdChanged` と `FileChanged` フックでも利用可能）：

```bash
#!/bin/bash
if [ -n "$CLAUDE_ENV_FILE" ]; then
  echo 'export NODE_ENV=development' >> "$CLAUDE_ENV_FILE"
fi
exit 0
```

### SessionEnd

セッション終了時にクリーンアップや最終ログを実行します。終了をブロックできません。

**reasonフィールドの値:**
- `clear` - ユーザーがセッションをクリアした
- `logout` - ユーザーがログアウトした
- `prompt_input_exit` - ユーザーがプロンプト入力を通じて終了した
- `other` - その他の理由

**設定:**
```json
{
  "hooks": {
    "SessionEnd": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "\"$CLAUDE_PROJECT_DIR/.claude/hooks/session-cleanup.sh\""
          }
        ]
      }
    ]
  }
}
```

### Notificationイベント

通知イベントのマッチャー：
- `permission_prompt` - パーミッションリクエスト通知
- `idle_prompt` - アイドル状態通知
- `auth_success` - 認証成功
- `elicitation_dialog` - ユーザーに表示されるダイアログ

## コンポーネントスコープのHooks

Hooksはフロントマターでスキルやエージェントなどのコンポーネントに添付できます：

**SKILL.md、agent.md、またはcommand.mdで:**

```yaml
---
name: secure-operations
description: セキュリティチェック付きで操作を実行
hooks:
  PreToolUse:
    - matcher: "Bash"
      hooks:
        - type: command
          command: "./scripts/check.sh"
          once: true  # セッションにつき1回だけ実行
---
```

**コンポーネントhooksでサポートされるイベント:** `PreToolUse`、`PostToolUse`、`Stop`

これにより、関連するhookをそれを使用するコンポーネント内で直接定義し、コードを一箇所にまとめられます。

### サブエージェントフロントマターのHooks

サブエージェントのフロントマターで `Stop` フックが定義されている場合、そのサブエージェントにスコープされた `SubagentStop` フックに自動的に変換されます。これにより、メインセッションが停止した時ではなく、その特定のサブエージェントが完了した時にのみstopフックが発火するようになります。

```yaml
---
name: code-review-agent
description: 自動化されたコードレビューサブエージェント
hooks:
  Stop:
    - hooks:
        - type: prompt
          prompt: "コードレビューが徹底的で完全かどうか確認してください。"
  # 上記のStopフックはこのサブエージェントのSubagentStopに自動変換される
---
```

## PermissionRequestイベント

カスタム出力形式でパーミッションリクエストを処理します：

```json
{
  "hookSpecificOutput": {
    "hookEventName": "PermissionRequest",
    "decision": {
      "behavior": "allow|deny",
      "updatedInput": {},
      "message": "カスタムメッセージ",
      "interrupt": false
    }
  }
}
```

## Hookの入力と出力

### JSON入力（stdin経由）

すべてのhookはstdin経由でJSON入力を受け取ります：

```json
{
  "session_id": "abc123",
  "transcript_path": "/path/to/transcript.jsonl",
  "cwd": "/current/working/directory",
  "permission_mode": "default",
  "hook_event_name": "PreToolUse",
  "tool_name": "Write",
  "tool_input": {
    "file_path": "/path/to/file.js",
    "content": "..."
  },
  "tool_use_id": "toolu_01ABC123...",
  "agent_id": "agent-abc123",
  "agent_type": "main",
  "worktree": "/path/to/worktree"
}
```

**共通フィールド:**

| フィールド | 説明 |
|-----------|------|
| `session_id` | 一意のセッション識別子 |
| `transcript_path` | 会話トランスクリプトファイルへのパス |
| `cwd` | 現在の作業ディレクトリ |
| `hook_event_name` | hookをトリガーしたイベントの名前 |
| `agent_id` | このhookを実行するエージェントの識別子 |
| `agent_type` | エージェントの種類（`"main"`、サブエージェントタイプ名など） |
| `worktree` | エージェントがworktree内で実行されている場合のgit worktreeのパス |

### 終了コード

| 終了コード | 意味 | 動作 |
|-----------|------|------|
| **0** | 成功 | 続行、JSON stdoutを解析 |
| **2** | ブロッキングエラー | 操作をブロック、stderrをエラーとして表示 |
| **その他** | 非ブロッキングエラー | 続行、stderrをverboseモードで表示 |

### JSON出力（stdout、終了コード0）

```json
{
  "continue": true,
  "stopReason": "停止する場合のオプションメッセージ",
  "suppressOutput": false,
  "systemMessage": "オプションの警告メッセージ",
  "hookSpecificOutput": {
    "hookEventName": "PreToolUse",
    "permissionDecision": "allow",
    "permissionDecisionReason": "ファイルは許可されたディレクトリ内にある",
    "updatedInput": {
      "file_path": "/modified/path.js"
    }
  }
}
```

## 環境変数

| 変数 | 利用可能な場所 | 説明 |
|-----|-------------|------|
| `CLAUDE_PROJECT_DIR` | すべてのhooks | プロジェクトルートへの絶対パス |
| `CLAUDE_ENV_FILE` | SessionStart、CwdChanged、FileChanged | 環境変数永続化のためのファイルパス |
| `CLAUDE_CODE_REMOTE` | すべてのhooks | リモート環境で実行中の場合 `"true"` |
| `${CLAUDE_PLUGIN_ROOT}` | プラグインhooks | プラグインディレクトリへのパス |
| `${CLAUDE_PLUGIN_DATA}` | プラグインhooks | プラグインデータディレクトリへのパス |
| `CLAUDE_CODE_SESSIONEND_HOOKS_TIMEOUT_MS` | SessionEndフック | SessionEndフックの設定可能なタイムアウト（ミリ秒） |

## プロンプトベースのHooks

`Stop` と `SubagentStop` イベントでは、LLMベースの評価を使えます：

```json
{
  "hooks": {
    "Stop": [
      {
        "hooks": [
          {
            "type": "prompt",
            "prompt": "すべてのタスクが完了しているか確認してください。決定を返してください。",
            "timeout": 30
          }
        ]
      }
    ]
  }
}
```

**LLMレスポンスのスキーマ:**
```json
{
  "decision": "approve",
  "reason": "すべてのタスクが正常に完了した",
  "continue": false,
  "stopReason": "タスク完了"
}
```

## 例

### 例1: Bashコマンドバリデーター（PreToolUse）

**ファイル:** `.claude/hooks/validate-bash.py`

```python
#!/usr/bin/env python3
import json
import sys
import re

BLOCKED_PATTERNS = [
    (r"\brm\s+-rf\s+/", "危険なrm -rf /コマンドをブロック"),
    (r"\bsudo\s+rm", "sudo rmコマンドをブロック"),
]

def main():
    input_data = json.load(sys.stdin)

    tool_name = input_data.get("tool_name", "")
    if tool_name != "Bash":
        sys.exit(0)

    command = input_data.get("tool_input", {}).get("command", "")

    for pattern, message in BLOCKED_PATTERNS:
        if re.search(pattern, command):
            print(message, file=sys.stderr)
            sys.exit(2)  # 終了コード2 = ブロッキングエラー

    sys.exit(0)

if __name__ == "__main__":
    main()
```

**設定:**
```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "python3 \"$CLAUDE_PROJECT_DIR/.claude/hooks/validate-bash.py\""
          }
        ]
      }
    ]
  }
}
```

### 例2: セキュリティスキャナー（PostToolUse）

**ファイル:** `.claude/hooks/security-scan.py`

```python
#!/usr/bin/env python3
import json
import sys
import re

SECRET_PATTERNS = [
    (r"password\s*=\s*['\"][^'\"]+['\"]", "ハードコードされたパスワードの可能性"),
    (r"api[_-]?key\s*=\s*['\"][^'\"]+['\"]", "ハードコードされたAPIキーの可能性"),
]

def main():
    input_data = json.load(sys.stdin)

    tool_name = input_data.get("tool_name", "")
    if tool_name not in ["Write", "Edit"]:
        sys.exit(0)

    tool_input = input_data.get("tool_input", {})
    content = tool_input.get("content", "") or tool_input.get("new_string", "")
    file_path = tool_input.get("file_path", "")

    warnings = []
    for pattern, message in SECRET_PATTERNS:
        if re.search(pattern, content, re.IGNORECASE):
            warnings.append(message)

    if warnings:
        output = {
            "hookSpecificOutput": {
                "hookEventName": "PostToolUse",
                "additionalContext": f"{file_path}のセキュリティ警告: " + "; ".join(warnings)
            }
        }
        print(json.dumps(output))

    sys.exit(0)

if __name__ == "__main__":
    main()
```

### 例3: コードの自動フォーマット（PostToolUse）

**ファイル:** `.claude/hooks/format-code.sh`

```bash
#!/bin/bash

# stdinからJSONを読み込む
INPUT=$(cat)
TOOL_NAME=$(echo "$INPUT" | python3 -c "import sys, json; print(json.load(sys.stdin).get('tool_name', ''))")
FILE_PATH=$(echo "$INPUT" | python3 -c "import sys, json; print(json.load(sys.stdin).get('tool_input', {}).get('file_path', ''))")

if [ "$TOOL_NAME" != "Write" ] && [ "$TOOL_NAME" != "Edit" ]; then
    exit 0
fi

# 拡張子に基づいてフォーマット
case "$FILE_PATH" in
    *.js|*.jsx|*.ts|*.tsx|*.json)
        command -v prettier &>/dev/null && prettier --write "$FILE_PATH" 2>/dev/null
        ;;
    *.py)
        command -v black &>/dev/null && black "$FILE_PATH" 2>/dev/null
        ;;
    *.go)
        command -v gofmt &>/dev/null && gofmt -w "$FILE_PATH" 2>/dev/null
        ;;
esac

exit 0
```

### 例4: プロンプトバリデーター（UserPromptSubmit）

**ファイル:** `.claude/hooks/validate-prompt.py`

```python
#!/usr/bin/env python3
import json
import sys
import re

BLOCKED_PATTERNS = [
    (r"delete\s+(all\s+)?database", "危険: データベースの削除"),
    (r"rm\s+-rf\s+/", "危険: ルートの削除"),
]

def main():
    input_data = json.load(sys.stdin)
    prompt = input_data.get("user_prompt", "") or input_data.get("prompt", "")

    for pattern, message in BLOCKED_PATTERNS:
        if re.search(pattern, prompt, re.IGNORECASE):
            output = {
                "decision": "block",
                "reason": f"ブロック: {message}"
            }
            print(json.dumps(output))
            sys.exit(0)

    sys.exit(0)

if __name__ == "__main__":
    main()
```

### 例5: インテリジェントなStopフック（プロンプトベース）

```json
{
  "hooks": {
    "Stop": [
      {
        "hooks": [
          {
            "type": "prompt",
            "prompt": "Claudeがリクエストされたすべてのタスクを完了したか確認してください。チェック: 1) すべてのファイルが作成/修正されたか? 2) 未解決のエラーがあるか? 不完全な場合は、何が欠けているか説明してください。",
            "timeout": 30
          }
        ]
      }
    ]
  }
}
```

### 例6: コンテキスト使用量トラッカー（Hookペア）

`UserPromptSubmit`（プレメッセージ）と `Stop`（ポストレスポンス）フックを組み合わせてリクエストごとのトークン消費を追跡します。

**ファイル:** `.claude/hooks/context-tracker.py`

```python
#!/usr/bin/env python3
"""
Context Usage Tracker - リクエストごとのトークン消費を追跡。

UserPromptSubmitを「プレメッセージ」フックとして、Stopを「ポストレスポンス」フックとして使用し、
各リクエストのトークン使用量のデルタを計算します。

トークンカウント方法:
1. 文字数推定（デフォルト）: 約4文字/トークン、依存関係なし
2. tiktoken（省略可）: より正確（約90-95%）、必要: pip install tiktoken
"""
import json
import os
import sys
import tempfile

# 設定
CONTEXT_LIMIT = 128000  # Claudeのコンテキストウィンドウ（モデルに合わせて調整）
USE_TIKTOKEN = False    # 精度向上にはtiktokenインストール後にTrueに設定


def get_state_file(session_id: str) -> str:
    """セッションで分離された、プレメッセージトークン数保存用の一時ファイルパスを取得。"""
    return os.path.join(tempfile.gettempdir(), f"claude-context-{session_id}.json")


def count_tokens(text: str) -> int:
    """テキストのトークン数をカウント。"""
    if USE_TIKTOKEN:
        try:
            import tiktoken
            enc = tiktoken.get_encoding("p50k_base")
            return len(enc.encode(text))
        except ImportError:
            pass  # 推定にフォールバック

    # 文字ベースの推定: 英語テキストは約4文字/トークン
    return len(text) // 4


def read_transcript(transcript_path: str) -> str:
    """トランスクリプトファイルからすべてのコンテンツを読み込んで結合。"""
    if not transcript_path or not os.path.exists(transcript_path):
        return ""

    content = []
    with open(transcript_path, "r") as f:
        for line in f:
            try:
                entry = json.loads(line.strip())
                if "message" in entry:
                    msg = entry["message"]
                    if isinstance(msg.get("content"), str):
                        content.append(msg["content"])
                    elif isinstance(msg.get("content"), list):
                        for block in msg["content"]:
                            if isinstance(block, dict) and block.get("type") == "text":
                                content.append(block.get("text", ""))
            except json.JSONDecodeError:
                continue

    return "\n".join(content)


def handle_user_prompt_submit(data: dict) -> None:
    """プレメッセージフック: リクエスト前の現在のトークン数を保存。"""
    session_id = data.get("session_id", "unknown")
    transcript_path = data.get("transcript_path", "")

    transcript_content = read_transcript(transcript_path)
    current_tokens = count_tokens(transcript_content)

    state_file = get_state_file(session_id)
    with open(state_file, "w") as f:
        json.dump({"pre_tokens": current_tokens}, f)


def handle_stop(data: dict) -> None:
    """ポストレスポンスフック: トークンデルタを計算してレポート。"""
    session_id = data.get("session_id", "unknown")
    transcript_path = data.get("transcript_path", "")

    transcript_content = read_transcript(transcript_path)
    current_tokens = count_tokens(transcript_content)

    state_file = get_state_file(session_id)
    pre_tokens = 0
    if os.path.exists(state_file):
        try:
            with open(state_file, "r") as f:
                state = json.load(f)
                pre_tokens = state.get("pre_tokens", 0)
        except (json.JSONDecodeError, IOError):
            pass

    delta_tokens = current_tokens - pre_tokens
    remaining = CONTEXT_LIMIT - current_tokens
    percentage = (current_tokens / CONTEXT_LIMIT) * 100

    method = "tiktoken" if USE_TIKTOKEN else "推定"
    print(f"コンテキスト（{method}）: 約{current_tokens:,}トークン（{percentage:.1f}%使用済み、約{remaining:,}残り）", file=sys.stderr)
    if delta_tokens > 0:
        print(f"このリクエスト: 約{delta_tokens:,}トークン", file=sys.stderr)


def main():
    data = json.load(sys.stdin)
    event = data.get("hook_event_name", "")

    if event == "UserPromptSubmit":
        handle_user_prompt_submit(data)
    elif event == "Stop":
        handle_stop(data)

    sys.exit(0)


if __name__ == "__main__":
    main()
```

**設定:**
```json
{
  "hooks": {
    "UserPromptSubmit": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "python3 \"$CLAUDE_PROJECT_DIR/.claude/hooks/context-tracker.py\""
          }
        ]
      }
    ],
    "Stop": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "python3 \"$CLAUDE_PROJECT_DIR/.claude/hooks/context-tracker.py\""
          }
        ]
      }
    ]
  }
}
```

**仕組み:**
1. `UserPromptSubmit` がプロンプト処理前に発火 - 現在のトークン数を保存
2. `Stop` がClaudeの応答後に発火 - デルタを計算してレポート
3. 各セッションは一時ファイル名の `session_id` で分離

**トークンカウント方法:**

| 方法 | 精度 | 依存関係 | 速度 |
|------|------|---------|------|
| 文字数推定 | 約80-90% | なし | <1ms |
| tiktoken (p50k_base) | 約90-95% | `pip install tiktoken` | <10ms |

### 例7: 自動アダプトモード（PostToolUse）

ツール承認から自動的に学習し、`~/.claude/settings.json` のパーミッションを更新します。ツール実行を承認するたびに、hookがコマンドを再利用可能なパーミッションルールに汎用化します — 同じ種類のコマンドを二度承認する必要がなくなります。危険/破壊的なコマンドは**決して**記憶されません。

**ファイル:** `.claude/hooks/auto-adapt-mode.py`

```python
#!/usr/bin/env python3
"""
auto-adapt-mode: ユーザーのツール承認から学習してClaude設定を更新。

Hookの種類: PostToolUse
イベント: ツールが正常に実行された後（ユーザーが承認したことを意味する）に発火
"""

import json
import os
import sys
import re
from pathlib import Path

SETTINGS_PATH = Path.home() / ".claude" / "settings.json"
LOG_PATH = Path.home() / ".claude" / "auto-adapt-mode.log"

# 自動モードのベースライン: 安全、ローカル、可逆な操作
AUTO_MODE_BASELINE = [
    "Read(*)", "Edit(*)", "Write(*)", "Glob(*)", "Grep(*)",
    "Bash(git status:*)", "Bash(git log:*)", "Bash(git diff:*)",
    "Bash(git add:*)", "Bash(git commit:*)", "Bash(git checkout:*)",
    "Bash(npm install:*)", "Bash(npm test:*)", "Bash(npm run:*)",
    "Bash(pip install:*)", "Bash(pytest:*)",
    "Bash(ls:*)", "Bash(cat:*)", "Bash(find:*)", "Bash(mkdir:*)",
    "Bash(cp:*)", "Bash(mv:*)", "Bash(chmod:*)",
    "Bash(gh pr view:*)", "Bash(gh issue list:*)",
    "Agent(*)", "Skill(*)", "WebSearch(*)", "WebFetch(*)",
    # ...（フルリストには70以上の安全なパターンを含む）
]

# 決して自動記憶されないコマンド
DANGEROUS_PATTERNS = [
    r"rm\s+(-[a-zA-Z]*r[a-zA-Z]*|--recursive)",   # rm -rf
    r"git\s+push\s+(-[a-zA-Z]*f|--force)",          # force push
    r"git\s+reset\s+--hard",                         # hard reset
    r"DROP\s+(TABLE|DATABASE)",                       # SQL破壊的操作
    r"curl\s+.*\|\s*(bash|sh)",                       # シェルへのパイプ
    r"sudo\b",                                        # 権限昇格
    r"docker\s+(rm|rmi|system\s+prune)",              # コンテナ破壊的操作
    r"kubectl\s+delete",                              # k8s破壊的操作
    r"terraform\s+destroy",                           # インフラ破壊的操作
    r"npm\s+publish",                                 # 不可逆な公開
    r"deploy\s+.*prod",                               # 本番デプロイ
    # ...（フルリストには25以上のパターンを含む）
]


def is_dangerous_command(command: str) -> bool:
    """bashコマンドが危険なパターンにマッチするか確認。"""
    return any(re.search(p, command, re.IGNORECASE) for p in DANGEROUS_PATTERNS)


def generalize_tool_permission(tool_name: str, tool_input: dict) -> str | None:
    """特定のツール呼び出しを汎用化されたパーミッションルールに変換。"""
    if tool_name == "Bash":
        command = tool_input.get("command", "")
        if not command or is_dangerous_command(command):
            return None
        parts = command.strip().split()
        base = parts[0]
        compound = ["git", "npm", "npx", "pip", "cargo", "go", "gh", "python3"]
        if base in compound and len(parts) > 1:
            sub = parts[1]
            if sub.lower() in {"rm", "delete", "destroy", "publish"}:
                return None
            return f"Bash({base} {sub}:*)"
        return f"Bash({base}:*)"
    else:
        return f"{tool_name}(*)"


def main():
    try:
        hook_input = json.load(sys.stdin)
    except (json.JSONDecodeError, EOFError):
        sys.exit(0)

    tool_name = hook_input.get("tool_name", "")
    tool_input = hook_input.get("tool_input", {})
    if not tool_name:
        sys.exit(0)

    settings = json.load(open(SETTINGS_PATH)) if SETTINGS_PATH.exists() else {}
    allow = settings.setdefault("permissions", {}).setdefault("allow", [])

    marker = Path.home() / ".claude" / ".auto-adapt-mode-initialized"
    if not marker.exists():
        existing = set(allow)
        for rule in AUTO_MODE_BASELINE:
            if rule not in existing:
                allow.append(rule)
        marker.touch()

    rule = generalize_tool_permission(tool_name, tool_input)
    if rule and rule not in allow:
        allow.append(rule)
        with open(SETTINGS_PATH, "w") as f:
            json.dump(settings, f, indent=2)
            f.write("\n")

    sys.exit(0)

if __name__ == "__main__":
    main()
```

**設定:**
```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "*",
        "hooks": [
          {
            "type": "command",
            "command": "python3 \"$CLAUDE_PROJECT_DIR/.claude/hooks/auto-adapt-mode.py\"",
            "timeout": 10
          }
        ]
      }
    ]
  }
}
```

**汎用化の例:**

| 承認したコマンド | 追加されたルール | 対象範囲 |
|-------------|-----------|--------|
| `git push origin main` | `Bash(git push:*)` | すべてのgit pushのバリエーション |
| `npm run build` | `Bash(npm run:*)` | すべてのnpmスクリプト |
| `ls -la src/` | `Bash(ls:*)` | すべてのls呼び出し |
| `rm -rf /tmp/test` | *（ブロック）* | 決して記憶されない |
| `git push --force` | *（ブロック）* | 決して記憶されない |
| `Write` ツール | `Write(*)` | すべてのファイル書き込み |

## プラグインHooks

プラグインは `hooks/hooks.json` ファイルにhooksを含められます：

**ファイル:** `plugins/hooks/hooks.json`

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "${CLAUDE_PLUGIN_ROOT}/scripts/validate.sh"
          }
        ]
      }
    ]
  }
}
```

**プラグインhooksの環境変数:**
- `${CLAUDE_PLUGIN_ROOT}` - プラグインディレクトリへのパス
- `${CLAUDE_PLUGIN_DATA}` - プラグインデータディレクトリへのパス

これにより、プラグインにカスタムバリデーションと自動化hooksを含められます。

## MCPツールのHooks

MCPツールは `mcp__<server>__<tool>` のパターンに従います：

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "mcp__memory__.*",
        "hooks": [
          {
            "type": "command",
            "command": "echo '{\"systemMessage\": \"メモリ操作をログ記録\"}'"
          }
        ]
      }
    ]
  }
}
```

## セキュリティに関する考慮事項

### 免責事項

**自己責任で使用してください**: Hooksは任意のシェルコマンドを実行します。以下については利用者の責任となります：
- 設定するコマンド
- ファイルアクセス/修正パーミッション
- データ損失やシステム損傷の可能性
- 本番環境での使用前に安全な環境でhooksをテストすること

### セキュリティメモ

- **ワークスペースの信頼が必要:** `statusLine` と `fileSuggestion` のhook出力コマンドは、効果を発揮する前にワークスペースの信頼承認が必要になりました。
- **HTTPフックと環境変数:** HTTPフックでURLの環境変数インターポレーションを使用するには明示的な `allowedEnvVars` リストが必要です。これにより機密環境変数が誤ってリモートエンドポイントに漏洩することを防ぎます。
- **マネージド設定の階層:** `disableAllHooks` 設定はマネージド設定の階層を尊重するようになり、組織レベルの設定で個々のユーザーが上書きできないhookの無効化を強制できます。

### ベストプラクティス

| やること | やってはいけないこと |
|---------|-------------|
| すべての入力をバリデートして無害化する | 入力データを盲目的に信頼する |
| シェル変数をクォートする: `"$VAR"` | クォートなしで使用: `$VAR` |
| パストラバーサル（`..`）をブロックする | 任意のパスを許可する |
| `$CLAUDE_PROJECT_DIR` で絶対パスを使用 | パスをハードコードする |
| 機密ファイル（`.env`、`.git/`、鍵）をスキップ | すべてのファイルを処理する |
| まず隔離環境でhooksをテストする | テストせずにhooksをデプロイする |
| HTTPフックには明示的な `allowedEnvVars` を使用 | すべての環境変数をウェブフックに公開する |

## デバッグ

### デバッグモードを有効化

デバッグフラグ付きでClaudeを実行して詳細なhookログを取得：

```bash
claude --debug
```

### Verboseモード

Claude Codeで `Ctrl+O` を使ってverboseモードを有効化し、hookの実行状況を確認します。

### Hooksを独立してテスト

```bash
# サンプルJSON入力でテスト
echo '{"tool_name": "Bash", "tool_input": {"command": "ls -la"}}' | python3 .claude/hooks/validate-bash.py

# 終了コードを確認
echo $?
```

## 完全な設定例

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "python3 \"$CLAUDE_PROJECT_DIR/.claude/hooks/validate-bash.py\"",
            "timeout": 10
          }
        ]
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "type": "command",
            "command": "\"$CLAUDE_PROJECT_DIR/.claude/hooks/format-code.sh\"",
            "timeout": 30
          },
          {
            "type": "command",
            "command": "python3 \"$CLAUDE_PROJECT_DIR/.claude/hooks/security-scan.py\"",
            "timeout": 10
          }
        ]
      }
    ],
    "UserPromptSubmit": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "python3 \"$CLAUDE_PROJECT_DIR/.claude/hooks/validate-prompt.py\""
          }
        ]
      }
    ],
    "SessionStart": [
      {
        "matcher": "startup",
        "hooks": [
          {
            "type": "command",
            "command": "\"$CLAUDE_PROJECT_DIR/.claude/hooks/session-init.sh\""
          }
        ]
      }
    ],
    "Stop": [
      {
        "hooks": [
          {
            "type": "prompt",
            "prompt": "停止する前にすべてのタスクが完了しているか確認してください。",
            "timeout": 30
          }
        ]
      }
    ]
  }
}
```

## Hook実行の詳細

| 観点 | 動作 |
|------|------|
| **タイムアウト** | デフォルト60秒、コマンドごとに設定可能 |
| **並列化** | マッチするすべてのhookが並列で実行 |
| **重複排除** | 同一のhookコマンドは重複排除される |
| **環境** | Claude Codeの環境で現在のディレクトリで実行 |

## トラブルシューティング

### Hookが実行されない
- JSON設定構文が正しいか確認
- マッチャーパターンがツール名にマッチするか確認
- スクリプトが存在して実行可能か確認: `chmod +x script.sh`
- `claude --debug` を実行してhookの実行ログを確認
- hookがコマンド引数ではなくstdinからJSONを読み込んでいるか確認

### Hookが予期せずブロックする
- サンプルJSONでhookをテスト: `echo '{"tool_name": "Write", ...}' | ./hook.py`
- 終了コードを確認: 許可は0、ブロックは2
- stderr出力を確認（終了コード2の場合に表示）

### JSONパースエラー
- コマンド引数ではなくstdinから常に読み込む
- 適切なJSONパース（文字列操作ではなく）を使用
- 欠けているフィールドを優雅に処理する

## インストール

### ステップ1: Hooksディレクトリを作成
```bash
mkdir -p ~/.claude/hooks
```

### ステップ2: 例のhooksをコピー
```bash
cp 06-hooks/*.sh ~/.claude/hooks/
chmod +x ~/.claude/hooks/*.sh
```

### ステップ3: 設定で構成
`~/.claude/settings.json` または `.claude/settings.json` を上記のhook設定で編集します。

## 関連概念

- **[CheckpointsとRewind](../08-checkpoints/)** - 会話状態の保存と復元
- **[スラッシュコマンド](../01-slash-commands/)** - カスタムスラッシュコマンドの作成
- **[スキル](../03-skills/)** - 再利用可能な自律的な機能
- **[サブエージェント](../04-subagents/)** - 委譲されたタスク実行
- **[プラグイン](../07-plugins/)** - バンドルされた拡張パッケージ
- **[高度な機能](../09-advanced-features/)** - Claude Codeの高度な機能を探索

## 追加リソース

- **[公式Hooksドキュメント](https://code.claude.com/docs/en/hooks)** - 完全なhooksリファレンス
- **[CLIリファレンス](https://code.claude.com/docs/en/cli-reference)** - コマンドラインインターフェースのドキュメント
- **[メモリガイド](../02-memory/)** - 永続コンテキストの設定
