<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../resources/logos/claude-howto-logo.svg">
</picture>

# MCP（モデルコンテキストプロトコル）

このフォルダには、MCPサーバーの設定とClaude Codeでの使い方に関する包括的なドキュメントと例が含まれています。

## 概要

MCP（モデルコンテキストプロトコル）は、Claudeが外部ツール、API、リアルタイムデータソースにアクセスするための標準化された方法です。メモリとは異なり、MCPは変化するデータへのライブアクセスを提供します。

主な特徴：
- 外部サービスへのリアルタイムアクセス
- ライブデータの同期
- 拡張可能なアーキテクチャ
- 安全な認証
- ツールベースのインタラクション

## MCPアーキテクチャ

```mermaid
graph TB
    A["Claude"]
    B["MCPサーバー"]
    C["外部サービス"]

    A -->|リクエスト: list_issues| B
    B -->|クエリ| C
    C -->|データ| B
    B -->|レスポンス| A

    A -->|リクエスト: create_issue| B
    B -->|アクション| C
    C -->|結果| B
    B -->|レスポンス| A

    style A fill:#e1f5fe,stroke:#333,color:#333
    style B fill:#f3e5f5,stroke:#333,color:#333
    style C fill:#e8f5e9,stroke:#333,color:#333
```

## MCPエコシステム

```mermaid
graph TB
    A["Claude"] -->|MCP| B["Filesystem<br/>MCPサーバー"]
    A -->|MCP| C["GitHub<br/>MCPサーバー"]
    A -->|MCP| D["データベース<br/>MCPサーバー"]
    A -->|MCP| E["Slack<br/>MCPサーバー"]
    A -->|MCP| F["Google Docs<br/>MCPサーバー"]

    B -->|ファイルI/O| G["ローカルファイル"]
    C -->|API| H["GitHubリポジトリ"]
    D -->|クエリ| I["PostgreSQL/MySQL"]
    E -->|メッセージ| J["Slackワークスペース"]
    F -->|ドキュメント| K["Google Drive"]

    style A fill:#e1f5fe,stroke:#333,color:#333
    style B fill:#f3e5f5,stroke:#333,color:#333
    style C fill:#f3e5f5,stroke:#333,color:#333
    style D fill:#f3e5f5,stroke:#333,color:#333
    style E fill:#f3e5f5,stroke:#333,color:#333
    style F fill:#f3e5f5,stroke:#333,color:#333
    style G fill:#e8f5e9,stroke:#333,color:#333
    style H fill:#e8f5e9,stroke:#333,color:#333
    style I fill:#e8f5e9,stroke:#333,color:#333
    style J fill:#e8f5e9,stroke:#333,color:#333
    style K fill:#e8f5e9,stroke:#333,color:#333
```

## MCPインストール方法

Claude CodeはMCPサーバー接続用に複数のトランスポートプロトコルをサポートしています：

### HTTPトランスポート（推奨）

```bash
# 基本的なHTTP接続
claude mcp add --transport http notion https://mcp.notion.com/mcp

# 認証ヘッダー付きのHTTP
claude mcp add --transport http secure-api https://api.example.com/mcp \
  --header "Authorization: Bearer your-token"
```

### Stdioトランスポート（ローカル）

ローカルで実行するMCPサーバーの場合：

```bash
# ローカルNode.jsサーバー
claude mcp add --transport stdio myserver -- npx @myorg/mcp-server

# 環境変数付き
claude mcp add --transport stdio myserver --env KEY=value -- npx server
```

### SSEトランスポート（非推奨）

Server-Sent Eventsトランスポートは `http` に代わって非推奨になりましたが、まだサポートされています：

```bash
claude mcp add --transport sse legacy-server https://example.com/sse
```

### WebSocketトランスポート

永続的な双方向接続のためのWebSocketトランスポート：

```bash
claude mcp add --transport ws realtime-server wss://example.com/mcp
```

### Windowsでの注意事項

ネイティブWindows（WSLなし）では、npxコマンドに `cmd /c` を使用：

```bash
claude mcp add --transport stdio my-server -- cmd /c npx -y @some/package
```

### OAuth 2.0認証

Claude Codeは、必要なMCPサーバーのOAuth 2.0をサポートしています。OAuth対応サーバーに接続する際、Claude Codeが認証フロー全体を処理します：

```bash
# OAuth対応MCPサーバーに接続（インタラクティブフロー）
claude mcp add --transport http my-service https://my-service.example.com/mcp

# 非インタラクティブセットアップのためにOAuth認証情報を事前設定
claude mcp add --transport http my-service https://my-service.example.com/mcp \
  --client-id "your-client-id" \
  --client-secret "your-client-secret" \
  --callback-port 8080
```

| 機能 | 説明 |
|------|------|
| **インタラクティブOAuth** | `/mcp` を使ってブラウザベースのOAuthフローをトリガー |
| **事前設定OAuthクライアント** | Notion、Stripeなど一般的なサービスの組み込みOAuthクライアント（v2.1.30+） |
| **事前設定認証情報** | 自動セットアップのための `--client-id`、`--client-secret`、`--callback-port` フラグ |
| **トークン保存** | トークンはシステムキーチェーンに安全に保存 |
| **ステップアップ認証** | 特権操作のためのステップアップ認証をサポート |
| **ディスカバリーキャッシュ** | より速い再接続のためにOAuthディスカバリーメタデータをキャッシュ |
| **メタデータオーバーライド** | `.mcp.json` の `oauth.authServerMetadataUrl` でデフォルトのOAuthメタデータディスカバリーをオーバーライド |

#### OAuthメタデータディスカバリーのオーバーライド

MCPサーバーが標準のOAuthメタデータエンドポイント（`/.well-known/oauth-authorization-server`）でエラーを返すが、動作するOIDCエンドポイントを公開している場合、特定のURLからOAuthメタデータを取得するようClaude Codeに指示できます。サーバー設定の `oauth` オブジェクトに `authServerMetadataUrl` を設定：

```json
{
  "mcpServers": {
    "my-server": {
      "type": "http",
      "url": "https://mcp.example.com/mcp",
      "oauth": {
        "authServerMetadataUrl": "https://auth.example.com/.well-known/openid-configuration"
      }
    }
  }
}
```

URLは `https://` を使用する必要があります。このオプションはClaude Code v2.1.64以降が必要です。

### Claude.ai MCPコネクター

Claude.aiアカウントで設定されたMCPサーバーは、Claude Codeで自動的に利用可能になります。Claude.aiウェブインターフェースで設定したMCP接続は、追加設定なしにアクセスできます。

Claude.ai MCPコネクターは `--print` モード（v2.1.83+）でも利用可能で、非インタラクティブおよびスクリプトでの使用が可能です。

Claude CodeでClaude.ai MCPサーバーを無効化するには、`ENABLE_CLAUDEAI_MCP_SERVERS` 環境変数を `false` に設定：

```bash
ENABLE_CLAUDEAI_MCP_SERVERS=false claude
```

> **注意**: この機能はClaude.aiアカウントでログインしているユーザーのみ利用可能です。

## MCPセットアッププロセス

```mermaid
sequenceDiagram
    participant User as ユーザー
    participant Claude as Claude Code
    participant Config as 設定ファイル
    participant Service as 外部サービス

    User->>Claude: /mcp を入力
    Claude->>Claude: 利用可能なMCPサーバーを一覧表示
    Claude->>User: オプションを表示
    User->>Claude: GitHub MCPを選択
    Claude->>Config: 設定を更新
    Config->>Claude: 接続をアクティブ化
    Claude->>Service: 接続テスト
    Service-->>Claude: 認証成功
    Claude->>User: ✅ MCP接続完了！
```

## MCPツール検索

MCPツールの説明がコンテキストウィンドウの10%を超えると、Claude Codeはモデルのコンテキストを圧迫せずに適切なツールを効率的に選択するためにツール検索を自動的に有効化します。

| 設定 | 値 | 説明 |
|------|----|----|
| `ENABLE_TOOL_SEARCH` | `auto`（デフォルト） | ツールの説明がコンテキストの10%を超えたときに自動的に有効化 |
| `ENABLE_TOOL_SEARCH` | `auto:<N>` | `N` ツールのカスタムしきい値で自動的に有効化 |
| `ENABLE_TOOL_SEARCH` | `true` | ツール数に関わらず常に有効 |
| `ENABLE_TOOL_SEARCH` | `false` | 無効；すべてのツールの説明を完全に送信 |

> **注意**: ツール検索にはSonnet 4以降またはOpus 4以降が必要です。HaikuモデルはツールI検索をサポートしていません。

## 動的ツール更新

Claude Codeは MCP の `list_changed` 通知をサポートしています。MCPサーバーが利用可能なツールを動的に追加、削除、変更すると、Claude Codeがその更新を受け取り、再接続や再起動なしにツールリストを自動的に調整します。

## MCPのElicitation

MCPサーバーはインタラクティブダイアログを通じてユーザーから構造化された入力を要求できます（v2.1.49+）。これにより、MCPサーバーがワークフローの途中で追加情報を求めることができます — 例えば、確認を求める、オプションのリストから選択する、必須フィールドに記入するなど — MCPサーバーとのインタラクションにインタラクティブ性を追加します。

## ツールの説明と指示の上限

v2.1.84以降、Claude CodeはMCPサーバーごとのツールの説明と指示に**2 KBの上限**を適用します。これにより、個々のサーバーが過度に詳細なツール定義でコンテキストを消費することを防ぎ、コンテキストの肥大化を減らしてインタラクションを効率的に保ちます。

## MCPプロンプトのスラッシュコマンド

MCPサーバーはClaude Codeでスラッシュコマンドとして表示されるプロンプトを公開できます。プロンプトは次の命名規則でアクセスできます：

```
/mcp__<server>__<prompt>
```

例えば、`github` というサーバーが `review` というプロンプトを公開している場合、`/mcp__github__review` として呼び出せます。

## サーバーの重複排除

同じMCPサーバーが複数のスコープ（ローカル、プロジェクト、ユーザー）で定義されている場合、ローカルの設定が優先されます。これにより、競合なしにプロジェクトレベルまたはユーザーレベルのMCP設定をローカルのカスタマイズで上書きできます。

## @メンションによるMCPリソース

`@` メンション構文を使ってプロンプトで直接MCPリソースを参照できます：

```
@server-name:protocol://resource/path
```

例えば、特定のデータベースリソースを参照する場合：

```
@database:postgres://mydb/users
```

これにより、Claudeが会話のコンテキストの一部としてMCPリソースのコンテンツをインラインでフェッチして含められます。

## MCPスコープ

MCP設定は、共有レベルが異なる複数のスコープに保存できます：

| スコープ | 場所 | 説明 | 共有先 | 承認が必要 |
|---------|------|------|-------|----------|
| **ローカル**（デフォルト） | `~/.claude.json`（プロジェクトパス以下） | 現在のユーザーと現在のプロジェクトのみ（旧バージョンでは `project` と呼ばれていた） | あなただけ | いいえ |
| **プロジェクト** | `.mcp.json` | gitリポジトリにコミット | チームメンバー | はい（初回使用時） |
| **ユーザー** | `~/.claude.json` | すべてのプロジェクトで利用可能（旧バージョンでは `global` と呼ばれていた） | あなただけ | いいえ |

### プロジェクトスコープの使用

プロジェクト固有のMCP設定を `.mcp.json` に保存：

```json
{
  "mcpServers": {
    "github": {
      "type": "http",
      "url": "https://api.github.com/mcp"
    }
  }
}
```

チームメンバーはプロジェクトMCPを初回使用時に承認プロンプトが表示されます。

## MCP設定の管理

### MCPサーバーの追加

```bash
# HTTPベースのサーバーを追加
claude mcp add --transport http github https://api.github.com/mcp

# ローカルstdioサーバーを追加
claude mcp add --transport stdio database -- npx @company/db-server

# すべてのMCPサーバーを一覧表示
claude mcp list

# 特定のサーバーの詳細を取得
claude mcp get github

# MCPサーバーを削除
claude mcp remove github

# プロジェクト固有の承認選択をリセット
claude mcp reset-project-choices

# Claude Desktopからインポート
claude mcp add-from-claude-desktop
```

## 利用可能なMCPサーバー一覧

| MCPサーバー | 目的 | 主なツール | 認証 | リアルタイム |
|------------|------|-----------|------|------------|
| **Filesystem** | ファイル操作 | read, write, delete | OSパーミッション | ✅ あり |
| **GitHub** | リポジトリ管理 | list_prs, create_issue, push | OAuth | ✅ あり |
| **Slack** | チームコミュニケーション | send_message, list_channels | トークン | ✅ あり |
| **Database** | SQLクエリ | query, insert, update | 認証情報 | ✅ あり |
| **Google Docs** | ドキュメントアクセス | read, write, share | OAuth | ✅ あり |
| **Asana** | プロジェクト管理 | create_task, update_status | APIキー | ✅ あり |
| **Stripe** | 支払いデータ | list_charges, create_invoice | APIキー | ✅ あり |
| **Memory** | 永続メモリ | store, retrieve, delete | ローカル | ❌ なし |

## 実践例

### 例1: GitHub MCP設定

**ファイル:** `.mcp.json`（プロジェクトルート）

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

**利用可能なGitHub MCPツール:**

#### プルリクエスト管理
- `list_prs` - リポジトリのすべてのPRを一覧表示
- `get_pr` - 差分を含むPRの詳細を取得
- `create_pr` - 新しいPRを作成
- `update_pr` - PRの説明/タイトルを更新
- `merge_pr` - PRをmainブランチにマージ
- `review_pr` - レビューコメントを追加

**リクエスト例:**
```
/mcp__github__get_pr 456

# 返される内容:
タイトル: Add dark mode support
作成者: @alice
説明: CSSカスタムプロパティを使用したダークテーマの実装
ステータス: OPEN
レビュアー: @bob, @charlie
```

#### Issue管理
- `list_issues` - すべてのIssueを一覧表示
- `get_issue` - Issueの詳細を取得
- `create_issue` - 新しいIssueを作成
- `close_issue` - Issueをクローズ
- `add_comment` - Issueにコメントを追加

#### リポジトリ情報
- `get_repo_info` - リポジトリの詳細
- `list_files` - ファイルツリー構造
- `get_file_content` - ファイルの内容を読む
- `search_code` - コードベース全体を検索

#### コミット操作
- `list_commits` - コミット履歴
- `get_commit` - 特定のコミットの詳細
- `create_commit` - 新しいコミットを作成

**セットアップ**:
```bash
export GITHUB_TOKEN="your_github_token"
# またはCLIで直接追加:
claude mcp add --transport stdio github -- npx @modelcontextprotocol/server-github
```

### 設定での環境変数展開

MCP設定はフォールバックデフォルト付きの環境変数展開をサポートしています。`${VAR}` と `${VAR:-default}` 構文は次のフィールドで機能します: `command`、`args`、`env`、`url`、`headers`。

```json
{
  "mcpServers": {
    "api-server": {
      "type": "http",
      "url": "${API_BASE_URL:-https://api.example.com}/mcp",
      "headers": {
        "Authorization": "Bearer ${API_KEY}",
        "X-Custom-Header": "${CUSTOM_HEADER:-default-value}"
      }
    },
    "local-server": {
      "command": "${MCP_BIN_PATH:-npx}",
      "args": ["${MCP_PACKAGE:-@company/mcp-server}"],
      "env": {
        "DB_URL": "${DATABASE_URL:-postgresql://localhost/dev}"
      }
    }
  }
}
```

変数は実行時に展開されます：
- `${VAR}` - 環境変数を使用、設定されていない場合はエラー
- `${VAR:-default}` - 環境変数を使用、設定されていない場合はデフォルトにフォールバック

### 例2: データベースMCPのセットアップ

**設定:**

```json
{
  "mcpServers": {
    "database": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-database"],
      "env": {
        "DATABASE_URL": "postgresql://user:pass@localhost/mydb"
      }
    }
  }
}
```

**使用例:**

```markdown
ユーザー: 10件以上の注文があるすべてのユーザーを取得して

Claude: データベースからその情報を検索します。

# MCP databaseツールを使用:
SELECT u.*, COUNT(o.id) as order_count
FROM users u
LEFT JOIN orders o ON u.id = o.user_id
GROUP BY u.id
HAVING COUNT(o.id) > 10
ORDER BY order_count DESC;

# 結果:
- Alice: 15件の注文
- Bob: 12件の注文
- Charlie: 11件の注文
```

**セットアップ**:
```bash
export DATABASE_URL="postgresql://user:pass@localhost/mydb"
# またはCLIで直接追加:
claude mcp add --transport stdio database -- npx @modelcontextprotocol/server-database
```

### 例3: マルチMCPワークフロー

**シナリオ: 日次レポート生成**

```markdown
# 複数のMCPを使った日次レポートワークフロー

## セットアップ
1. GitHub MCP - PRメトリクスを取得
2. データベースMCP - 売上データをクエリ
3. Slack MCP - レポートを投稿
4. Filesystem MCP - レポートを保存

## ワークフロー

### ステップ1: GitHubデータを取得
/mcp__github__list_prs completed:true last:7days

出力:
- 総PR数: 42
- 平均マージ時間: 2.3時間
- レビュー応答時間: 1.1時間

### ステップ2: データベースをクエリ
SELECT COUNT(*) as sales, SUM(amount) as revenue
FROM orders
WHERE created_at > NOW() - INTERVAL '1 day'

出力:
- 売上: 247件
- 売上高: $12,450

### ステップ3: レポートを生成
データをHTMLレポートに統合

### ステップ4: Filesystemに保存
report.htmlを /reports/ に書き込む

### ステップ5: Slackに投稿
サマリーを #daily-reports チャンネルに送信

最終出力:
✅ レポートが生成され投稿された
📊 今週47件のPRがマージされた
💰 日次売上高 $12,450
```

**セットアップ**:
```bash
export GITHUB_TOKEN="your_github_token"
export DATABASE_URL="postgresql://user:pass@localhost/mydb"
export SLACK_TOKEN="your_slack_token"
# CLIまたは .mcp.json で各MCPサーバーを追加
```

### 例4: Filesystem MCPの操作

**設定:**

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-filesystem", "/home/user/projects"]
    }
  }
}
```

**利用可能な操作:**

| 操作 | コマンド | 目的 |
|------|---------|------|
| ファイル一覧 | `ls ~/projects` | ディレクトリの内容を表示 |
| ファイル読み取り | `cat src/main.ts` | ファイルの内容を読む |
| ファイル作成 | `create docs/api.md` | 新しいファイルを作成 |
| ファイル編集 | `edit src/app.ts` | ファイルを修正 |
| 検索 | `grep "async function"` | ファイル内を検索 |
| 削除 | `rm old-file.js` | ファイルを削除 |

**セットアップ**:
```bash
# CLIで直接追加:
claude mcp add --transport stdio filesystem -- npx @modelcontextprotocol/server-filesystem /home/user/projects
```

## MCPとメモリの使い分け

```mermaid
graph TD
    A["外部データが必要？"]
    A -->|いいえ| B["メモリを使用"]
    A -->|はい| C["頻繁に変化するか？"]
    C -->|いいえ/まれに| B
    C -->|はい/よく| D["MCPを使用"]

    B -->|保存する| E["設定<br/>コンテキスト<br/>履歴"]
    D -->|アクセスする| F["ライブAPI<br/>データベース<br/>サービス"]

    style A fill:#fff3e0,stroke:#333,color:#333
    style B fill:#e1f5fe,stroke:#333,color:#333
    style C fill:#fff3e0,stroke:#333,color:#333
    style D fill:#f3e5f5,stroke:#333,color:#333
    style E fill:#e8f5e9,stroke:#333,color:#333
    style F fill:#e8f5e9,stroke:#333,color:#333
```

## リクエスト/レスポンスパターン

```mermaid
sequenceDiagram
    participant App as Claude
    participant MCP as MCPサーバー
    participant DB as データベース

    App->>MCP: リクエスト: "SELECT * FROM users WHERE id=1"
    MCP->>DB: クエリを実行
    DB-->>MCP: 結果セット
    MCP-->>App: 解析されたデータを返す
    App->>App: 結果を処理
    App->>App: タスクを続行

    Note over MCP,DB: リアルタイムアクセス<br/>キャッシュなし
```

## 環境変数

機密の認証情報は環境変数に保存：

```bash
# ~/.bashrc または ~/.zshrc
export GITHUB_TOKEN="ghp_xxxxxxxxxxxxx"
export DATABASE_URL="postgresql://user:pass@localhost/mydb"
export SLACK_TOKEN="xoxb-xxxxxxxxxxxxx"
```

次にMCP設定で参照：

```json
{
  "env": {
    "GITHUB_TOKEN": "${GITHUB_TOKEN}"
  }
}
```

## ClaudeをMCPサーバーとして使う（`claude mcp serve`）

Claude Code自体が他のアプリケーション用のMCPサーバーとして機能できます。これにより、外部ツール、エディタ、自動化システムが標準MCPプロトコルを通じてClaudeの機能を活用できます。

```bash
# stdioでMCPサーバーとしてClaude Codeを起動
claude mcp serve
```

他のアプリケーションは、stdioベースのMCPサーバーと同様にこのサーバーに接続できます。例えば、別のClaude CodeインスタンスにClaude CodeをMCPサーバーとして追加する場合：

```bash
claude mcp add --transport stdio claude-agent -- claude mcp serve
```

これは、1つのClaudeインスタンスが別のインスタンスをオーケストレートするマルチエージェントワークフローの構築に便利です。

## マネージドMCP設定（エンタープライズ）

エンタープライズデプロイメントでは、ITアドミニストレーターが `managed-mcp.json` 設定ファイルを通じてMCPサーバーポリシーを適用できます。このファイルは、組織全体で許可またはブロックするMCPサーバーを独占的に制御します。

**場所:**
- macOS: `/Library/Application Support/ClaudeCode/managed-mcp.json`
- Linux: `~/.config/ClaudeCode/managed-mcp.json`
- Windows: `%APPDATA%\ClaudeCode\managed-mcp.json`

**機能:**
- `allowedMcpServers` -- 許可されたサーバーのホワイトリスト
- `deniedMcpServers` -- 禁止されたサーバーのブロックリスト
- サーバー名、コマンド、URLパターンによるマッチングをサポート
- ユーザー設定より先に適用される組織全体のMCPポリシー
- 許可されていないサーバー接続を防止

**設定例:**

```json
{
  "allowedMcpServers": [
    {
      "serverName": "github",
      "serverUrl": "https://api.github.com/mcp"
    },
    {
      "serverName": "company-internal",
      "serverCommand": "company-mcp-server"
    }
  ],
  "deniedMcpServers": [
    {
      "serverName": "untrusted-*"
    },
    {
      "serverUrl": "http://*"
    }
  ]
}
```

> **注意**: `allowedMcpServers` と `deniedMcpServers` の両方がサーバーにマッチした場合、拒否ルールが優先されます。

## プラグイン提供のMCPサーバー

プラグインは独自のMCPサーバーをバンドルでき、プラグインがインストールされると自動的に利用可能になります。プラグイン提供のMCPサーバーは2つの方法で定義できます：

1. **スタンドアロンの `.mcp.json`** -- プラグインルートディレクトリに `.mcp.json` ファイルを配置
2. **`plugin.json` にインライン** -- プラグインマニフェスト内に直接MCPサーバーを定義

プラグインのインストールディレクトリからの相対パスを参照するために `${CLAUDE_PLUGIN_ROOT}` 変数を使用：

```json
{
  "mcpServers": {
    "plugin-tools": {
      "command": "node",
      "args": ["${CLAUDE_PLUGIN_ROOT}/dist/mcp-server.js"],
      "env": {
        "CONFIG_PATH": "${CLAUDE_PLUGIN_ROOT}/config.json"
      }
    }
  }
}
```

## サブエージェントスコープのMCP

MCPサーバーはエージェントフロントマター内で `mcpServers:` キーを使ってインラインで定義でき、プロジェクト全体ではなく特定のサブエージェントにスコープを絞れます。これは、エージェントがワークフローの他のエージェントが必要としない特定のMCPサーバーへのアクセスが必要な場合に便利です。

```yaml
---
mcpServers:
  my-tool:
    type: http
    url: https://my-tool.example.com/mcp
---

あなたは特殊な操作のために my-tool へのアクセスを持つエージェントです。
```

サブエージェントスコープのMCPサーバーはそのエージェントの実行コンテキスト内でのみ利用可能で、親エージェントや兄弟エージェントとは共有されません。

## MCPの出力制限

Claude Codeはコンテキストオーバーフローを防ぐためにMCPツールの出力に制限を適用します：

| 制限 | しきい値 | 動作 |
|------|---------|------|
| **警告** | 10,000トークン | 出力が大きいという警告が表示される |
| **デフォルト上限** | 25,000トークン | この制限を超えると出力が切り捨てられる |
| **ディスク永続化** | 50,000文字 | 50K文字を超えるツール結果はディスクに永続化される |

最大出力制限は `MAX_MCP_OUTPUT_TOKENS` 環境変数で設定可能：

```bash
# 最大出力を50,000トークンに増加
export MAX_MCP_OUTPUT_TOKENS=50000
```

## コード実行でコンテキスト肥大化を解決する

MCPの採用が拡大し、数十のサーバーと数百または数千のツールに接続するようになると、重大な課題が生じます: **コンテキスト肥大化**。これはMCPをスケールで使う際の最大の問題であり、Anthropicのエンジニアリングチームはエレガントなソリューションを提案しています — 直接ツール呼び出しの代わりにコード実行を使う。

> **ソース**: [Code Execution with MCP: Building More Efficient Agents](https://www.anthropic.com/engineering/code-execution-with-mcp) — Anthropic Engineering Blog

### 問題: トークン無駄使いの2つのソース

**1. ツール定義がコンテキストウィンドウを圧迫する**

ほとんどのMCPクライアントはすべてのツール定義を事前に読み込みます。数千のツールに接続している場合、モデルはユーザーのリクエストを読む前に数十万トークンを処理する必要があります。

**2. 中間結果が追加トークンを消費する**

すべての中間ツール結果がモデルのコンテキストを通過します。Google DriveからSalesforceに会議の議事録を転送することを考えると、議事録はコンテキストを**2回**流れます: 読み取り時と、宛先に書き込む時。2時間の会議の議事録は50,000以上の追加トークンを意味する可能性があります。

```mermaid
graph LR
    A["モデル"] -->|"ツール呼び出し: getDocument"| B["MCPサーバー"]
    B -->|"全議事録（50Kトークン）"| A
    A -->|"ツール呼び出し: updateRecord<br/>（全議事録を再送信）"| B
    B -->|"確認"| A

    style A fill:#ffcdd2,stroke:#333,color:#333
    style B fill:#f3e5f5,stroke:#333,color:#333
```

### 解決策: APIとしてのMCPツール

ツールの定義と結果をコンテキストウィンドウを通じて渡す代わりに、エージェントはMCPツールをAPIとして呼び出す**コードを書きます**。コードはサンドボックス化された実行環境で実行され、最終結果のみがモデルに返ります。

```mermaid
graph LR
    A["モデル"] -->|"コードを書く"| B["コード実行<br/>環境"]
    B -->|"ツールを直接呼び出す"| C["MCPサーバー"]
    C -->|"データは実行環境に<br/>留まる"| B
    B -->|"最終結果のみ<br/>（最小トークン）"| A

    style A fill:#c8e6c9,stroke:#333,color:#333
    style B fill:#e1f5fe,stroke:#333,color:#333
    style C fill:#f3e5f5,stroke:#333,color:#333
```

#### 仕組み

MCPツールは型付き関数のファイルツリーとして表現されます：

```
servers/
├── google-drive/
│   ├── getDocument.ts
│   └── index.ts
├── salesforce/
│   ├── updateRecord.ts
│   └── index.ts
└── ...
```

各ツールファイルには型付きラッパーが含まれています：

```typescript
// ./servers/google-drive/getDocument.ts
import { callMCPTool } from "../../../client.js";

interface GetDocumentInput {
  documentId: string;
}

interface GetDocumentResponse {
  content: string;
}

export async function getDocument(
  input: GetDocumentInput
): Promise<GetDocumentResponse> {
  return callMCPTool<GetDocumentResponse>(
    'google_drive__get_document', input
  );
}
```

エージェントはツールをオーケストレートするコードを書きます：

```typescript
import * as gdrive from './servers/google-drive';
import * as salesforce from './servers/salesforce';

// データはツール間を直接流れる — モデルを通じて渡されない
const transcript = (
  await gdrive.getDocument({ documentId: 'abc123' })
).content;

await salesforce.updateRecord({
  objectType: 'SalesMeeting',
  recordId: '00Q5f000001abcXYZ',
  data: { Notes: transcript }
});
```

**結果: トークン使用量が約150,000から約2,000に削減 — 98.7%の削減。**

### 主なメリット

| メリット | 説明 |
|---------|------|
| **プログレッシブディスクロージャー** | エージェントがファイルシステムを閲覧して必要なツール定義のみを読み込む — すべてのツールを事前に読み込まない |
| **コンテキスト効率的な結果** | データがモデルに返る前に実行環境でフィルタリング/変換される |
| **強力な制御フロー** | ループ、条件分岐、エラーハンドリングがモデルを往復せずコードで実行される |
| **プライバシー保護** | 中間データ（PII、機密記録）が実行環境に留まり、モデルのコンテキストに入らない |
| **状態永続化** | エージェントが中間結果をファイルに保存し、再利用可能なスキル関数を構築できる |

#### 例: 大きなデータセットのフィルタリング

```typescript
// コード実行なし — 全10,000行がコンテキストを流れる
// ツール呼び出し: gdrive.getSheet(sheetId: 'abc123')
//   -> コンテキスト内で10,000行を返す

// コード実行あり — 実行環境でフィルタリング
const allRows = await gdrive.getSheet({ sheetId: 'abc123' });
const pendingOrders = allRows.filter(
  row => row["Status"] === 'pending'
);
console.log(`${pendingOrders.length}件の保留中の注文が見つかりました`);
console.log(pendingOrders.slice(0, 5)); // 5行のみがモデルに到達
```

#### 例: 往復なしのループ

```typescript
// デプロイ通知をポーリング — コード内で完全に実行
let found = false;
while (!found) {
  const messages = await slack.getChannelHistory({
    channel: 'C123456'
  });
  found = messages.some(
    m => m.text.includes('deployment complete')
  );
  if (!found) await new Promise(r => setTimeout(r, 5000));
}
console.log('デプロイ通知を受信しました');
```

### 考慮すべきトレードオフ

コード実行には独自の複雑さがあります。エージェントが生成したコードの実行には以下が必要です：

- 適切なリソース制限を持つ**セキュアなサンドボックス実行環境**
- 実行されたコードの**モニタリングとロギング**
- 直接ツール呼び出しと比べた追加の**インフラオーバーヘッド**

メリット（削減されたトークンコスト、低レイテンシ、改善されたツール合成）とこれらの実装コストを比較検討する必要があります。MCPサーバーが数台のみのエージェントでは、直接ツール呼び出しの方がシンプルかもしれません。スケールでのエージェント（数十のサーバー、数百のツール）では、コード実行は大きな改善です。

### MCPorter: MCPツール合成のランタイム

[MCPorter](https://github.com/steipete/mcporter)は、ボイラープレートなしにMCPサーバーを実用的に呼び出し、選択的なツール露出と型付きラッパーによってコンテキスト肥大化を削減するためのTypeScriptランタイムおよびCLIツールキットです。

**解決する問題:** すべてのMCPサーバーからすべてのツール定義を事前に読み込む代わりに、MCPorterはオンデマンドで特定のツールを探索、検査、呼び出しできます — コンテキストをスリムに保ちます。

**主な機能:**

| 機能 | 説明 |
|------|------|
| **ゼロコンフィグディスカバリー** | Cursor、Claude、Codex、またはローカル設定からMCPサーバーを自動探索 |
| **型付きツールクライアント** | `mcporter emit-ts` が `.d.ts` インターフェースとすぐに実行できるラッパーを生成 |
| **合成可能なAPI** | `createServerProxy()` がツールをキャメルケースメソッドとして `.text()`、`.json()`、`.markdown()` ヘルパー付きで公開 |
| **CLI生成** | `mcporter generate-cli` が任意のMCPサーバーを `--include-tools` / `--exclude-tools` フィルタリング付きのスタンドアロンCLIに変換 |
| **パラメーター非表示** | オプションパラメーターはデフォルトで非表示になり、スキーマの冗長性を削減 |

**インストール:**

```bash
npx mcporter list          # インストール不要 — サーバーを即座に探索
pnpm add mcporter          # プロジェクトに追加
brew install steipete/tap/mcporter  # macOS via Homebrew
```

**例 — TypeScriptでのツール合成:**

```typescript
import { createRuntime, createServerProxy } from "mcporter";

const runtime = await createRuntime();
const gdrive = createServerProxy(runtime, "google-drive");
const salesforce = createServerProxy(runtime, "salesforce");

// データはモデルコンテキストを通らずにツール間を流れる
const doc = await gdrive.getDocument({ documentId: "abc123" });
await salesforce.updateRecord({
  objectType: "SalesMeeting",
  recordId: "00Q5f000001abcXYZ",
  data: { Notes: doc.text() }
});
```

**例 — CLIツール呼び出し:**

```bash
# 特定のツールを直接呼び出す
npx mcporter call linear.create_comment issueId:ENG-123 body:'Looks good!'

# 利用可能なサーバーとツールを一覧表示
npx mcporter list
```

MCPorterは上記のコード実行アプローチを補完し、MCPツールを型付きAPIとして呼び出すためのランタイムインフラを提供します — 中間データをモデルコンテキストの外に保つことを簡単にします。

## ベストプラクティス

### セキュリティに関する考慮事項

#### やること ✅
- すべての認証情報に環境変数を使用する
- トークンとAPIキーを定期的にローテーションする（月次推奨）
- 可能な限り読み取り専用トークンを使用する
- MCPサーバーのアクセススコープを必要最小限に制限する
- MCPサーバーの使用状況とアクセスログを監視する
- 外部サービスでは利用可能な場合にOAuthを使用する
- MCPリクエストにレートリミットを実装する
- 本番環境での使用前にMCP接続をテストする
- すべてのアクティブなMCP接続を文書化する
- MCPサーバーパッケージを最新の状態に保つ

#### やってはいけないこと ❌
- 設定ファイルに認証情報をハードコードしない
- トークンやシークレットをgitにコミットしない
- チームチャットやメールでトークンを共有しない
- チームプロジェクトに個人トークンを使用しない
- 不必要なパーミッションを付与しない
- 認証エラーを無視しない
- MCPエンドポイントを公開しない
- root/admin権限でMCPサーバーを実行しない
- ログに機密データをキャッシュしない
- 認証メカニズムを無効化しない

### 設定のベストプラクティス

1. **バージョン管理**: `.mcp.json` をgitに保持するが、シークレットには環境変数を使用
2. **最小権限**: 各MCPサーバーに必要な最小限のパーミッションのみを付与
3. **隔離**: 可能な場合は異なるMCPサーバーを別のプロセスで実行
4. **モニタリング**: 監査証跡のためにすべてのMCPリクエストとエラーをログ記録
5. **テスト**: 本番環境にデプロイする前にすべてのMCP設定をテスト

### パフォーマンスのヒント

- アプリケーションレベルで頻繁にアクセスするデータをキャッシュする
- データ転送を減らすために特定のMCPクエリを使用する
- MCP操作のレスポンスタイムを監視する
- 外部APIのレートリミットを考慮する
- 複数の操作を実行する際はバッチ処理を使用する

## インストール手順

### 前提条件
- Node.jsとnpmのインストール
- Claude Code CLIのインストール
- 外部サービスのAPIトークン/認証情報

### ステップバイステップのセットアップ

1. **最初のMCPサーバーをCLIで追加**（例: GitHub）:
```bash
claude mcp add --transport stdio github -- npx @modelcontextprotocol/server-github
```

   または `.mcp.json` ファイルをプロジェクトルートに作成:
```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

2. **環境変数を設定:**
```bash
export GITHUB_TOKEN="your_github_personal_access_token"
```

3. **接続をテスト:**
```bash
claude /mcp
```

4. **MCPツールを使用:**
```bash
/mcp__github__list_prs
/mcp__github__create_issue "タイトル" "説明"
```

### 特定サービスのインストール

**GitHub MCP:**
```bash
npm install -g @modelcontextprotocol/server-github
```

**データベースMCP:**
```bash
npm install -g @modelcontextprotocol/server-database
```

**Filesystem MCP:**
```bash
npm install -g @modelcontextprotocol/server-filesystem
```

**Slack MCP:**
```bash
npm install -g @modelcontextprotocol/server-slack
```

## トラブルシューティング

### MCPサーバーが見つからない
```bash
# MCPサーバーがインストールされているか確認
npm list -g @modelcontextprotocol/server-github

# インストールされていない場合はインストール
npm install -g @modelcontextprotocol/server-github
```

### 認証失敗
```bash
# 環境変数が設定されているか確認
echo $GITHUB_TOKEN

# 必要に応じて再エクスポート
export GITHUB_TOKEN="your_token"

# トークンに正しいパーミッションがあるか確認
# GitHubトークンのスコープを確認: https://github.com/settings/tokens
```

### 接続タイムアウト
- ネットワーク接続を確認: `ping api.github.com`
- APIエンドポイントにアクセス可能か確認
- APIのレートリミットを確認
- 設定でタイムアウトを増やしてみる
- ファイアウォールやプロキシの問題を確認

### MCPサーバーのクラッシュ
- MCPサーバーのログを確認: `~/.claude/logs/`
- すべての環境変数が設定されているか確認
- 適切なファイルパーミッションを確認
- MCPサーバーパッケージの再インストールを試みる
- 同じポートで競合するプロセスがないか確認

## 関連概念

### メモリとMCP
- **メモリ**: 永続的な変化しないデータを保存（設定、コンテキスト、履歴）
- **MCP**: ライブな変化するデータにアクセス（API、データベース、リアルタイムサービス）

### 使い分け
- **メモリを使う場合**: ユーザー設定、会話履歴、学習済みコンテキスト
- **MCPを使う場合**: 現在のGitHub Issue、ライブデータベースクエリ、リアルタイムデータ

### 他のClaude機能との統合
- MCPとメモリを組み合わせてリッチなコンテキストを実現
- プロンプトのMCPツールをより良い推論のために使用
- 複雑なワークフローのために複数のMCPを活用

## 追加リソース

- [公式MCPドキュメント](https://code.claude.com/docs/en/mcp)
- [MCPプロトコル仕様](https://modelcontextprotocol.io/specification)
- [MCP GitHubリポジトリ](https://github.com/modelcontextprotocol/servers)
- [利用可能なMCPサーバー](https://github.com/modelcontextprotocol/servers)
- [MCPorter](https://github.com/steipete/mcporter) — ボイラープレートなしにMCPサーバーを呼び出すTypeScriptランタイム&CLI
- [Code Execution with MCP](https://www.anthropic.com/engineering/code-execution-with-mcp) — コンテキスト肥大化の解決に関するAnthropicエンジニアリングブログ
- [Claude Code CLIリファレンス](https://code.claude.com/docs/en/cli-reference)
- [Claude APIドキュメント](https://docs.anthropic.com)
