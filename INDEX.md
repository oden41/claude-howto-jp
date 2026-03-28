<picture>
  <source media="(prefers-color-scheme: dark)" srcset="resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="resources/logos/claude-howto-logo.svg">
</picture>

# Claude Code サンプル集 - 完全インデックス

このドキュメントは機能タイプ別に整理されたすべてのサンプルファイルの完全インデックスです。

## サマリー統計

- **総ファイル数**: 100以上のファイル
- **カテゴリ**: 10の機能カテゴリ
- **Plugins**: 3つの完全なplugin
- **Skills**: 6つの完全なskill
- **Hooks**: 8つのサンプルhook
- **すぐに使えるもの**: すべてのサンプル

---

## 01. Slash Commands (10ファイル)

一般的なワークフロー用のユーザー呼び出しショートカット。

| ファイル | 説明 | ユースケース |
|------|-------------|----------|
| `optimize.md` | コード最適化分析 | パフォーマンス問題の発見 |
| `pr.md` | プルリクエスト準備 | PRワークフローの自動化 |
| `generate-api-docs.md` | APIドキュメント生成 | APIドキュメントの生成 |
| `commit.md` | コミットメッセージヘルパー | 標準化されたコミット |
| `setup-ci-cd.md` | CI/CDパイプラインのセットアップ | DevOps自動化 |
| `push-all.md` | すべての変更をプッシュ | クイックプッシュワークフロー |
| `unit-test-expand.md` | ユニットテストカバレッジを拡張 | テスト自動化 |
| `doc-refactor.md` | ドキュメントのリファクタリング | ドキュメント改善 |
| `pr-slash-command.png` | スクリーンショットサンプル | ビジュアルリファレンス |
| `README.md` | ドキュメント | セットアップと使用ガイド |

**インストールパス**: `.claude/commands/`

**使い方**: `/optimize`, `/pr`, `/generate-api-docs`, `/commit`, `/setup-ci-cd`, `/push-all`, `/unit-test-expand`, `/doc-refactor`

---

## 02. Memory (6ファイル)

永続的なコンテキストとプロジェクト標準。

| ファイル | 説明 | スコープ | 場所 |
|------|-------------|-------|----------|
| `project-CLAUDE.md` | チームプロジェクト標準 | プロジェクト全体 | `./CLAUDE.md` |
| `directory-api-CLAUDE.md` | API固有のルール | ディレクトリ | `./src/api/CLAUDE.md` |
| `personal-CLAUDE.md` | 個人の設定 | ユーザー | `~/.claude/CLAUDE.md` |
| `memory-saved.png` | スクリーンショット: memory保存 | - | ビジュアルリファレンス |
| `memory-ask-claude.png` | スクリーンショット: Claudeに聞く | - | ビジュアルリファレンス |
| `README.md` | ドキュメント | - | リファレンス |

**インストール**: 適切な場所にコピー

**使い方**: Claudeによって自動的に読み込まれる

---

## 03. Skills (28ファイル)

スクリプトとテンプレートを持つ自動呼び出し機能。

### コードレビューSkill (5ファイル)
```
code-review/
├── SKILL.md                          # Skill定義
├── scripts/
│   ├── analyze-metrics.py            # コードメトリクス分析
│   └── compare-complexity.py         # 複雑度比較
└── templates/
    ├── review-checklist.md           # レビューチェックリスト
    └── finding-template.md           # 発見事項ドキュメント
```

**目的**: セキュリティ・パフォーマンス・品質分析を含む包括的なコードレビュー

**自動呼び出し**: コードレビュー時

---

### ブランドボイスSkill (4ファイル)
```
brand-voice/
├── SKILL.md                          # Skill定義
├── templates/
│   ├── email-template.txt            # メール形式
│   └── social-post-template.txt      # ソーシャルメディア形式
└── tone-examples.md                  # メッセージの例
```

**目的**: コミュニケーションにおける一貫したブランドボイスを確保

**自動呼び出し**: マーケティングコピー作成時

---

### ドキュメント生成Skill (2ファイル)
```
doc-generator/
├── SKILL.md                          # Skill定義
└── generate-docs.py                  # Python ドキュメント抽出ツール
```

**目的**: ソースコードから包括的なAPIドキュメントを生成

**自動呼び出し**: APIドキュメントの作成/更新時

---

### リファクタリングSkill (5ファイル)
```
refactor/
├── SKILL.md                          # Skill定義
├── scripts/
│   ├── analyze-complexity.py         # 複雑度分析
│   └── detect-smells.py              # コードスメル検出
├── references/
│   ├── code-smells.md                # コードスメルカタログ
│   └── refactoring-catalog.md        # リファクタリングパターン
└── templates/
    └── refactoring-plan.md           # リファクタリング計画テンプレート
```

**目的**: 複雑度分析を使った体系的なコードリファクタリング

**自動呼び出し**: コードリファクタリング時

---

### Claude MD Skill (1ファイル)
```
claude-md/
└── SKILL.md                          # Skill定義
```

**目的**: CLAUDE.mdファイルの管理と最適化

---

### ブログ下書きSkill (3ファイル)
```
blog-draft/
├── SKILL.md                          # Skill定義
└── templates/
    ├── draft-template.md             # ブログ下書きテンプレート
    └── outline-template.md           # ブログアウトラインテンプレート
```

**目的**: 一貫した構造でブログ記事を下書き

**その他**: `README.md` - Skillsの概要と使用ガイド

**インストールパス**: `~/.claude/skills/` または `.claude/skills/`

---

## 04. Subagents (9ファイル)

カスタム機能を持つ専門AIアシスタント。

| ファイル | 説明 | ツール | ユースケース |
|------|-------------|-------|----------|
| `code-reviewer.md` | コード品質分析 | read, grep, diff, lint_runner | 包括的レビュー |
| `test-engineer.md` | テストカバレッジ分析 | read, write, bash, grep | テスト自動化 |
| `documentation-writer.md` | ドキュメント作成 | read, write, grep | ドキュメント生成 |
| `secure-reviewer.md` | セキュリティレビュー (read-only) | read, grep | セキュリティ監査 |
| `implementation-agent.md` | フル実装 | read, write, bash, grep, edit, glob | 機能開発 |
| `debugger.md` | デバッグ専門家 | read, bash, grep | バグ調査 |
| `data-scientist.md` | データ分析専門家 | read, write, bash | データワークフロー |
| `clean-code-reviewer.md` | Clean Code標準 | read, grep | コード品質 |
| `README.md` | ドキュメント | - | セットアップと使用ガイド |

**インストールパス**: `.claude/agents/`

**使い方**: メインエージェントによって自動的に委任される

---

## 05. MCPプロトコル (5ファイル)

外部ツールとAPI連携。

| ファイル | 説明 | 連携先 | ユースケース |
|------|-------------|-----------------|----------|
| `github-mcp.json` | GitHub連携 | GitHub API | PR/issue管理 |
| `database-mcp.json` | データベースクエリ | PostgreSQL/MySQL | ライブデータクエリ |
| `filesystem-mcp.json` | ファイル操作 | ローカルファイルシステム | ファイル管理 |
| `multi-mcp.json` | 複数サーバー | GitHub + DB + Slack | 完全統合 |
| `README.md` | ドキュメント | - | セットアップと使用ガイド |

**インストールパス**: `.mcp.json` (プロジェクトスコープ) または `~/.claude.json` (ユーザースコープ)

**使い方**: `/mcp__github__list_prs` など

---

## 06. Hooks (9ファイル)

自動実行されるイベント駆動自動化スクリプト。

| ファイル | 説明 | イベント | ユースケース |
|------|-------------|-------|----------|
| `format-code.sh` | コードの自動フォーマット | PreToolUse:Write | コードフォーマット |
| `pre-commit.sh` | コミット前にテストを実行 | PreToolUse:Bash | テスト自動化 |
| `security-scan.sh` | セキュリティスキャン | PostToolUse:Write | セキュリティチェック |
| `log-bash.sh` | bashコマンドをログ | PostToolUse:Bash | コマンドログ記録 |
| `validate-prompt.sh` | プロンプトを検証 | PreToolUse | 入力バリデーション |
| `notify-team.sh` | 通知を送信 | Notification | チーム通知 |
| `context-tracker.py` | コンテキストウィンドウ使用量を追跡 | PostToolUse | コンテキスト監視 |
| `context-tracker-tiktoken.py` | トークンベースのコンテキスト追跡 | PostToolUse | 正確なトークンカウント |
| `README.md` | ドキュメント | - | セットアップと使用ガイド |

**インストールパス**: `~/.claude/settings.json` で設定

**使い方**: settingsで設定され、自動的に実行される

**Hookタイプ** (4タイプ、25イベント):
- Tool Hooks: PreToolUse, PostToolUse, PostToolUseFailure, PermissionRequest
- Session Hooks: SessionStart, SessionEnd, Stop, StopFailure, SubagentStart, SubagentStop
- Task Hooks: UserPromptSubmit, TaskCompleted, TaskCreated, TeammateIdle
- Lifecycle Hooks: ConfigChange, CwdChanged, FileChanged, PreCompact, PostCompact, WorktreeCreate, WorktreeRemove, Notification, InstructionsLoaded, Elicitation, ElicitationResult

---

## 07. Plugins (3つの完全なplugin、40ファイル)

機能のバンドルコレクション。

### PRレビューPlugin (10ファイル)
```
pr-review/
├── .claude-plugin/
│   └── plugin.json                   # Pluginマニフェスト
├── commands/
│   ├── review-pr.md                  # 包括的レビュー
│   ├── check-security.md             # セキュリティチェック
│   └── check-tests.md                # テストカバレッジチェック
├── agents/
│   ├── security-reviewer.md          # セキュリティ専門家
│   ├── test-checker.md               # テスト専門家
│   └── performance-analyzer.md       # パフォーマンス専門家
├── mcp/
│   └── github-config.json            # GitHub連携
├── hooks/
│   └── pre-review.js                 # レビュー前バリデーション
└── README.md                         # Pluginドキュメント
```

**機能**: セキュリティ分析、テストカバレッジ、パフォーマンス影響

**コマンド**: `/review-pr`, `/check-security`, `/check-tests`

**インストール**: `/plugin install pr-review`

---

### DevOps自動化Plugin (15ファイル)
```
devops-automation/
├── .claude-plugin/
│   └── plugin.json                   # Pluginマニフェスト
├── commands/
│   ├── deploy.md                     # デプロイ
│   ├── rollback.md                   # ロールバック
│   ├── status.md                     # システム状態
│   └── incident.md                   # インシデント対応
├── agents/
│   ├── deployment-specialist.md      # デプロイ専門家
│   ├── incident-commander.md         # インシデント調整役
│   └── alert-analyzer.md             # アラート分析
├── mcp/
│   └── kubernetes-config.json        # Kubernetes連携
├── hooks/
│   ├── pre-deploy.js                 # デプロイ前チェック
│   └── post-deploy.js                # デプロイ後タスク
├── scripts/
│   ├── deploy.sh                     # デプロイ自動化
│   ├── rollback.sh                   # ロールバック自動化
│   └── health-check.sh               # ヘルスチェック
└── README.md                         # Pluginドキュメント
```

**機能**: Kubernetesデプロイ、ロールバック、モニタリング、インシデント対応

**コマンド**: `/deploy`, `/rollback`, `/status`, `/incident`

**インストール**: `/plugin install devops-automation`

---

### ドキュメントPlugin (14ファイル)
```
documentation/
├── .claude-plugin/
│   └── plugin.json                   # Pluginマニフェスト
├── commands/
│   ├── generate-api-docs.md          # APIドキュメント生成
│   ├── generate-readme.md            # README作成
│   ├── sync-docs.md                  # ドキュメント同期
│   └── validate-docs.md              # ドキュメント検証
├── agents/
│   ├── api-documenter.md             # APIドキュメント専門家
│   ├── code-commentator.md           # コードコメント専門家
│   └── example-generator.md          # サンプル作成
├── mcp/
│   └── github-docs-config.json       # GitHub連携
├── templates/
│   ├── api-endpoint.md               # APIエンドポイントテンプレート
│   ├── function-docs.md              # 関数ドキュメントテンプレート
│   └── adr-template.md               # ADRテンプレート
└── README.md                         # Pluginドキュメント
```

**機能**: APIドキュメント、README生成、ドキュメント同期、検証

**コマンド**: `/generate-api-docs`, `/generate-readme`, `/sync-docs`, `/validate-docs`

**インストール**: `/plugin install documentation`

**その他**: `README.md` - Pluginsの概要と使用ガイド

---

## 08. CheckpointsとRewind (2ファイル)

会話の状態を保存して代替アプローチを探る。

| ファイル | 説明 | 内容 |
|------|-------------|---------|
| `README.md` | ドキュメント | 包括的なcheckpointガイド |
| `checkpoint-examples.md` | 実際のサンプル | データベース移行、パフォーマンス最適化、UIイテレーション、デバッグ |

**主要概念**:
- **Checkpoint**: 会話状態のスナップショット
- **Rewind**: 以前のcheckpointに戻る
- **Branch Point**: 複数のアプローチを探る

**使い方**:
```
# Checkpointはユーザーの各プロンプトで自動作成される
# 巻き戻すには Esc を2回押すか以下を使用:
/rewind
# 選択: コードと会話を復元、会話を復元、
# コードを復元、ここからまとめる、キャンセル
```

**ユースケース**:
- 異なる実装を試す
- ミスからの回復
- 安全な実験
- ソリューションの比較
- A/Bテスト

---

## 09. Advanced Features (3ファイル)

複雑なワークフローのための高度な機能。

| ファイル | 説明 | 機能 |
|------|-------------|----------|
| `README.md` | 完全ガイド | すべての高度な機能のドキュメント |
| `config-examples.json` | 設定サンプル | 10以上のユースケース固有設定 |
| `planning-mode-examples.md` | Planning modeサンプル | REST API、データベース移行、リファクタリング |
| スケジュールタスク | `/loop` とcronツールで繰り返しタスク | 自動化された繰り返しワークフロー |
| Chrome連携 | ヘッドレスChromiumによるブラウザ自動化 | Webテストとスクレイピング |
| Remote Control (拡張) | 接続方法、セキュリティ、比較表 | リモートセッション管理 |
| キーボードカスタマイズ | カスタムキーバインディング、コード入力サポート、コンテキスト | パーソナライズされたショートカット |
| Desktop App (拡張) | コネクタ、launch.json、エンタープライズ機能 | デスクトップ連携 |

**カバーされている高度な機能**:

### Planning Mode
- 詳細な実装計画を作成
- 時間見積もりとリスク評価
- 体系的なタスク分解

### Extended Thinking
- 複雑な問題への深い推論
- アーキテクチャ決定の分析
- トレードオフの評価

### Background Tasks
- ブロックせずに長時間処理を実行
- 並列開発ワークフロー
- タスク管理と監視

### パーミッションモード
- **default**: リスクのある操作に承認を求める
- **acceptEdits**: ファイル編集を自動承認、その他はプロンプト
- **plan**: 読み取り専用分析、変更なし
- **auto**: 安全なアクションを自動承認、リスクのあるものはプロンプト
- **dontAsk**: リスクの高いもの以外はすべての操作を承認
- **bypassPermissions**: すべてを承認 (`--dangerously-skip-permissions` が必要)

### ヘッドレスモード (`claude -p`)
- CI/CD統合
- 自動タスク実行
- バッチ処理

### セッション管理
- 複数の作業セッション
- セッションの切り替えと保存
- セッション永続化

### インタラクティブ機能
- キーボードショートカット
- コマンド履歴
- タブ補完
- マルチライン入力

### 設定
- 包括的な設定管理
- 環境固有の設定
- プロジェクトごとのカスタマイズ

### スケジュールタスク
- `/loop` コマンドによる繰り返しタスク
- Cronツール: CronCreate、CronList、CronDelete
- 自動化された繰り返しワークフロー

### Chrome連携
- ヘッドレスChromiumによるブラウザ自動化
- Webテストとスクレイピング機能
- ページ操作とデータ抽出

### Remote Control (拡張)
- 接続方法とプロトコル
- セキュリティの考慮事項とベストプラクティス
- リモートアクセスオプションの比較表

### キーボードカスタマイズ
- カスタムキーバインディング設定
- マルチキーショートカットのコード入力サポート
- コンテキスト対応キーバインディングの有効化

### Desktop App (拡張)
- IDE連携のコネクタ
- launch.json設定
- エンタープライズ機能とデプロイ

---

## 10. CLI使用法 (1ファイル)

コマンドラインインターフェースの使用パターンとリファレンス。

| ファイル | 説明 | 内容 |
|------|-------------|---------|
| `README.md` | CLIドキュメント | フラグ、オプション、使用パターン |

**主要CLI機能**:
- `claude` - インタラクティブセッションを開始
- `claude -p "prompt"` - ヘッドレス/非インタラクティブモード
- `claude web` - Webセッションを起動
- `claude --model` - モデルを選択 (Sonnet 4.6、Opus 4.6)
- `claude --permission-mode` - パーミッションモードを設定
- `claude --remote` - WebSocket経由のリモートコントロールを有効化

---

## ドキュメントファイル (13ファイル)

| ファイル | 場所 | 説明 |
|------|----------|-------------|
| `README.md` | `/` | メインサンプル概要 |
| `INDEX.md` | `/` | この完全インデックス |
| `QUICK_REFERENCE.md` | `/` | クイックリファレンスカード |
| `README.md` | `/01-slash-commands/` | Slash commandsガイド |
| `README.md` | `/02-memory/` | Memoryガイド |
| `README.md` | `/03-skills/` | Skillsガイド |
| `README.md` | `/04-subagents/` | Subagentsガイド |
| `README.md` | `/05-mcp/` | MCPガイド |
| `README.md` | `/06-hooks/` | Hooksガイド |
| `README.md` | `/07-plugins/` | Pluginsガイド |
| `README.md` | `/08-checkpoints/` | Checkpointsガイド |
| `README.md` | `/09-advanced-features/` | Advanced featuresガイド |
| `README.md` | `/10-cli/` | CLIガイド |

---

## 完全ファイルツリー

```
claude-howto/
├── README.md                                    # メイン概要
├── INDEX.md                                     # このファイル
├── QUICK_REFERENCE.md                           # クイックリファレンスカード
├── claude_concepts_guide.md                     # オリジナルガイド
│
├── 01-slash-commands/                           # Slash Commands
│   ├── optimize.md
│   ├── pr.md
│   ├── generate-api-docs.md
│   ├── commit.md
│   ├── setup-ci-cd.md
│   ├── push-all.md
│   ├── unit-test-expand.md
│   ├── doc-refactor.md
│   ├── pr-slash-command.png
│   └── README.md
│
├── 02-memory/                                   # Memory
│   ├── project-CLAUDE.md
│   ├── directory-api-CLAUDE.md
│   ├── personal-CLAUDE.md
│   ├── memory-saved.png
│   ├── memory-ask-claude.png
│   └── README.md
│
├── 03-skills/                                   # Skills
│   ├── code-review/
│   │   ├── SKILL.md
│   │   ├── scripts/
│   │   │   ├── analyze-metrics.py
│   │   │   └── compare-complexity.py
│   │   └── templates/
│   │       ├── review-checklist.md
│   │       └── finding-template.md
│   ├── brand-voice/
│   │   ├── SKILL.md
│   │   ├── templates/
│   │   │   ├── email-template.txt
│   │   │   └── social-post-template.txt
│   │   └── tone-examples.md
│   ├── doc-generator/
│   │   ├── SKILL.md
│   │   └── generate-docs.py
│   ├── refactor/
│   │   ├── SKILL.md
│   │   ├── scripts/
│   │   │   ├── analyze-complexity.py
│   │   │   └── detect-smells.py
│   │   ├── references/
│   │   │   ├── code-smells.md
│   │   │   └── refactoring-catalog.md
│   │   └── templates/
│   │       └── refactoring-plan.md
│   ├── claude-md/
│   │   └── SKILL.md
│   ├── blog-draft/
│   │   ├── SKILL.md
│   │   └── templates/
│   │       ├── draft-template.md
│   │       └── outline-template.md
│   └── README.md
│
├── 04-subagents/                                # Subagents
│   ├── code-reviewer.md
│   ├── test-engineer.md
│   ├── documentation-writer.md
│   ├── secure-reviewer.md
│   ├── implementation-agent.md
│   ├── debugger.md
│   ├── data-scientist.md
│   ├── clean-code-reviewer.md
│   └── README.md
│
├── 05-mcp/                                      # MCPプロトコル
│   ├── github-mcp.json
│   ├── database-mcp.json
│   ├── filesystem-mcp.json
│   ├── multi-mcp.json
│   └── README.md
│
├── 06-hooks/                                    # Hooks
│   ├── format-code.sh
│   ├── pre-commit.sh
│   ├── security-scan.sh
│   ├── log-bash.sh
│   ├── validate-prompt.sh
│   ├── notify-team.sh
│   ├── context-tracker.py
│   ├── context-tracker-tiktoken.py
│   └── README.md
│
├── 07-plugins/                                  # Plugins
│   ├── pr-review/
│   │   ├── .claude-plugin/
│   │   │   └── plugin.json
│   │   ├── commands/
│   │   │   ├── review-pr.md
│   │   │   ├── check-security.md
│   │   │   └── check-tests.md
│   │   ├── agents/
│   │   │   ├── security-reviewer.md
│   │   │   ├── test-checker.md
│   │   │   └── performance-analyzer.md
│   │   ├── mcp/
│   │   │   └── github-config.json
│   │   ├── hooks/
│   │   │   └── pre-review.js
│   │   └── README.md
│   ├── devops-automation/
│   │   ├── .claude-plugin/
│   │   │   └── plugin.json
│   │   ├── commands/
│   │   │   ├── deploy.md
│   │   │   ├── rollback.md
│   │   │   ├── status.md
│   │   │   └── incident.md
│   │   ├── agents/
│   │   │   ├── deployment-specialist.md
│   │   │   ├── incident-commander.md
│   │   │   └── alert-analyzer.md
│   │   ├── mcp/
│   │   │   └── kubernetes-config.json
│   │   ├── hooks/
│   │   │   ├── pre-deploy.js
│   │   │   └── post-deploy.js
│   │   ├── scripts/
│   │   │   ├── deploy.sh
│   │   │   ├── rollback.sh
│   │   │   └── health-check.sh
│   │   └── README.md
│   ├── documentation/
│   │   ├── .claude-plugin/
│   │   │   └── plugin.json
│   │   ├── commands/
│   │   │   ├── generate-api-docs.md
│   │   │   ├── generate-readme.md
│   │   │   ├── sync-docs.md
│   │   │   └── validate-docs.md
│   │   ├── agents/
│   │   │   ├── api-documenter.md
│   │   │   ├── code-commentator.md
│   │   │   └── example-generator.md
│   │   ├── mcp/
│   │   │   └── github-docs-config.json
│   │   ├── templates/
│   │   │   ├── api-endpoint.md
│   │   │   ├── function-docs.md
│   │   │   └── adr-template.md
│   │   └── README.md
│   └── README.md
│
├── 08-checkpoints/                              # Checkpoints
│   ├── checkpoint-examples.md
│   └── README.md
│
├── 09-advanced-features/                        # Advanced Features
│   ├── config-examples.json
│   ├── planning-mode-examples.md
│   └── README.md
│
└── 10-cli/                                      # CLI使用法
    └── README.md
```

---

## ユースケース別クイックスタート

### コード品質とレビュー
```bash
# slash commandをインストール
cp 01-slash-commands/optimize.md .claude/commands/

# subagentをインストール
cp 04-subagents/code-reviewer.md .claude/agents/

# skillをインストール
cp -r 03-skills/code-review ~/.claude/skills/

# または完全なpluginをインストール
/plugin install pr-review
```

### DevOpsとデプロイ
```bash
# pluginをインストール (すべてを含む)
/plugin install devops-automation
```

### ドキュメント
```bash
# slash commandをインストール
cp 01-slash-commands/generate-api-docs.md .claude/commands/

# subagentをインストール
cp 04-subagents/documentation-writer.md .claude/agents/

# skillをインストール
cp -r 03-skills/doc-generator ~/.claude/skills/

# または完全なpluginをインストール
/plugin install documentation
```

### チーム標準
```bash
# プロジェクトmemoryをセットアップ
cp 02-memory/project-CLAUDE.md ./CLAUDE.md

# チームの標準に合わせて編集
```

### 外部連携
```bash
# 環境変数を設定
export GITHUB_TOKEN="your_token"
export DATABASE_URL="postgresql://..."

# MCP設定をインストール (プロジェクトスコープ)
cp 05-mcp/multi-mcp.json .mcp.json
```

### 自動化とバリデーション
```bash
# hooksをインストール
mkdir -p ~/.claude/hooks
cp 06-hooks/*.sh ~/.claude/hooks/
chmod +x ~/.claude/hooks/*.sh

# settingsでhooksを設定 (~/.claude/settings.json)
# 06-hooks/README.md を参照
```

### 安全な実験
```bash
# Checkpointはユーザーの各プロンプトで自動作成される
# 巻き戻すには: Esc+Esc を押すか /rewind を使用
# 巻き戻しメニューから復元するものを選択

# 08-checkpoints/README.md のサンプルを参照
```

### 高度なワークフロー
```bash
# 高度な機能を設定
# 09-advanced-features/config-examples.json を参照

# Planning modeを使用
/plan 機能Xを実装

# パーミッションモードを使用
claude --permission-mode plan          # コードレビュー用 (read-only)
claude --permission-mode acceptEdits   # 編集を自動承認
claude --permission-mode auto          # 安全なアクションを自動承認

# CI/CDにヘッドレスモードで実行
claude -p "テストを実行して結果を報告して"

# バックグラウンドタスクを実行
バックグラウンドでテストを実行

# 09-advanced-features/README.md で完全ガイドを参照
```

---

## 機能カバレッジマトリクス

| カテゴリ | コマンド | エージェント | MCP | Hooks | スクリプト | テンプレート | ドキュメント | 画像 | 合計 |
|----------|----------|--------|-----|-------|---------|-----------|------|--------|-------|
| **01 Slash Commands** | 8 | - | - | - | - | - | 1 | 1 | **10** |
| **02 Memory** | - | - | - | - | - | 3 | 1 | 2 | **6** |
| **03 Skills** | - | - | - | - | 5 | 9 | 1 | - | **28** |
| **04 Subagents** | - | 8 | - | - | - | - | 1 | - | **9** |
| **05 MCP** | - | - | 4 | - | - | - | 1 | - | **5** |
| **06 Hooks** | - | - | - | 8 | - | - | 1 | - | **9** |
| **07 Plugins** | 11 | 9 | 3 | 3 | 3 | 3 | 4 | - | **40** |
| **08 Checkpoints** | - | - | - | - | - | - | 1 | 1 | **2** |
| **09 Advanced** | - | - | - | - | - | - | 1 | 2 | **3** |
| **10 CLI** | - | - | - | - | - | - | 1 | - | **1** |

---

## 学習パス

### 初心者 (1週目)
1. ✅ `README.md` を読む
2. ✅ slash commandを1〜2個インストール
3. ✅ プロジェクトmemoryファイルを作成
4. ✅ 基本コマンドを試す

### 中級者 (2〜3週目)
1. ✅ GitHub MCPをセットアップ
2. ✅ subagentをインストール
3. ✅ タスクの委任を試す
4. ✅ skillをインストール

### 上級者 (4週目以降)
1. ✅ 完全なpluginをインストール
2. ✅ カスタムslash commandsを作成
3. ✅ カスタムsubagentを作成
4. ✅ カスタムskillを作成
5. ✅ 独自のpluginを構築

### エキスパート (5週目以降)
1. ✅ 自動化のためにhooksをセットアップ
2. ✅ 実験のためにcheckpointsを使用
3. ✅ Planning modeを設定
4. ✅ パーミッションモードを効果的に使用
5. ✅ CI/CDのためにヘッドレスモードをセットアップ
6. ✅ セッション管理をマスター

---

## キーワードで検索

### パフォーマンス
- `01-slash-commands/optimize.md` - パフォーマンス分析
- `04-subagents/code-reviewer.md` - パフォーマンスレビュー
- `03-skills/code-review/` - パフォーマンスメトリクス
- `07-plugins/pr-review/agents/performance-analyzer.md` - パフォーマンス専門家

### セキュリティ
- `04-subagents/secure-reviewer.md` - セキュリティレビュー
- `03-skills/code-review/` - セキュリティ分析
- `07-plugins/pr-review/` - セキュリティチェック

### テスト
- `04-subagents/test-engineer.md` - テストエンジニア
- `07-plugins/pr-review/commands/check-tests.md` - テストカバレッジ

### ドキュメント
- `01-slash-commands/generate-api-docs.md` - APIドキュメントコマンド
- `04-subagents/documentation-writer.md` - ドキュメントライターエージェント
- `03-skills/doc-generator/` - ドキュメント生成skill
- `07-plugins/documentation/` - 完全なドキュメントplugin

### デプロイ
- `07-plugins/devops-automation/` - 完全なDevOpsソリューション

### 自動化
- `06-hooks/` - イベント駆動自動化
- `06-hooks/pre-commit.sh` - コミット前自動化
- `06-hooks/format-code.sh` - 自動フォーマット
- `09-advanced-features/` - CI/CDのヘッドレスモード

### バリデーション
- `06-hooks/security-scan.sh` - セキュリティバリデーション
- `06-hooks/validate-prompt.sh` - プロンプトバリデーション

### 実験
- `08-checkpoints/` - rewindを使った安全な実験
- `08-checkpoints/checkpoint-examples.md` - 実際のサンプル

### 計画
- `09-advanced-features/planning-mode-examples.md` - Planning modeサンプル
- `09-advanced-features/README.md` - Extended thinking

### 設定
- `09-advanced-features/config-examples.json` - 設定サンプル

---

## 注意事項

- すべてのサンプルはすぐに使える状態です
- 自分のニーズに合わせて変更してください
- サンプルはClaude Codeのベストプラクティスに従っています
- 各カテゴリには詳細な指示を含む独自のREADMEがあります
- スクリプトには適切なエラー処理が含まれています
- テンプレートはカスタマイズ可能です

---

## コントリビュート

さらにサンプルを追加したい？以下の構造に従ってください:
1. 適切なサブディレクトリを作成
2. 使用方法を含むREADME.mdを含める
3. 命名規則に従う
4. 十分にテストする
5. このインデックスを更新する

---

**最終更新**: 2026年3月
**総サンプル数**: 100以上のファイル
**カテゴリ**: 10機能
**Hooks**: 8つの自動化スクリプト
**設定サンプル**: 10以上のシナリオ
**すぐに使えるもの**: すべてのサンプル
