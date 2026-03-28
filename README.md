<picture>
  <source media="(prefers-color-scheme: dark)" srcset="resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="resources/logos/claude-howto-logo.svg">
</picture>

[![GitHub Stars](https://img.shields.io/github/stars/luongnv89/claude-howto?style=flat&color=gold)](https://github.com/luongnv89/claude-howto/stargazers)
[![GitHub Forks](https://img.shields.io/github/forks/luongnv89/claude-howto?style=flat)](https://github.com/luongnv89/claude-howto/network/members)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Version](https://img.shields.io/badge/version-2.2.0-brightgreen)](CHANGELOG.md)
[![Claude Code](https://img.shields.io/badge/Claude_Code-2.1+-purple)](https://code.claude.com)

# 週末でClaude Codeをマスターする

`claude` と入力するところから、エージェント・hooks・skills・MCPサーバーのオーケストレーションまで — ビジュアルチュートリアル、コピペ用テンプレート、ガイド付き学習パスで一気に習得。

**[15分で始める](#-15分で始める)** | **[自分のレベルを確認する](#-どこから始めるかわからない方へ)** | **[機能カタログを見る](CATALOG.md)**

---

## 目次

- [問題点](#問題点)
- [このガイドが解決すること](#このガイドが解決すること)
- [使い方](#使い方)
- [どこから始めるかわからない方へ](#-どこから始めるかわからない方へ)
- [15分で始める](#-15分で始める)
- [何を作れるか](#何を作れるか)
- [よくある質問](#よくある質問)
- [コントリビュート](#コントリビュート)
- [ライセンス](#ライセンス)

---

## 問題点

Claude Codeをインストールした。いくつかプロンプトを試した。次は？

- **公式ドキュメントは機能を説明しているが、組み合わせ方は教えてくれない。** slash commandsの存在は知っていても、それをhooks・memory・subagentsと連携させて、実際に何時間もの作業を省けるワークフローを組む方法はわからない。
- **明確な学習パスがない。** hooksより先にMCPを学ぶべき？subagentsより先にskillsを？結局すべてを浅くかじっただけで、何も習得できない。
- **サンプルが単純すぎる。** 「Hello World」のslash commandでは、memory・専門エージェントへの委任・自動セキュリティスキャンを組み合わせた本番品質のコードレビューパイプラインは作れない。

Claude Codeのパワーの90%を使いこなせていない — しかも、自分が何を知らないかもわかっていない。

---

## このガイドが解決すること

これは単なる機能リファレンスではない。**構造化されたビジュアル・サンプル駆動のガイド**であり、すべてのClaude Code機能を実際のプロジェクトにすぐコピーして使えるテンプレートで学べる。

| | 公式ドキュメント | このガイド |
|--|---------------|------------|
| **形式** | リファレンス文書 | Mermaidダイアグラム付きビジュアルチュートリアル |
| **深さ** | 機能の説明 | 内部動作まで理解できる |
| **サンプル** | 基本的なスニペット | 即使える本番対応テンプレート |
| **構成** | 機能別 | 段階的学習パス（初心者〜上級者） |
| **オンボーディング** | 自主学習 | 所要時間付きガイド付きロードマップ |
| **自己評価** | なし | インタラクティブクイズで弱点を特定し個人化パスを生成 |

### 得られるもの:

- **10のチュートリアルモジュール** — slash commandsからカスタムエージェントチームまで、すべてのClaude Code機能を網羅
- **コピペ用設定ファイル** — slash commands・CLAUDE.mdテンプレート・hookスクリプト・MCP設定・subagent定義・完全なpluginバンドル
- **Mermaidダイアグラム** — 各機能の内部動作を図解。「どうやるか」ではなく「なぜそうなるか」まで理解できる
- **ガイド付き学習パス** — 11〜13時間で初心者からパワーユーザーへ
- **組み込み自己評価** — Claude Codeで `/self-assessment` または `/lesson-quiz hooks` を実行して弱点を把握

**[学習パスを始める ->](LEARNING-ROADMAP.md)**

---

## 使い方

### 1. 自分のレベルを確認する

[自己評価クイズ](LEARNING-ROADMAP.md#-自分のレベルを確認する)を受けるか、Claude Codeで `/self-assessment` を実行。現在の知識に基づいたパーソナライズされたロードマップを取得する。

### 2. ガイド付きパスを進む

10モジュールを順番にこなす — 各モジュールは前のものを土台にしている。学習しながらテンプレートをプロジェクトに直接コピーする。

### 3. 機能を組み合わせてワークフローを作る

本当の力は機能の組み合わせにある。slash commands + memory + subagents + hooksを連携させ、コードレビュー・デプロイ・ドキュメント生成を自動化するパイプラインの構築を学ぶ。

### 4. 理解度を確認する

各モジュール後に `/lesson-quiz [topic]` を実行。クイズが見落としを特定し、素早くギャップを埋められる。

**[15分で始める](#-15分で始める)**

---

## 1,100人以上の開発者に信頼されている

- **1,100以上のGitHub stars** — 毎日Claude Codeを使う開発者から
- **78フォーク** — 自分たちのワークフローに合わせてカスタマイズしているチーム
- **アクティブにメンテナンス中** — すべてのClaude Codeリリースに同期（最新: v2.2.0, 2026年3月）
- **コミュニティ主導** — 実際の設定を共有する開発者によるコントリビューション

[![Star History Chart](https://api.star-history.com/svg?repos=luongnv89/claude-howto&type=Date)](https://star-history.com/#luongnv89/claude-howto&Date)

---

## どこから始めるかわからない方へ

自己評価を受けるか、レベルを選んで始める:

| レベル | できること | 始める場所 | 時間 |
|-------|-----------|------------|------|
| **初心者** | Claude Codeを起動してチャットできる | [Slash Commands](01-slash-commands/) | 約2.5時間 |
| **中級者** | CLAUDE.mdとカスタムコマンドを使える | [Skills](03-skills/) | 約3.5時間 |
| **上級者** | MCPサーバーとhooksを設定できる | [Advanced Features](09-advanced-features/) | 約5時間 |

**全10モジュールの学習パス:**

| 順番 | モジュール | レベル | 時間 |
|-------|--------|-------|------|
| 1 | [Slash Commands](01-slash-commands/) | 初心者 | 30分 |
| 2 | [Memory](02-memory/) | 初心者+ | 45分 |
| 3 | [Checkpoints](08-checkpoints/) | 中級者 | 45分 |
| 4 | [CLI Basics](10-cli/) | 初心者+ | 30分 |
| 5 | [Skills](03-skills/) | 中級者 | 1時間 |
| 6 | [Hooks](06-hooks/) | 中級者 | 1時間 |
| 7 | [MCP](05-mcp/) | 中級者+ | 1時間 |
| 8 | [Subagents](04-subagents/) | 中級者+ | 1.5時間 |
| 9 | [Advanced Features](09-advanced-features/) | 上級者 | 2〜3時間 |
| 10 | [Plugins](07-plugins/) | 上級者 | 2時間 |

**[完全な学習ロードマップ ->](LEARNING-ROADMAP.md)**

---

## 15分で始める

```bash
# 1. ガイドをクローン
git clone https://github.com/luongnv89/claude-howto.git
cd claude-howto

# 2. 最初のslash commandをコピー
mkdir -p /path/to/your-project/.claude/commands
cp 01-slash-commands/optimize.md /path/to/your-project/.claude/commands/

# 3. 試してみる — Claude Codeで以下を入力:
# /optimize

# 4. もっと試したい？プロジェクトmemoryをセットアップ:
cp 02-memory/project-CLAUDE.md /path/to/your-project/CLAUDE.md

# 5. skillをインストール:
cp -r 03-skills/code-review ~/.claude/skills/
```

フルセットアップは？**1時間の必須セットアップ**:

```bash
# Slash commands (15分)
cp 01-slash-commands/*.md .claude/commands/

# プロジェクトmemory (15分)
cp 02-memory/project-CLAUDE.md ./CLAUDE.md

# skillをインストール (15分)
cp -r 03-skills/code-review ~/.claude/skills/

# 週末の目標: hooks・subagents・MCP・pluginsを追加
# 学習パスに従ってガイド付きセットアップを進める
```

**[インストールクイックリファレンスを見る](#インストールクイックリファレンス)**

---

## 何を作れるか

| ユースケース | 組み合わせる機能 |
|----------|------------------------|
| **自動コードレビュー** | Slash Commands + Subagents + Memory + MCP |
| **チームオンボーディング** | Memory + Slash Commands + Plugins |
| **CI/CD自動化** | CLI Reference + Hooks + Background Tasks |
| **ドキュメント生成** | Skills + Subagents + Plugins |
| **セキュリティ監査** | Subagents + Skills + Hooks (read-onlyモード) |
| **DevOpsパイプライン** | Plugins + MCP + Hooks + Background Tasks |
| **大規模リファクタリング** | Checkpoints + Planning Mode + Hooks |

---

## よくある質問

**無料ですか？**
はい。MITライセンスで永久無料。個人プロジェクト・職場・チームでご利用いただけます — ライセンス表記を含めること以外に制限はありません。

**メンテナンスはされていますか？**
アクティブにメンテナンス中。すべてのClaude Codeリリースに合わせて更新。現在のバージョン: v2.2.0 (2026年3月)、Claude Code 2.1+対応。

**公式ドキュメントとの違いは？**
公式ドキュメントは機能リファレンス。このガイドはダイアグラム・本番対応テンプレート・段階的学習パスを備えたチュートリアル。両方を補完的に使うのがベスト — まずここで学び、詳細が必要な時に公式ドキュメントを参照。

**全部やるのにどのくらいかかりますか？**
フルパスで11〜13時間。ただし15分で即効果を得られます — slash commandテンプレートをコピーして試すだけでOK。

**Claude Sonnet / Haiku / Opusでも使えますか？**
はい。すべてのテンプレートはClaude Sonnet 4.6、Claude Opus 4.6、Claude Haiku 4.5で動作します。

**コントリビュートできますか？**
もちろん。開始方法については [CONTRIBUTING.md](CONTRIBUTING.md) をご覧ください。新しいサンプル・バグ修正・ドキュメント改善・コミュニティテンプレートを歓迎します。

**オフラインで読めますか？**
はい。`uv run scripts/build_epub.py` を実行して、全コンテンツとレンダリングされたMermaidダイアグラムを含むEPUB電子書籍を生成できます。

---

## 今すぐClaude Codeをマスターしよう

Claude Codeはすでにインストールされている。あなたと10倍の生産性の間にあるのは、使いこなし方を知ることだけ。このガイドは構造化されたパス・ビジュアル解説・コピペ用テンプレートを提供する。

MITライセンス。永久無料。クローンして、フォークして、あなたのものにしよう。

**[学習パスを始める ->](LEARNING-ROADMAP.md)** | **[機能カタログを見る](CATALOG.md)** | **[15分で始める](#-15分で始める)**

---

<details>
<summary>クイックナビゲーション — 全機能</summary>

| 機能 | 説明 | フォルダ |
|---------|-------------|--------|
| **機能カタログ** | インストールコマンド付き完全リファレンス | [CATALOG.md](CATALOG.md) |
| **Slash Commands** | ユーザーが呼び出すショートカット | [01-slash-commands/](01-slash-commands/) |
| **Memory** | セッションを跨ぐ永続的なコンテキスト | [02-memory/](02-memory/) |
| **Skills** | 再利用可能な機能 | [03-skills/](03-skills/) |
| **Subagents** | 専門AIアシスタント | [04-subagents/](04-subagents/) |
| **MCPプロトコル** | 外部ツールへのアクセス | [05-mcp/](05-mcp/) |
| **Hooks** | イベント駆動の自動化 | [06-hooks/](06-hooks/) |
| **Plugins** | 機能のバンドル | [07-plugins/](07-plugins/) |
| **Checkpoints** | セッションのスナップショットと巻き戻し | [08-checkpoints/](08-checkpoints/) |
| **Advanced Features** | Planning・thinking・バックグラウンドタスク | [09-advanced-features/](09-advanced-features/) |
| **CLIリファレンス** | コマンド・フラグ・オプション | [10-cli/](10-cli/) |
| **ブログ記事** | 実際の使用例 | [ブログ記事](https://medium.com/@luongnv89) |

</details>

<details>
<summary>機能比較</summary>

| 機能 | 呼び出し方 | 永続性 | 最適な用途 |
|---------|-----------|------------|----------|
| **Slash Commands** | 手動 (`/cmd`) | セッション内のみ | クイックショートカット |
| **Memory** | 自動読み込み | セッション間 | 長期的な学習 |
| **Skills** | 自動呼び出し | ファイルシステム | 自動化ワークフロー |
| **Subagents** | 自動委任 | 独立したコンテキスト | タスク分散 |
| **MCPプロトコル** | 自動クエリ | リアルタイム | ライブデータアクセス |
| **Hooks** | イベントトリガー | 設定済み | 自動化とバリデーション |
| **Plugins** | ワンコマンド | 全機能 | 完全なソリューション |
| **Checkpoints** | 手動/自動 | セッションベース | 安全な実験 |
| **Planning Mode** | 手動/自動 | 計画フェーズ | 複雑な実装 |
| **Background Tasks** | 手動 | タスク期間中 | 長時間実行処理 |
| **CLIリファレンス** | ターミナルコマンド | セッション/スクリプト | 自動化とスクリプト |

</details>

<details>
<summary>インストールクイックリファレンス</summary>

```bash
# Slash Commands
cp 01-slash-commands/*.md .claude/commands/

# Memory
cp 02-memory/project-CLAUDE.md ./CLAUDE.md

# Skills
cp -r 03-skills/code-review ~/.claude/skills/

# Subagents
cp 04-subagents/*.md .claude/agents/

# MCP
export GITHUB_TOKEN="token"
claude mcp add github -- npx -y @modelcontextprotocol/server-github

# Hooks
mkdir -p ~/.claude/hooks
cp 06-hooks/*.sh ~/.claude/hooks/
chmod +x ~/.claude/hooks/*.sh

# Plugins
/plugin install pr-review

# Checkpoints (自動有効化、settingsで設定)
# 08-checkpoints/README.md を参照

# Advanced Features (settingsで設定)
# 09-advanced-features/config-examples.json を参照

# CLIリファレンス (インストール不要)
# 使用例は 10-cli/README.md を参照
```

</details>

<details>
<summary>01. Slash Commands</summary>

**場所**: [01-slash-commands/](01-slash-commands/)

**概要**: Markdownファイルとして保存されたユーザー呼び出しショートカット

**サンプル**:
- `optimize.md` - コード最適化分析
- `pr.md` - プルリクエスト準備
- `generate-api-docs.md` - APIドキュメント生成

**インストール**:
```bash
cp 01-slash-commands/*.md /path/to/project/.claude/commands/
```

**使い方**:
```
/optimize
/pr
/generate-api-docs
```

**詳細**: [Claude Code Slash Commandsの発見](https://medium.com/@luongnv89/discovering-claude-code-slash-commands-cdc17f0dfb29)

</details>

<details>
<summary>02. Memory</summary>

**場所**: [02-memory/](02-memory/)

**概要**: セッションを跨ぐ永続的なコンテキスト

**サンプル**:
- `project-CLAUDE.md` - チーム全体のプロジェクト標準
- `directory-api-CLAUDE.md` - ディレクトリ固有のルール
- `personal-CLAUDE.md` - 個人の設定

**インストール**:
```bash
# プロジェクトmemory
cp 02-memory/project-CLAUDE.md /path/to/project/CLAUDE.md

# ディレクトリmemory
cp 02-memory/directory-api-CLAUDE.md /path/to/project/src/api/CLAUDE.md

# 個人memory
cp 02-memory/personal-CLAUDE.md ~/.claude/CLAUDE.md
```

**使い方**: Claudeによって自動的に読み込まれる

</details>

<details>
<summary>03. Skills</summary>

**場所**: [03-skills/](03-skills/)

**概要**: 指示とスクリプトを持つ再利用可能な自動呼び出し機能

**サンプル**:
- `code-review/` - スクリプト付き包括的コードレビュー
- `brand-voice/` - ブランドボイスの一貫性チェッカー
- `doc-generator/` - APIドキュメント生成

**インストール**:
```bash
# 個人スキル
cp -r 03-skills/code-review ~/.claude/skills/

# プロジェクトスキル
cp -r 03-skills/code-review /path/to/project/.claude/skills/
```

**使い方**: 関連する場合に自動的に呼び出される

</details>

<details>
<summary>04. Subagents</summary>

**場所**: [04-subagents/](04-subagents/)

**概要**: 独立したコンテキストとカスタムプロンプトを持つ専門AIアシスタント

**サンプル**:
- `code-reviewer.md` - 包括的なコード品質分析
- `test-engineer.md` - テスト戦略とカバレッジ
- `documentation-writer.md` - 技術ドキュメント
- `secure-reviewer.md` - セキュリティ重視のレビュー (read-only)
- `implementation-agent.md` - フル機能実装

**インストール**:
```bash
cp 04-subagents/*.md /path/to/project/.claude/agents/
```

**使い方**: メインエージェントによって自動的に委任される

</details>

<details>
<summary>05. MCPプロトコル</summary>

**場所**: [05-mcp/](05-mcp/)

**概要**: 外部ツールとAPIにアクセスするためのModel Context Protocol

**サンプル**:
- `github-mcp.json` - GitHub連携
- `database-mcp.json` - データベースクエリ
- `filesystem-mcp.json` - ファイル操作
- `multi-mcp.json` - 複数MCPサーバー

**インストール**:
```bash
# 環境変数を設定
export GITHUB_TOKEN="your_token"
export DATABASE_URL="postgresql://..."

# CLIでMCPサーバーを追加
claude mcp add github -- npx -y @modelcontextprotocol/server-github

# またはプロジェクトの .mcp.json に手動で追加 (05-mcp/ のサンプルを参照)
```

**使い方**: 設定後、MCPツールはClaudeで自動的に利用可能になる

</details>

<details>
<summary>06. Hooks</summary>

**場所**: [06-hooks/](06-hooks/)

**概要**: Claude Codeのイベントに応じて自動実行されるイベント駆動シェルコマンド

**サンプル**:
- `format-code.sh` - 書き込み前にコードを自動フォーマット
- `pre-commit.sh` - コミット前にテストを実行
- `security-scan.sh` - セキュリティ問題をスキャン
- `log-bash.sh` - すべてのbashコマンドをログ
- `validate-prompt.sh` - ユーザープロンプトを検証
- `notify-team.sh` - イベント時にチームへ通知

**インストール**:
```bash
mkdir -p ~/.claude/hooks
cp 06-hooks/*.sh ~/.claude/hooks/
chmod +x ~/.claude/hooks/*.sh
```

`~/.claude/settings.json` にhooksを設定:
```json
{
  "hooks": {
    "PreToolUse": [{
      "matcher": "Write",
      "hooks": ["~/.claude/hooks/format-code.sh"]
    }],
    "PostToolUse": [{
      "matcher": "Write",
      "hooks": ["~/.claude/hooks/security-scan.sh"]
    }]
  }
}
```

**使い方**: Hooksはイベント発生時に自動実行される

**Hookの種類** (4種類、25イベント):
- **Tool Hooks**: `PreToolUse`, `PostToolUse`, `PostToolUseFailure`, `PermissionRequest`
- **Session Hooks**: `SessionStart`, `SessionEnd`, `Stop`, `StopFailure`, `SubagentStart`, `SubagentStop`
- **Task Hooks**: `UserPromptSubmit`, `TaskCompleted`, `TaskCreated`, `TeammateIdle`
- **Lifecycle Hooks**: `ConfigChange`, `CwdChanged`, `FileChanged`, `PreCompact`, `PostCompact`, `WorktreeCreate`, `WorktreeRemove`, `Notification`, `InstructionsLoaded`, `Elicitation`, `ElicitationResult`

</details>

<details>
<summary>07. Plugins</summary>

**場所**: [07-plugins/](07-plugins/)

**概要**: コマンド・エージェント・MCP・hooksをまとめたバンドルコレクション

**サンプル**:
- `pr-review/` - 完全なPRレビューワークフロー
- `devops-automation/` - デプロイとモニタリング
- `documentation/` - ドキュメント生成

**インストール**:
```bash
/plugin install pr-review
/plugin install devops-automation
/plugin install documentation
```

**使い方**: バンドルされたslash commandsと機能を使用

</details>

<details>
<summary>08. Checkpointsと巻き戻し</summary>

**場所**: [08-checkpoints/](08-checkpoints/)

**概要**: 会話の状態を保存し、以前のポイントに巻き戻して別のアプローチを探る

**主要概念**:
- **Checkpoint**: 会話状態のスナップショット
- **Rewind**: 以前のcheckpointに戻る
- **Branch Point**: 同じcheckpointから複数のアプローチを探る

**使い方**:
```
# Checkpointはユーザーの各プロンプトで自動作成される
# 巻き戻すには Esc を2回押すか以下を使用:
/rewind

# 5つのオプションから選択:
# 1. コードと会話を復元
# 2. 会話を復元
# 3. コードを復元
# 4. ここからまとめる
# 5. キャンセル
```

**ユースケース**:
- 異なる実装アプローチを試す
- ミスからの回復
- 安全な実験
- 代替ソリューションの比較
- 異なる設計のA/Bテスト

</details>

<details>
<summary>09. Advanced Features</summary>

**場所**: [09-advanced-features/](09-advanced-features/)

**概要**: 複雑なワークフローと自動化のための高度な機能

**内容**:
- **Planning Mode** — コーディング前に詳細な実装計画を作成
- **Extended Thinking** — 複雑な問題への深い推論 (`Alt+T` / `Option+T` で切り替え)
- **Background Tasks** — ブロックせずに長時間処理を実行
- **Permission Modes** — `default`, `acceptEdits`, `plan`, `dontAsk`, `bypassPermissions`
- **Headless Mode** — CI/CDでClaude Codeを実行: `claude -p "テストを実行してレポートを生成"`
- **Session Management** — `/resume`, `/rename`, `/fork`, `claude -c`, `claude -r`
- **設定** — `~/.claude/settings.json` で動作をカスタマイズ

完全な設定例は [config-examples.json](09-advanced-features/config-examples.json) を参照。

</details>

<details>
<summary>10. CLIリファレンス</summary>

**場所**: [10-cli/](10-cli/)

**概要**: Claude Codeの完全なコマンドラインインターフェースリファレンス

**クイックサンプル**:
```bash
# インタラクティブモード
claude "このプロジェクトを説明して"

# プリントモード (非インタラクティブ)
claude -p "このコードをレビューして"

# ファイル内容を処理
cat error.log | claude -p "このエラーを説明して"

# スクリプト用JSON出力
claude -p --output-format json "関数を一覧表示"

# セッションを再開
claude -r "feature-auth" "実装を続ける"
```

**ユースケース**: CI/CDパイプライン統合・スクリプト自動化・バッチ処理・マルチセッションワークフロー・カスタムエージェント設定

</details>

<details>
<summary>ワークフロー例</summary>

### 完全なコードレビューワークフロー

```markdown
# 使用機能: Slash Commands + Subagents + Memory + MCP

ユーザー: /review-pr

Claude:
1. プロジェクトmemoryを読み込む（コーディング標準）
2. GitHub MCPでPRを取得
3. code-reviewer subagentに委任
4. test-engineer subagentに委任
5. 結果を統合
6. 包括的なレビューを提供
```

### 自動ドキュメント生成

```markdown
# 使用機能: Skills + Subagents + Memory

ユーザー: "authモジュールのAPIドキュメントを生成して"

Claude:
1. プロジェクトmemoryを読み込む（ドキュメント標準）
2. ドキュメント生成リクエストを検出
3. doc-generator skillを自動呼び出し
4. api-documenter subagentに委任
5. サンプル付き包括的なドキュメントを作成
```

### DevOpsデプロイ

```markdown
# 使用機能: Plugins + MCP + Hooks

ユーザー: /deploy production

Claude:
1. pre-deploy hookを実行（環境を検証）
2. deployment-specialist subagentに委任
3. Kubernetes MCPでデプロイを実行
4. 進捗を監視
5. post-deploy hookを実行（ヘルスチェック）
6. 状態を報告
```

</details>

<details>
<summary>ディレクトリ構造</summary>

```
├── 01-slash-commands/
│   ├── optimize.md
│   ├── pr.md
│   ├── generate-api-docs.md
│   └── README.md
├── 02-memory/
│   ├── project-CLAUDE.md
│   ├── directory-api-CLAUDE.md
│   ├── personal-CLAUDE.md
│   └── README.md
├── 03-skills/
│   ├── code-review/
│   │   ├── SKILL.md
│   │   ├── scripts/
│   │   └── templates/
│   ├── brand-voice/
│   │   ├── SKILL.md
│   │   └── templates/
│   ├── doc-generator/
│   │   ├── SKILL.md
│   │   └── generate-docs.py
│   └── README.md
├── 04-subagents/
│   ├── code-reviewer.md
│   ├── test-engineer.md
│   ├── documentation-writer.md
│   ├── secure-reviewer.md
│   ├── implementation-agent.md
│   └── README.md
├── 05-mcp/
│   ├── github-mcp.json
│   ├── database-mcp.json
│   ├── filesystem-mcp.json
│   ├── multi-mcp.json
│   └── README.md
├── 06-hooks/
│   ├── format-code.sh
│   ├── pre-commit.sh
│   ├── security-scan.sh
│   ├── log-bash.sh
│   ├── validate-prompt.sh
│   ├── notify-team.sh
│   └── README.md
├── 07-plugins/
│   ├── pr-review/
│   ├── devops-automation/
│   ├── documentation/
│   └── README.md
├── 08-checkpoints/
│   ├── checkpoint-examples.md
│   └── README.md
├── 09-advanced-features/
│   ├── config-examples.json
│   ├── planning-mode-examples.md
│   └── README.md
├── 10-cli/
│   └── README.md
└── README.md (このファイル)
```

</details>

<details>
<summary>ベストプラクティス</summary>

### やること
- slash commandsからシンプルに始める
- 機能を段階的に追加する
- チーム標準にmemoryを使う
- まずローカルで設定をテストする
- カスタム実装をドキュメント化する
- プロジェクト設定をバージョン管理する
- pluginsをチームで共有する

### やってはいけないこと
- 冗長な機能を作らない
- 認証情報をハードコードしない
- ドキュメントを省かない
- シンプルなタスクを複雑にしすぎない
- セキュリティのベストプラクティスを無視しない
- 機密データをコミットしない

</details>

<details>
<summary>トラブルシューティング</summary>

### 機能が読み込まれない
1. ファイルの場所と名前を確認
2. YAMLフロントマターの構文を確認
3. ファイルのパーミッションを確認
4. Claude Codeのバージョン互換性を確認

### MCP接続失敗
1. 環境変数を確認
2. MCPサーバーのインストールを確認
3. 認証情報をテスト
4. ネットワーク接続を確認

### Subagentが委任しない
1. ツールのパーミッションを確認
2. エージェントの説明が明確か確認
3. タスクの複雑さを確認
4. エージェントを独立してテスト

</details>

<details>
<summary>テスト</summary>

このプロジェクトには包括的な自動テストが含まれている:

- **ユニットテスト**: pytestを使用したPythonテスト (Python 3.10, 3.11, 3.12)
- **コード品質**: Ruffによるリンティングとフォーマット
- **セキュリティ**: Banditによる脆弱性スキャン
- **型チェック**: mypyによる静的型解析
- **ビルド検証**: EPUB生成テスト
- **カバレッジ追跡**: Codecov連携

```bash
# 開発用依存関係をインストール
uv pip install -r requirements-dev.txt

# 全ユニットテストを実行
pytest scripts/tests/ -v

# カバレッジレポート付きでテストを実行
pytest scripts/tests/ -v --cov=scripts --cov-report=html

# コード品質チェックを実行
ruff check scripts/
ruff format --check scripts/

# セキュリティスキャンを実行
bandit -c pyproject.toml -r scripts/ --exclude scripts/tests/

# 型チェックを実行
mypy scripts/ --ignore-missing-imports
```

テストは `main`/`develop` へのすべてのpushと `main` へのすべてのPRで自動実行される。詳細は [TESTING.md](.github/TESTING.md) を参照。

</details>

<details>
<summary>EPUB生成</summary>

オフラインでこのガイドを読みたい？EPUBを生成:

```bash
uv run scripts/build_epub.py
```

これにより全コンテンツとレンダリングされたMermaidダイアグラムを含む `claude-howto-guide.epub` が作成される。

その他のオプションは [scripts/README.md](scripts/README.md) を参照。

</details>

<details>
<summary>コントリビュート</summary>

問題を見つけた、またはサンプルを追加したい？ぜひご協力ください！

**詳細なガイドラインは [CONTRIBUTING.md](CONTRIBUTING.md) を参照:**
- コントリビュートの種類（サンプル・ドキュメント・機能・バグ・フィードバック）
- 開発環境のセットアップ方法
- ディレクトリ構造とコンテンツの追加方法
- 執筆ガイドラインとベストプラクティス
- コミットとPRのプロセス

**コミュニティ基準:**
- [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md) - メンバー間の行動規範
- [SECURITY.md](SECURITY.md) - セキュリティポリシーと脆弱性報告

### セキュリティ問題の報告

セキュリティの脆弱性を発見した場合、責任を持って報告してください:

1. **GitHub プライベート脆弱性報告を使用**: https://github.com/luongnv89/claude-howto/security/advisories
2. **または** [.github/SECURITY_REPORTING.md](.github/SECURITY_REPORTING.md) の詳細な手順を参照
3. **公開Issueにセキュリティ脆弱性を投稿しないでください**

クイックスタート:
1. リポジトリをフォークしてクローン
2. 説明的なブランチを作成 (`add/feature-name`, `fix/bug`, `docs/improvement`)
3. ガイドラインに従って変更を加える
4. 明確な説明でプルリクエストを提出

**困ったら？** Issueまたはディスカッションを開いてください。プロセスをガイドします。

</details>

<details>
<summary>追加リソース</summary>

- [Claude Code ドキュメント](https://code.claude.com/docs/en/overview)
- [MCPプロトコル仕様](https://modelcontextprotocol.io)
- [Skills リポジトリ](https://github.com/luongnv89/skills) - すぐに使えるskillsのコレクション
- [Anthropic Cookbook](https://github.com/anthropics/anthropic-cookbook)
- [Boris ChernyのClaude Codeワークフロー](https://x.com/bcherny/status/2007179832300581177) - Claude Codeの作者が体系化されたワークフローを共有: 並列エージェント・共有CLAUDE.md・Plan mode・slash commands・subagents・自律長時間セッションのための検証hooks。

</details>

---

## コントリビュート

コントリビュートを歓迎します！始め方の詳細は [コントリビュートガイド](CONTRIBUTING.md) をご覧ください。

## コントリビューター

このプロジェクトに貢献してくれた方々に感謝します！

| コントリビューター | PR |
|-------------|-----|
| [wjhrdy](https://github.com/wjhrdy) | [#1 - EPUBを作成するツールを追加](https://github.com/luongnv89/claude-howto/pull/1) |
| [VikalpP](https://github.com/VikalpP) | [#7 - fix(docs): コンセプトガイドのネストされたコードブロックにチルダフェンスを使用](https://github.com/luongnv89/claude-howto/pull/7) |

---

## ライセンス

MITライセンス - [LICENSE](LICENSE) を参照。使用・改変・配布は自由。ライセンス表記を含めることが唯一の条件。

---

**最終更新**: 2026年3月
**Claude Codeバージョン**: 2.1+
**対応モデル**: Claude Sonnet 4.6, Claude Opus 4.6, Claude Haiku 4.5
