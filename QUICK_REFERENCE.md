<picture>
  <source media="(prefers-color-scheme: dark)" srcset="resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="resources/logos/claude-howto-logo.svg">
</picture>

# Claude Code サンプル集 - クイックリファレンスカード

## 🚀 インストールクイックコマンド

### Slash Commands
```bash
# すべてインストール
cp 01-slash-commands/*.md .claude/commands/

# 特定のものをインストール
cp 01-slash-commands/optimize.md .claude/commands/
```

### Memory
```bash
# プロジェクトmemory
cp 02-memory/project-CLAUDE.md ./CLAUDE.md

# 個人memory
cp 02-memory/personal-CLAUDE.md ~/.claude/CLAUDE.md
```

### Skills
```bash
# 個人スキル
cp -r 03-skills/code-review ~/.claude/skills/

# プロジェクトスキル
cp -r 03-skills/code-review .claude/skills/
```

### Subagents
```bash
# すべてインストール
cp 04-subagents/*.md .claude/agents/

# 特定のものをインストール
cp 04-subagents/code-reviewer.md .claude/agents/
```

### MCP
```bash
# 認証情報を設定
export GITHUB_TOKEN="your_token"
export DATABASE_URL="postgresql://..."

# 設定をインストール (プロジェクトスコープ)
cp 05-mcp/github-mcp.json .mcp.json

# またはユーザースコープ: ~/.claude.json に追加
```

### Hooks
```bash
# hooksをインストール
mkdir -p ~/.claude/hooks
cp 06-hooks/*.sh ~/.claude/hooks/
chmod +x ~/.claude/hooks/*.sh

# settingsで設定 (~/.claude/settings.json)
```

### Plugins
```bash
# サンプルからインストール (公開済みの場合)
/plugin install pr-review
/plugin install devops-automation
/plugin install documentation
```

### Checkpoints
```bash
# Checkpointsはユーザーの各プロンプトで自動作成される
# 巻き戻すには Esc を2回押すか以下を使用:
/rewind

# 選択肢: コードと会話を復元、会話を復元、
# コードを復元、ここからまとめる、キャンセル
```

### Advanced Features
```bash
# settingsで設定 (.claude/settings.json)
# 09-advanced-features/config-examples.json を参照

# Planning mode
/plan タスクの説明

# パーミッションモード (--permission-mode フラグを使用)
# default        - リスクのある操作は承認を求める
# acceptEdits    - ファイル編集を自動承認、その他は確認
# plan           - 読み取り専用分析、変更なし
# dontAsk        - リスクの高いもの以外はすべての操作を承認
# auto           - バックグラウンド分類器が自動的にパーミッションを決定
# bypassPermissions - すべての操作を承認 (--dangerously-skip-permissions が必要)

# セッション管理
/resume                # 以前の会話を再開
/rename "名前"         # 現在のセッションに名前を付ける
/fork                  # 現在のセッションをフォーク
claude -c              # 最近の会話を続ける
claude -r "session"    # 名前/IDでセッションを再開
```

---

## 📋 機能チートシート

| 機能 | インストールパス | 使い方 |
|---------|-------------|-------|
| **Slash Commands (55以上)** | `.claude/commands/*.md` | `/コマンド名` |
| **Memory** | `./CLAUDE.md` | 自動読み込み |
| **Skills** | `.claude/skills/*/SKILL.md` | 自動呼び出し |
| **Subagents** | `.claude/agents/*.md` | 自動委任 |
| **MCP** | `.mcp.json` (プロジェクト) または `~/.claude.json` (ユーザー) | `/mcp__server__action` |
| **Hooks (25イベント)** | `~/.claude/hooks/*.sh` | イベントトリガー (4タイプ) |
| **Plugins** | `/plugin install` 経由 | すべてをバンドル |
| **Checkpoints** | 組み込み | `Esc+Esc` または `/rewind` |
| **Planning Mode** | 組み込み | `/plan <タスク>` |
| **Permission Modes (6種類)** | 組み込み | `--allowedTools`, `--permission-mode` |
| **Sessions** | 組み込み | `/session <コマンド>` |
| **Background Tasks** | 組み込み | バックグラウンドで実行 |
| **Remote Control** | 組み込み | WebSocket API |
| **Web Sessions** | 組み込み | `claude web` |
| **Git Worktrees** | 組み込み | `/worktree` |
| **Auto Memory** | 組み込み | CLAUDE.mdに自動保存 |
| **Task List** | 組み込み | `/task list` |
| **バンドルSkills (5種類)** | 組み込み | `/simplify`, `/loop`, `/claude-api`, `/voice`, `/browse` |

---

## 🎯 よくあるユースケース

### コードレビュー
```bash
# 方法1: Slash command
cp 01-slash-commands/optimize.md .claude/commands/
# 使い方: /optimize

# 方法2: Subagent
cp 04-subagents/code-reviewer.md .claude/agents/
# 使い方: 自動委任

# 方法3: Skill
cp -r 03-skills/code-review ~/.claude/skills/
# 使い方: 自動呼び出し

# 方法4: Plugin (ベスト)
/plugin install pr-review
# 使い方: /review-pr
```

### ドキュメント生成
```bash
# Slash command
cp 01-slash-commands/generate-api-docs.md .claude/commands/

# Subagent
cp 04-subagents/documentation-writer.md .claude/agents/

# Skill
cp -r 03-skills/doc-generator ~/.claude/skills/

# Plugin (完全なソリューション)
/plugin install documentation
```

### DevOps
```bash
# 完全なplugin
/plugin install devops-automation

# コマンド: /deploy, /rollback, /status, /incident
```

### チーム標準
```bash
# プロジェクトmemory
cp 02-memory/project-CLAUDE.md ./CLAUDE.md

# チーム用に編集
vim CLAUDE.md
```

### 自動化とHooks
```bash
# hooksをインストール (25イベント、4タイプ: command, http, prompt, agent)
mkdir -p ~/.claude/hooks
cp 06-hooks/*.sh ~/.claude/hooks/
chmod +x ~/.claude/hooks/*.sh

# 例:
# - コミット前テスト: pre-commit.sh
# - コードの自動フォーマット: format-code.sh
# - セキュリティスキャン: security-scan.sh

# 完全自律ワークフローのAuto Mode
claude --enable-auto-mode -p "authモジュールをリファクタリングしてテストして"
# またはShift+Tabでモードを対話的に切り替え
```

### 安全なリファクタリング
```bash
# Checkpointは各プロンプトの前に自動作成される
# リファクタリングを試みる
# 成功: 続ける
# 失敗: Esc+Esc を押すか /rewind で戻る
```

### 複雑な実装
```bash
# Planning modeを使用
/plan ユーザー認証システムの実装

# Claudeが詳細な計画を作成
# レビューして承認
# Claudeが体系的に実装
```

### CI/CD統合
```bash
# ヘッドレスモードで実行 (非インタラクティブ)
claude -p "すべてのテストを実行してレポートを生成して"

# CI用パーミッションモード付き
claude -p "テストを実行して" --permission-mode dontAsk

# 完全自律CI タスクのAuto Mode付き
claude --enable-auto-mode -p "テストを実行して失敗を修正して"

# 自動化用hooksと組み合わせ
# 09-advanced-features/README.md を参照
```

### 学習と実験
```bash
# 安全な分析のためにplan modeを使用
claude --permission-mode plan

# 安全に実験 — checkpointは自動作成される
# 巻き戻す必要があれば: Esc+Esc を押すか /rewind を使用
```

### Agent Teams
```bash
# agent teamsを有効化
export CLAUDE_AGENT_TEAMS=1

# またはsettings.jsonで
{ "agentTeams": { "enabled": true } }

# 開始: "チームアプローチで機能Xを実装して"
```

### スケジュールタスク
```bash
# 5分ごとにコマンドを実行
/loop 5m /check-status

# 1回限りのリマインダー
/loop 30m "デプロイを確認するように思い出させて"
```

---

## 📁 ファイル場所リファレンス

```
プロジェクト/
├── .claude/
│   ├── commands/              # Slash commandsはここに
│   ├── agents/                # Subagentsはここに
│   ├── skills/                # プロジェクトskillsはここに
│   └── settings.json          # プロジェクト設定 (hooks等)
├── .mcp.json                  # MCP設定 (プロジェクトスコープ)
├── CLAUDE.md                  # プロジェクトmemory
└── src/
    └── api/
        └── CLAUDE.md          # ディレクトリ固有のmemory

ユーザーホーム/
├── .claude/
│   ├── commands/              # 個人コマンド
│   ├── agents/                # 個人エージェント
│   ├── skills/                # 個人スキル
│   ├── hooks/                 # Hookスクリプト
│   ├── settings.json          # ユーザー設定
│   ├── managed-settings.d/    # 管理設定 (エンタープライズ/組織)
│   └── CLAUDE.md              # 個人memory
└── .claude.json               # 個人MCP設定 (ユーザースコープ)
```

---

## 🔍 サンプルを探す

### カテゴリ別
- **Slash Commands**: `01-slash-commands/`
- **Memory**: `02-memory/`
- **Skills**: `03-skills/`
- **Subagents**: `04-subagents/`
- **MCP**: `05-mcp/`
- **Hooks**: `06-hooks/`
- **Plugins**: `07-plugins/`
- **Checkpoints**: `08-checkpoints/`
- **Advanced Features**: `09-advanced-features/`
- **CLI**: `10-cli/`

### ユースケース別
- **パフォーマンス**: `01-slash-commands/optimize.md`
- **セキュリティ**: `04-subagents/secure-reviewer.md`
- **テスト**: `04-subagents/test-engineer.md`
- **ドキュメント**: `03-skills/doc-generator/`
- **DevOps**: `07-plugins/devops-automation/`

### 複雑さ別
- **シンプル**: Slash commands
- **中程度**: Subagents、Memory
- **上級**: Skills、Hooks
- **完全**: Plugins

---

## 🎓 学習パス

### 1日目
```bash
# 概要を読む
cat README.md

# コマンドをインストール
cp 01-slash-commands/optimize.md .claude/commands/

# 試してみる
/optimize
```

### 2〜3日目
```bash
# memoryをセットアップ
cp 02-memory/project-CLAUDE.md ./CLAUDE.md
vim CLAUDE.md

# subagentをインストール
cp 04-subagents/code-reviewer.md .claude/agents/
```

### 4〜5日目
```bash
# MCPをセットアップ
export GITHUB_TOKEN="your_token"
cp 05-mcp/github-mcp.json .mcp.json

# MCPコマンドを試す
/mcp__github__list_prs
```

### 2週目
```bash
# skillをインストール
cp -r 03-skills/code-review ~/.claude/skills/

# 自動呼び出しさせる
# 単に: "このコードの問題点をレビューして"
```

### 3週目以降
```bash
# 完全なpluginをインストール
/plugin install pr-review

# バンドル機能を使う
/review-pr
/check-security
/check-tests
```

---

## 新機能 (2026年3月)

| 機能 | 説明 | 使い方 |
|---------|-------------|-------|
| **Auto Mode** | バックグラウンド分類器による完全自律操作 | `--enable-auto-mode` フラグ、`Shift+Tab` でモード切り替え |
| **Channels** | DiscordとTelegram連携 | `--channels` フラグ、Discord/Telegram bot |
| **Voice Dictation** | Claudeにコマンドとコンテキストを音声入力 | `/voice` コマンド |
| **Hooks (25イベント)** | 4タイプに拡張されたhookシステム | command, http, prompt, agent hookタイプ |
| **MCP Elicitation** | MCPサーバーが実行時にユーザー入力を要求できる | サーバーが明確化を必要とする際に自動プロンプト |
| **WebSocket MCP** | MCP接続のWebSocketトランスポート | `.mcp.json` に `ws://` URLで設定 |
| **Plugin LSP** | pluginsのLanguage Server Protocolサポート | `userConfig`、`${CLAUDE_PLUGIN_DATA}` 変数 |
| **Remote Control** | WebSocket APIでClaude Codeを制御 | 外部統合のために `claude --remote` |
| **Web Sessions** | ブラウザベースのClaude Codeインターフェース | `claude web` で起動 |
| **Desktop App** | ネイティブデスクトップアプリケーション | claude.ai/download からダウンロード |
| **Task List** | バックグラウンドタスクを管理 | `/task list`、`/task status <id>` |
| **Auto Memory** | 会話から自動的にmemoryを保存 | ClaudeがCLAUDE.mdに重要なコンテキストを自動保存 |
| **Git Worktrees** | 並列開発のための独立したワークスペース | `/worktree` で独立したワークスペースを作成 |
| **Model Selection** | Sonnet 4.6とOpus 4.6を切り替え | `/model` または `--model` フラグ |
| **Agent Teams** | 複数エージェントをタスクで調整 | `CLAUDE_AGENT_TEAMS=1` 環境変数で有効化 |
| **Scheduled Tasks** | `/loop` による繰り返しタスク | `/loop 5m /command` または CronCreateツール |
| **Chrome Integration** | ブラウザ自動化 | `--chrome` フラグまたは `/chrome` コマンド |
| **Keyboard Customization** | カスタムキーバインディング | `/keybindings` コマンド |

---

## ヒントとコツ

### カスタマイズ
- まずサンプルをそのまま使う
- 自分のニーズに合わせて変更する
- チームと共有する前にテストする
- 設定をバージョン管理する

### ベストプラクティス
- チーム標準にmemoryを使う
- 完全なワークフローにpluginsを使う
- 複雑なタスクにsubagentsを使う
- クイックタスクにslash commandsを使う

### トラブルシューティング
```bash
# ファイルの場所を確認
ls -la .claude/commands/
ls -la .claude/agents/

# YAML構文を確認
head -20 .claude/agents/code-reviewer.md

# MCP接続をテスト
echo $GITHUB_TOKEN
```

---

## 📊 機能マトリクス

| ニーズ | 使うもの | サンプル |
|------|----------|---------|
| クイックショートカット | Slash Command (55以上) | `01-slash-commands/optimize.md` |
| チーム標準 | Memory | `02-memory/project-CLAUDE.md` |
| 自動ワークフロー | Skill | `03-skills/code-review/` |
| 専門的なタスク | Subagent | `04-subagents/code-reviewer.md` |
| 外部データ | MCP (+ Elicitation、WebSocket) | `05-mcp/github-mcp.json` |
| イベント自動化 | Hook (25イベント、4タイプ) | `06-hooks/pre-commit.sh` |
| 完全なソリューション | Plugin (+ LSPサポート) | `07-plugins/pr-review/` |
| 安全な実験 | Checkpoint | `08-checkpoints/checkpoint-examples.md` |
| 完全自律 | Auto Mode | `--enable-auto-mode` または `Shift+Tab` |
| チャット統合 | Channels | `--channels` (Discord、Telegram) |
| CI/CDパイプライン | CLI | `10-cli/README.md` |

---

## 🔗 クイックリンク

- **メインガイド**: `README.md`
- **完全インデックス**: `INDEX.md`
- **サマリー**: `EXAMPLES_SUMMARY.md`
- **オリジナルガイド**: `claude_concepts_guide.md`

---

## 📞 よくある質問

**Q: どれを使えばいい？**
A: slash commandsから始めて、必要に応じて機能を追加する。

**Q: 機能を組み合わせられる？**
A: はい！一緒に使えます。Memory + Commands + MCP = 強力。

**Q: チームと共有するには？**
A: `.claude/` ディレクトリをgitにコミットする。

**Q: シークレットは？**
A: 環境変数を使う、絶対にハードコードしない。

**Q: サンプルを変更できる？**
A: もちろん！カスタマイズするためのテンプレートです。

---

## ✅ チェックリスト

はじめにやることチェックリスト:

- [ ] `README.md` を読む
- [ ] slash commandを1つインストール
- [ ] コマンドを試す
- [ ] プロジェクト用 `CLAUDE.md` を作成
- [ ] subagentを1つインストール
- [ ] MCP統合を1つセットアップ
- [ ] skillを1つインストール
- [ ] 完全なpluginを試す
- [ ] 自分のニーズにカスタマイズ
- [ ] チームと共有

---

**クイックスタート**: `cat README.md`

**完全インデックス**: `cat INDEX.md`

**このカード**: クイックリファレンスとして手元に置いておこう！
