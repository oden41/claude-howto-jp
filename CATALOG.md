<picture>
  <source media="(prefers-color-scheme: dark)" srcset="resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="resources/logos/claude-howto-logo.svg">
</picture>

# Claude Code 機能カタログ

> すべてのClaude Code機能のクイックリファレンス: コマンド、エージェント、skills、plugins、hooksを網羅。

**ナビゲーション**: [コマンド](#slash-commands) | [パーミッションモード](#パーミッションモード) | [Subagents](#subagents) | [Skills](#skills) | [Plugins](#plugins) | [MCPサーバー](#mcpサーバー) | [Hooks](#hooks) | [Memory](#memoryファイル) | [新機能](#新機能-2026年3月)

---

## サマリー

| 機能 | 組み込み | サンプル | 合計 | リファレンス |
|---------|----------|----------|-------|-----------|
| **Slash Commands** | 55以上 | 8 | 63以上 | [01-slash-commands/](01-slash-commands/) |
| **Subagents** | 6 | 10 | 16 | [04-subagents/](04-subagents/) |
| **Skills** | 5バンドル | 4 | 9 | [03-skills/](03-skills/) |
| **Plugins** | - | 3 | 3 | [07-plugins/](07-plugins/) |
| **MCPサーバー** | 1 | 8 | 9 | [05-mcp/](05-mcp/) |
| **Hooks** | 25イベント | 7 | 7 | [06-hooks/](06-hooks/) |
| **Memory** | 7タイプ | 3 | 3 | [02-memory/](02-memory/) |
| **合計** | **99** | **43** | **117** | |

---

## Slash Commands

コマンドは特定のアクションを実行するユーザー呼び出しショートカットです。

### 組み込みコマンド

| コマンド | 説明 | 使う場面 |
|---------|-------------|-------------|
| `/help` | ヘルプ情報を表示 | 始め方・コマンドを学ぶ |
| `/btw` | コンテキストに追加せずに質問 | クイックな脱線質問 |
| `/chrome` | Chrome連携を設定 | ブラウザ自動化 |
| `/clear` | 会話履歴をクリア | 新鮮なスタート、コンテキスト削減 |
| `/diff` | インタラクティブdiffビューア | 変更をレビュー |
| `/config` | 設定を表示/編集 | 動作をカスタマイズ |
| `/status` | セッション状態を表示 | 現在の状態を確認 |
| `/agents` | 利用可能なエージェントを一覧 | 委任オプションを確認 |
| `/skills` | 利用可能なskillsを一覧 | 自動呼び出し機能を確認 |
| `/hooks` | 設定済みhooksを一覧 | 自動化をデバッグ |
| `/insights` | セッションパターンを分析 | セッション最適化 |
| `/install-slack-app` | Claude Slackアプリをインストール | Slack連携 |
| `/keybindings` | キーボードショートカットをカスタマイズ | キーカスタマイズ |
| `/mcp` | MCPサーバーを一覧 | 外部統合を確認 |
| `/memory` | 読み込まれたmemoryファイルを表示 | コンテキスト読み込みのデバッグ |
| `/mobile` | モバイルQRコードを生成 | モバイルアクセス |
| `/passes` | 使用パスを表示 | サブスクリプション情報 |
| `/plugin` | pluginsを管理 | 拡張機能のインストール/削除 |
| `/plan` | Planning modeに入る | 複雑な実装 |
| `/rewind` | checkpointに巻き戻す | 変更を元に戻す・代替案を探る |
| `/checkpoint` | checkpointsを管理 | 状態の保存/復元 |
| `/cost` | トークン使用コストを表示 | 支出を監視 |
| `/context` | コンテキストウィンドウの使用量を表示 | 会話の長さを管理 |
| `/export` | 会話をエクスポート | 参照用に保存 |
| `/extra-usage` | 追加使用制限を設定 | レート制限管理 |
| `/feedback` | フィードバックやバグレポートを送信 | 問題を報告 |
| `/login` | Anthropicで認証 | 機能にアクセス |
| `/logout` | サインアウト | アカウントを切り替え |
| `/sandbox` | サンドボックスモードを切り替え | 安全なコマンド実行 |
| `/vim` | vimモードを切り替え | Vimスタイルの編集 |
| `/doctor` | 診断を実行 | 問題のトラブルシューティング |
| `/reload-plugins` | インストール済みpluginsを再読み込み | plugin管理 |
| `/release-notes` | リリースノートを表示 | 新機能を確認 |
| `/remote-control` | リモートコントロールを有効化 | リモートアクセス |
| `/permissions` | パーミッションを管理 | アクセス制御 |
| `/session` | セッションを管理 | マルチセッションワークフロー |
| `/rename` | 現在のセッションを名前変更 | セッションを整理 |
| `/resume` | 以前のセッションを再開 | 作業を続ける |
| `/todo` | TODOリストを表示/管理 | タスクを追跡 |
| `/tasks` | バックグラウンドタスクを表示 | 非同期処理を監視 |
| `/copy` | 最後の応答をクリップボードにコピー | 出力をすばやく共有 |
| `/teleport` | セッションを別のマシンに転送 | リモートで作業を続ける |
| `/desktop` | Claude Desktopアプリを開く | デスクトップインターフェースに切り替え |
| `/theme` | カラーテーマを変更 | 外観をカスタマイズ |
| `/usage` | API使用統計を表示 | クォータとコストを監視 |
| `/fork` | 現在の会話をフォーク | 代替案を探る |
| `/stats` | セッション統計を表示 | セッションメトリクスを確認 |
| `/statusline` | ステータスラインを設定 | ステータス表示をカスタマイズ |
| `/stickers` | セッションステッカーを表示 | 楽しい報酬 |
| `/fast` | 高速出力モードを切り替え | 応答を高速化 |
| `/terminal-setup` | ターミナル連携を設定 | ターミナル機能をセットアップ |
| `/upgrade` | アップデートを確認 | バージョン管理 |

### カスタムコマンド (サンプル)

| コマンド | 説明 | 使う場面 | スコープ | インストール |
|---------|-------------|-------------|-------|--------------|
| `/optimize` | 最適化のためのコード分析 | パフォーマンス改善 | プロジェクト | `cp 01-slash-commands/optimize.md .claude/commands/` |
| `/pr` | プルリクエストの準備 | PR提出前 | プロジェクト | `cp 01-slash-commands/pr.md .claude/commands/` |
| `/generate-api-docs` | APIドキュメント生成 | APIのドキュメント化 | プロジェクト | `cp 01-slash-commands/generate-api-docs.md .claude/commands/` |
| `/commit` | コンテキスト付きgitコミット作成 | 変更をコミット | ユーザー | `cp 01-slash-commands/commit.md .claude/commands/` |
| `/push-all` | ステージ・コミット・プッシュ | クイックデプロイ | ユーザー | `cp 01-slash-commands/push-all.md .claude/commands/` |
| `/doc-refactor` | ドキュメントの再構成 | ドキュメント改善 | プロジェクト | `cp 01-slash-commands/doc-refactor.md .claude/commands/` |
| `/setup-ci-cd` | CI/CDパイプラインのセットアップ | 新規プロジェクト | プロジェクト | `cp 01-slash-commands/setup-ci-cd.md .claude/commands/` |
| `/unit-test-expand` | テストカバレッジを拡張 | テスト改善 | プロジェクト | `cp 01-slash-commands/unit-test-expand.md .claude/commands/` |

> **スコープ**: `ユーザー` = 個人ワークフロー (`~/.claude/commands/`)、`プロジェクト` = チーム共有 (`.claude/commands/`)

**リファレンス**: [01-slash-commands/](01-slash-commands/) | [公式ドキュメント](https://code.claude.com/docs/en/interactive-mode)

**クイックインストール (すべてのカスタムコマンド)**:
```bash
cp 01-slash-commands/*.md .claude/commands/
```

---

## パーミッションモード

Claude Codeはツール使用の承認方法を制御する6つのパーミッションモードをサポートしています。

| モード | 説明 | 使う場面 |
|------|-------------|-------------|
| `default` | 各ツール呼び出しでプロンプト | 標準的なインタラクティブ使用 |
| `acceptEdits` | ファイル編集を自動承認、その他はプロンプト | 信頼できる編集ワークフロー |
| `plan` | 読み取り専用ツールのみ、書き込みなし | 計画と探索 |
| `auto` | プロンプトなしですべてのツールを承認 | 完全自律操作 (Research Preview) |
| `bypassPermissions` | すべてのパーミッションチェックをスキップ | CI/CD、ヘッドレス環境 |
| `dontAsk` | パーミッションが必要なツールをスキップ | 非インタラクティブスクリプティング |

> **注**: `auto` モードは Research Preview 機能 (2026年3月)。`bypassPermissions` は信頼された・サンドボックス化された環境でのみ使用すること。

**リファレンス**: [公式ドキュメント](https://code.claude.com/docs/en/permissions)

---

## Subagents

特定のタスクのために独立したコンテキストを持つ専門AIアシスタント。

### 組み込みSubagents

| エージェント | 説明 | ツール | モデル | 使う場面 |
|-------|-------------|-------|-------|-------------|
| **general-purpose** | マルチステップタスク、調査 | すべてのツール | 継承 | 複雑な調査、マルチファイルタスク |
| **Plan** | 実装計画 | Read, Glob, Grep, Bash | 継承 | アーキテクチャ設計、計画 |
| **Explore** | コードベース探索 | Read, Glob, Grep | Haiku 4.5 | クイック検索、コード理解 |
| **Bash** | コマンド実行 | Bash | 継承 | Git操作、ターミナルタスク |
| **statusline-setup** | ステータスライン設定 | Bash, Read, Write | Sonnet 4.6 | ステータスライン表示の設定 |
| **Claude Code Guide** | ヘルプとドキュメント | Read, Glob, Grep | Haiku 4.5 | ヘルプを得る・機能を学ぶ |

### Subagent設定フィールド

| フィールド | 型 | 説明 |
|-------|------|-------------|
| `name` | string | エージェントの識別子 |
| `description` | string | エージェントの機能 |
| `model` | string | モデルのオーバーライド (例: `haiku-4.5`) |
| `tools` | array | 許可されたツールリスト |
| `effort` | string | 推論の努力レベル (`low`, `medium`, `high`) |
| `initialPrompt` | string | エージェント開始時に注入されるシステムプロンプト |
| `disallowedTools` | array | このエージェントに明示的に拒否されたツール |

### カスタムSubagents (サンプル)

| エージェント | 説明 | 使う場面 | スコープ | インストール |
|-------|-------------|-------------|-------|--------------|
| `code-reviewer` | 包括的なコード品質 | コードレビューセッション | プロジェクト | `cp 04-subagents/code-reviewer.md .claude/agents/` |
| `code-architect` | 機能アーキテクチャ設計 | 新機能の計画 | プロジェクト | `cp 04-subagents/code-architect.md .claude/agents/` |
| `code-explorer` | 深いコードベース分析 | 既存機能の理解 | プロジェクト | `cp 04-subagents/code-explorer.md .claude/agents/` |
| `clean-code-reviewer` | Clean Code原則のレビュー | 保守性レビュー | プロジェクト | `cp 04-subagents/clean-code-reviewer.md .claude/agents/` |
| `test-engineer` | テスト戦略とカバレッジ | テスト計画 | プロジェクト | `cp 04-subagents/test-engineer.md .claude/agents/` |
| `documentation-writer` | 技術ドキュメント | APIドキュメント、ガイド | プロジェクト | `cp 04-subagents/documentation-writer.md .claude/agents/` |
| `secure-reviewer` | セキュリティ重視のレビュー | セキュリティ監査 | プロジェクト | `cp 04-subagents/secure-reviewer.md .claude/agents/` |
| `implementation-agent` | フル機能実装 | 機能開発 | プロジェクト | `cp 04-subagents/implementation-agent.md .claude/agents/` |
| `debugger` | 根本原因分析 | バグ調査 | ユーザー | `cp 04-subagents/debugger.md .claude/agents/` |
| `data-scientist` | SQLクエリ、データ分析 | データタスク | ユーザー | `cp 04-subagents/data-scientist.md .claude/agents/` |

> **スコープ**: `ユーザー` = 個人 (`~/.claude/agents/`)、`プロジェクト` = チーム共有 (`.claude/agents/`)

**リファレンス**: [04-subagents/](04-subagents/) | [公式ドキュメント](https://code.claude.com/docs/en/sub-agents)

**クイックインストール (すべてのカスタムエージェント)**:
```bash
cp 04-subagents/*.md .claude/agents/
```

---

## Skills

指示・スクリプト・テンプレートを持つ自動呼び出し機能。

### サンプルSkills

| Skill | 説明 | 自動呼び出しのタイミング | スコープ | インストール |
|-------|-------------|-------------------|-------|--------------|
| `code-review` | 包括的なコードレビュー | 「このコードをレビューして」「品質を確認して」 | プロジェクト | `cp -r 03-skills/code-review .claude/skills/` |
| `brand-voice` | ブランド一貫性チェッカー | マーケティングコピーの作成 | プロジェクト | `cp -r 03-skills/brand-voice .claude/skills/` |
| `doc-generator` | APIドキュメント生成 | 「ドキュメントを生成して」「APIをドキュメント化して」 | プロジェクト | `cp -r 03-skills/doc-generator .claude/skills/` |
| `refactor` | 体系的なコードリファクタリング (Martin Fowler流) | 「リファクタリングして」「コードをきれいにして」 | ユーザー | `cp -r 03-skills/refactor ~/.claude/skills/` |

> **スコープ**: `ユーザー` = 個人 (`~/.claude/skills/`)、`プロジェクト` = チーム共有 (`.claude/skills/`)

### Skill構造

```
~/.claude/skills/skill-name/
├── SKILL.md          # Skill定義と指示
├── scripts/          # ヘルパースクリプト
└── templates/        # 出力テンプレート
```

### SKILLフロントマターフィールド

`SKILL.md` のYAMLフロントマターで設定をサポート:

| フィールド | 型 | 説明 |
|-------|------|-------------|
| `name` | string | Skillの表示名 |
| `description` | string | Skillの機能 |
| `autoInvoke` | array | 自動呼び出しのトリガーフレーズ |
| `effort` | string | 推論の努力レベル (`low`, `medium`, `high`) |
| `shell` | string | スクリプトに使用するシェル (`bash`, `zsh`, `sh`) |

**リファレンス**: [03-skills/](03-skills/) | [公式ドキュメント](https://code.claude.com/docs/en/skills)

**クイックインストール (すべてのSkills)**:
```bash
cp -r 03-skills/* ~/.claude/skills/
```

### バンドルSkills

| Skill | 説明 | 自動呼び出しのタイミング |
|-------|-------------|-------------------|
| `/simplify` | コード品質のレビュー | コード作成後 |
| `/batch` | 複数ファイルにプロンプトを実行 | バッチ操作 |
| `/debug` | テスト失敗/エラーのデバッグ | デバッグセッション |
| `/loop` | 一定間隔でプロンプトを実行 | 繰り返しタスク |
| `/claude-api` | Claude APIを使ったアプリ構築 | API開発 |

---

## Plugins

コマンド・エージェント・MCPサーバー・hooksのバンドルコレクション。

### サンプルPlugins

| Plugin | 説明 | コンポーネント | 使う場面 | スコープ | インストール |
|--------|-------------|------------|-------------|-------|--------------|
| `pr-review` | PRレビューワークフロー | 3コマンド、3エージェント、GitHub MCP | コードレビュー | プロジェクト | `/plugin install pr-review` |
| `devops-automation` | デプロイとモニタリング | 4コマンド、3エージェント、K8s MCP | DevOpsタスク | プロジェクト | `/plugin install devops-automation` |
| `documentation` | ドキュメント生成スイート | 4コマンド、3エージェント、テンプレート | ドキュメント作成 | プロジェクト | `/plugin install documentation` |

> **スコープ**: `プロジェクト` = チーム共有、`ユーザー` = 個人ワークフロー

### Plugin構造

```
.claude-plugin/
├── plugin.json       # マニフェストファイル
├── commands/         # Slash commands
├── agents/           # Subagents
├── skills/           # Skills
├── mcp/              # MCP設定
├── hooks/            # Hookスクリプト
└── scripts/          # ユーティリティスクリプト
```

**リファレンス**: [07-plugins/](07-plugins/) | [公式ドキュメント](https://code.claude.com/docs/en/plugins)

**Plugin管理コマンド**:
```bash
/plugin list              # インストール済みpluginsを一覧
/plugin install <name>    # pluginをインストール
/plugin remove <name>     # pluginを削除
/plugin update <name>     # pluginをアップデート
```

---

## MCPサーバー

外部ツールとAPIアクセスのためのModel Context Protocolサーバー。

### 一般的なMCPサーバー

| サーバー | 説明 | 使う場面 | スコープ | インストール |
|--------|-------------|-------------|-------|--------------|
| **GitHub** | PR管理、issues、コード | GitHubワークフロー | プロジェクト | `claude mcp add github -- npx -y @modelcontextprotocol/server-github` |
| **Database** | SQLクエリ、データアクセス | データベース操作 | プロジェクト | `claude mcp add db -- npx -y @modelcontextprotocol/server-postgres` |
| **Filesystem** | 高度なファイル操作 | 複雑なファイルタスク | ユーザー | `claude mcp add fs -- npx -y @modelcontextprotocol/server-filesystem` |
| **Slack** | チームコミュニケーション | 通知、更新 | プロジェクト | settingsで設定 |
| **Google Docs** | ドキュメントアクセス | ドキュメント編集・レビュー | プロジェクト | settingsで設定 |
| **Asana** | プロジェクト管理 | タスク追跡 | プロジェクト | settingsで設定 |
| **Stripe** | 支払いデータ | 財務分析 | プロジェクト | settingsで設定 |
| **Memory** | 永続的なmemory | セッション間の記憶 | ユーザー | settingsで設定 |
| **Context7** | ライブラリドキュメント | 最新ドキュメントの参照 | 組み込み | 組み込み |

> **スコープ**: `プロジェクト` = チーム (`.mcp.json`)、`ユーザー` = 個人 (`~/.claude.json`)、`組み込み` = プリインストール済み

### MCP設定例

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

**リファレンス**: [05-mcp/](05-mcp/) | [MCPプロトコルドキュメント](https://modelcontextprotocol.io)

**クイックインストール (GitHub MCP)**:
```bash
export GITHUB_TOKEN="your_token" && claude mcp add github -- npx -y @modelcontextprotocol/server-github
```

---

## Hooks

Claude Codeのイベントに応じてシェルコマンドを実行するイベント駆動自動化。

### Hookイベント

| イベント | 説明 | トリガーのタイミング | ユースケース |
|-------|-------------|----------------|-----------|
| `SessionStart` | セッション開始/再開 | セッション初期化 | セットアップタスク |
| `InstructionsLoaded` | 指示が読み込まれた | CLAUDE.mdまたはルールファイルが読み込まれた | カスタム指示処理 |
| `UserPromptSubmit` | プロンプト処理前 | ユーザーがメッセージを送信 | 入力バリデーション |
| `PreToolUse` | ツール実行前 | 任意のツール実行前 | バリデーション、ログ記録 |
| `PermissionRequest` | パーミッションダイアログ表示 | 機密性の高い操作の前 | カスタム承認フロー |
| `PostToolUse` | ツール成功後 | 任意のツール完了後 | フォーマット、通知 |
| `PostToolUseFailure` | ツール実行失敗 | ツールエラー後 | エラー処理、ログ記録 |
| `Notification` | 通知送信 | Claudeが通知を送信 | 外部アラート |
| `SubagentStart` | Subagentが起動 | Subagentタスク開始 | Subagentコンテキストを初期化 |
| `SubagentStop` | Subagentが終了 | Subagentタスク完了 | アクションの連鎖 |
| `Stop` | Claudeが応答を終了 | 応答完了 | クリーンアップ、レポート |
| `StopFailure` | APIエラーでターン終了 | APIエラー発生 | エラー回復、ログ記録 |
| `TeammateIdle` | チームメイトエージェントがアイドル | エージェントチーム調整 | 作業を分散 |
| `TaskCompleted` | タスクが完了としてマーク | タスク完了 | タスク後処理 |
| `TaskCreated` | TaskCreateでタスクが作成 | 新しいタスクが作成された | タスク追跡、ログ記録 |
| `ConfigChange` | 設定が更新 | settingsが変更 | 設定変更に反応 |
| `CwdChanged` | 作業ディレクトリが変更 | ディレクトリ変更 | ディレクトリ固有のセットアップ |
| `FileChanged` | 監視ファイルが変更 | ファイル変更 | ファイル監視、再ビルド |
| `PreCompact` | compact操作前 | コンテキスト圧縮 | 状態保存 |
| `PostCompact` | compaction完了後 | Compaction完了 | compact後のアクション |
| `WorktreeCreate` | Worktreeが作成される | Git worktree作成 | Worktree環境のセットアップ |
| `WorktreeRemove` | Worktreeが削除される | Git worktree削除 | Worktreeリソースのクリーンアップ |
| `Elicitation` | MCPサーバーが入力を要求 | MCP elicitation | 入力バリデーション |
| `ElicitationResult` | ユーザーがelicitationに応答 | ユーザーが応答 | 応答処理 |
| `SessionEnd` | セッション終了 | セッション終了 | クリーンアップ、状態保存 |

### サンプルHooks

| Hook | 説明 | イベント | スコープ | インストール |
|------|-------------|-------|-------|--------------|
| `validate-bash.py` | コマンドバリデーション | PreToolUse:Bash | プロジェクト | `cp 06-hooks/validate-bash.py .claude/hooks/` |
| `security-scan.py` | セキュリティスキャン | PostToolUse:Write | プロジェクト | `cp 06-hooks/security-scan.py .claude/hooks/` |
| `format-code.sh` | 自動フォーマット | PostToolUse:Write | ユーザー | `cp 06-hooks/format-code.sh ~/.claude/hooks/` |
| `validate-prompt.py` | プロンプトバリデーション | UserPromptSubmit | プロジェクト | `cp 06-hooks/validate-prompt.py .claude/hooks/` |
| `context-tracker.py` | トークン使用量追跡 | Stop | ユーザー | `cp 06-hooks/context-tracker.py ~/.claude/hooks/` |
| `pre-commit.sh` | コミット前バリデーション | PreToolUse:Bash | プロジェクト | `cp 06-hooks/pre-commit.sh .claude/hooks/` |
| `log-bash.sh` | コマンドログ記録 | PostToolUse:Bash | ユーザー | `cp 06-hooks/log-bash.sh ~/.claude/hooks/` |

> **スコープ**: `プロジェクト` = チーム (`.claude/settings.json`)、`ユーザー` = 個人 (`~/.claude/settings.json`)

### Hook設定

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "command": "~/.claude/hooks/validate-bash.py"
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Write",
        "command": "~/.claude/hooks/format-code.sh"
      }
    ]
  }
}
```

**リファレンス**: [06-hooks/](06-hooks/) | [公式ドキュメント](https://code.claude.com/docs/en/hooks)

**クイックインストール (すべてのHooks)**:
```bash
mkdir -p ~/.claude/hooks && cp 06-hooks/*.sh ~/.claude/hooks/ && chmod +x ~/.claude/hooks/*.sh
```

---

## Memoryファイル

セッション間で自動的に読み込まれる永続的なコンテキスト。

### Memoryタイプ

| タイプ | 場所 | スコープ | 使う場面 |
|------|----------|-------|-------------|
| **管理ポリシー** | 組織管理ポリシー | 組織 | 組織全体の標準を強制 |
| **プロジェクト** | `./CLAUDE.md` | プロジェクト (チーム) | チーム標準、プロジェクトコンテキスト |
| **プロジェクトルール** | `.claude/rules/` | プロジェクト (チーム) | モジュール式プロジェクトルール |
| **ユーザー** | `~/.claude/CLAUDE.md` | ユーザー (個人) | 個人の設定 |
| **ユーザールール** | `~/.claude/rules/` | ユーザー (個人) | モジュール式個人ルール |
| **ローカル** | `./CLAUDE.local.md` | ローカル (git無視) | マシン固有のオーバーライド (2026年3月時点で公式ドキュメントには未記載; レガシーの可能性あり) |
| **Auto Memory** | 自動 | セッション | 自動キャプチャされた知見と修正 |

> **スコープ**: `組織` = 管理者管理、`プロジェクト` = gitでチーム共有、`ユーザー` = 個人設定、`ローカル` = コミットなし、`セッション` = 自動管理

**リファレンス**: [02-memory/](02-memory/) | [公式ドキュメント](https://code.claude.com/docs/en/memory)

**クイックインストール**:
```bash
cp 02-memory/project-CLAUDE.md ./CLAUDE.md
cp 02-memory/personal-CLAUDE.md ~/.claude/CLAUDE.md
```

---

## 新機能 (2026年3月)

| 機能 | 説明 | 使い方 |
|---------|-------------|------------|
| **Remote Control** | APIでClaude Codeセッションをリモート制御 | remote control APIを使ってプログラムでプロンプトを送信・応答を受信 |
| **Web Sessions** | ブラウザベース環境でClaude Codeを実行 | `claude web` またはAnthropic Consoleからアクセス |
| **Desktop App** | Claude Codeのネイティブデスクトップアプリ | `/desktop` またはAnthropic Webサイトからダウンロード |
| **Agent Teams** | 関連タスクで複数エージェントを調整 | チームメイトエージェントを設定してコンテキストを共有・協力 |
| **Task List** | バックグラウンドタスクの管理と監視 | `/tasks` でバックグラウンド操作を表示・管理 |
| **プロンプト提案** | コンテキスト対応のコマンド提案 | 現在のコンテキストに基づいて自動表示 |
| **Git Worktrees** | 並列開発のための独立したgit worktrees | 安全な並列ブランチ作業にworktreeコマンドを使用 |
| **サンドボックス化** | 安全のための独立した実行環境 | `/sandbox` で切り替え; 制限された環境でコマンドを実行 |
| **MCP OAuth** | MCPサーバーのOAuth認証 | セキュアなアクセスのためにMCPサーバー設定でOAuth認証情報を設定 |
| **MCPツール検索** | MCPツールを動的に検索・発見 | ツール検索を使って接続されたサーバー全体の利用可能なMCPツールを検索 |
| **スケジュールタスク** | `/loop` とcronツールで繰り返しタスクを設定 | `/loop 5m /command` またはCronCreateツールを使用 |
| **Chrome連携** | ヘッドレスChromiumによるブラウザ自動化 | `--chrome` フラグまたは `/chrome` コマンドを使用 |
| **キーボードカスタマイズ** | コード入力サポートを含むキーバインディングのカスタマイズ | `/keybindings` または `~/.claude/keybindings.json` を編集 |
| **Auto Mode** | パーミッションプロンプトなしの完全自律操作 (Research Preview) | `--mode auto` または `/permissions auto`; 2026年3月 |
| **Channels** | マルチチャンネルコミュニケーション (Telegram、Slackなど) (Research Preview) | チャンネルpluginsを設定; 2026年3月 |
| **Voice Dictation** | プロンプトの音声入力 | マイクアイコンまたは音声キーバインディングを使用 |
| **Agent Hookタイプ** | シェルコマンドの代わりにsubagentを起動するhooks | hook設定で `"type": "agent"` を設定 |
| **Prompt Hookタイプ** | 会話にプロンプトテキストを注入するhooks | hook設定で `"type": "prompt"` を設定 |
| **MCP Elicitation** | MCPサーバーがツール実行中にユーザー入力を要求できる | `Elicitation` と `ElicitationResult` hookイベントで処理 |
| **WebSocket MCPトランスポート** | MCPサーバー接続のWebSocketベーストランスポート | MCP サーバー設定で `"transport": "websocket"` を使用 |
| **Plugin LSPサポート** | pluginsを通じたLanguage Server Protocol連携 | エディタ機能のために `plugin.json` でLSPサーバーを設定 |
| **Managed Drop-ins** | 組織管理のドロップイン設定 (v2.1.83) | 管理者が管理ポリシー経由で設定; すべてのユーザーに自動適用 |

---

## クイックリファレンスマトリクス

### 機能選択ガイド

| ニーズ | 推奨機能 | 理由 |
|------|---------------------|-----|
| クイックショートカット | Slash Command | 手動、即時 |
| 永続的コンテキスト | Memory | 自動読み込み |
| 複雑な自動化 | Skill | 自動呼び出し |
| 専門的タスク | Subagent | 独立したコンテキスト |
| 外部データ | MCPサーバー | リアルタイムアクセス |
| イベント自動化 | Hook | イベントトリガー |
| 完全なソリューション | Plugin | オールインワンバンドル |

### インストール優先順位

| 優先度 | 機能 | コマンド |
|----------|---------|---------|
| 1. 必須 | Memory | `cp 02-memory/project-CLAUDE.md ./CLAUDE.md` |
| 2. 日常使用 | Slash Commands | `cp 01-slash-commands/*.md .claude/commands/` |
| 3. 品質 | Subagents | `cp 04-subagents/*.md .claude/agents/` |
| 4. 自動化 | Hooks | `cp 06-hooks/*.sh ~/.claude/hooks/ && chmod +x ~/.claude/hooks/*.sh` |
| 5. 外部連携 | MCP | `claude mcp add github -- npx -y @modelcontextprotocol/server-github` |
| 6. 高度 | Skills | `cp -r 03-skills/* ~/.claude/skills/` |
| 7. 完全 | Plugins | `/plugin install pr-review` |

---

## 一括インストールコマンド

このリポジトリのすべてのサンプルをインストール:

```bash
# ディレクトリを作成
mkdir -p .claude/{commands,agents,skills} ~/.claude/{hooks,skills}

# すべての機能をインストール
cp 01-slash-commands/*.md .claude/commands/ && \
cp 02-memory/project-CLAUDE.md ./CLAUDE.md && \
cp -r 03-skills/* ~/.claude/skills/ && \
cp 04-subagents/*.md .claude/agents/ && \
cp 06-hooks/*.sh ~/.claude/hooks/ && \
chmod +x ~/.claude/hooks/*.sh
```

---

## 追加リソース

- [公式 Claude Code ドキュメント](https://code.claude.com/docs/en/overview)
- [MCPプロトコル仕様](https://modelcontextprotocol.io)
- [学習ロードマップ](LEARNING-ROADMAP.md)
- [メイン README](README.md)

---

**最終更新**: 2026年3月
