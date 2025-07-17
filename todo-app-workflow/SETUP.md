# セットアップガイド

## ステップ 1: リポジトリのセットアップ

### 1.1 ファイルのコピー

```bash
# ワークフローディレクトリを作成
mkdir -p .github/workflows
mkdir -p .github/ISSUE_TEMPLATE
mkdir -p .claude

# ワークフローファイルをコピー
cp todo-app-workflow/create-todo-app.yml .github/workflows/
cp todo-app-workflow/issue-todo-trigger.yml .github/workflows/

# Issueテンプレート（オプション）
cp todo-app-workflow/.github/ISSUE_TEMPLATE/todo-app-request.md .github/ISSUE_TEMPLATE/

# Claude設定ファイル
cp todo-app-workflow/.claude/settings.json .claude/

# ファイルが正しくコピーされたか確認
ls -la .github/workflows/
ls -la .github/ISSUE_TEMPLATE/
ls -la .claude/
```

### 1.2 ディレクトリ構造確認

```
your-repo/
├── .github/
│   ├── workflows/
│   │   ├── create-todo-app.yml         # メインワークフロー
│   │   └── issue-todo-trigger.yml      # Issue連動ワークフロー
│   └── ISSUE_TEMPLATE/
│       └── todo-app-request.md         # Issueテンプレート
└── .claude/
    └── settings.json                   # Claude Code権限設定
```

## ステップ 2: GitHub Secrets の設定

### 2.1 必要な Secrets

| Secret 名           | 説明                         | 必須/オプション |
| ------------------- | ---------------------------- | --------------- |
| `ANTHROPIC_API_KEY` | Claude API Key               | 必須            |
| `PAT_TOKEN`         | GitHub Personal Access Token | オプション      |

### 2.2 ANTHROPIC_API_KEY の取得方法

1. [Anthropic Console](https://console.anthropic.com/)にアクセス
2. ログインまたはアカウント作成
3. 左側メニューの「API Keys」をクリック
4. 「Create Key」をクリック
5. キー名を入力（例：`github-actions-todo-app`）
6. 作成された API キーをコピー（⚠️ この画面でしか表示されません）

### 2.3 PAT_TOKEN の取得方法（オプション）

1. GitHub にログイン
2. Settings → Developer settings → Personal access tokens → Tokens (classic)
3. 「Generate new token (classic)」をクリック
4. 以下の権限を選択：
   - `repo` (リポジトリへの完全アクセス)
   - `workflow` (GitHub Actions ワークフローの更新)
5. 「Generate token」をクリック
6. 作成されたトークンをコピー（⚠️ この画面でしか表示されません）

### 2.4 Secrets 設定手順

**方法 1: GitHub CLI（推奨・簡単）**

```bash
# カレントディレクトリがリポジトリ内の場合
gh secret set ANTHROPIC_API_KEY --app actions
# ↑ 実行後、APIキーを安全に入力（画面に表示されません）

# PAT_TOKEN（必要に応じて）
gh secret set PAT_TOKEN --app actions

# 設定確認
gh secret list --app actions
```

**方法 2: GitHub Web UI**

1. **GitHub リポジトリページ**にアクセス
2. **Settings**タブをクリック
3. 左サイドバーの**Secrets and variables** → **Actions**をクリック
4. **New repository secret**をクリック
5. 以下を順番に追加：

**ANTHROPIC_API_KEY の追加：**

- **Name**: `ANTHROPIC_API_KEY`
- **Secret**: 先ほどコピーした Claude API キー
- **Add secret**をクリック

**PAT_TOKEN の追加（必要に応じて）：**

- **Name**: `PAT_TOKEN`
- **Secret**: 先ほどコピーした Personal Access Token
- **Add secret**をクリック

### 2.5 設定確認

設定完了後、Secrets ページに以下が表示されることを確認：

- ✅ `ANTHROPIC_API_KEY` (Updated X minutes ago)
- ✅ `PAT_TOKEN` (Updated X minutes ago) ※設定した場合

## ステップ 3: Claude Code 設定

### 3.1 Claude Code 権限設定

`.claude/settings.json` ファイルが正しく配置されていることを確認：

```bash
# 設定ファイルの確認
cat .claude/settings.json
```

**設定内容の説明：**

```json
{
  "defaultMode": "acceptEdits",
  "permissions": {
    "allow": [
      "Bash(curl:*)",
      "Bash(wget:*)",
      "Bash(node:*)",
      "Bash(npm:*)",
      "Bash(jq:*)",
      "Bash(find:*)",
      "Bash(ls:*)",
      "Bash(cat:*)",
      "Bash(mkdir:*)",
      "Bash(echo:*)"
    ]
  }
}
```

**⚠️ 重要**: この設定がないと、Claude Code が Bash コマンドを使用できずワークフローが失敗します。

## ステップ 4: GitHub 権限設定

### 4.1 ワークフロー権限設定

**Settings** → **Actions** → **General** → **Workflow permissions**

- ✅ "Read and write permissions" を選択
- ✅ "Allow GitHub Actions to create and approve pull requests" をチェック

### 4.2 権限設定の確認

以下の権限が有効になっていることを確認：

- ✅ ブランチ作成・変更権限
- ✅ ファイル作成・変更権限
- ✅ プルリクエスト作成権限
- ✅ Issue 読み取り権限

## ステップ 5: 動作確認

### 5.1 手動実行テスト

1. **GitHub Actions**タブに移動
2. **Create TODO App**ワークフローを選択
3. **Run workflow**をクリック
4. 以下の内容でテスト実行：

```
App Requirements: シンプルなタスク管理アプリ、追加・削除・完了機能
App Type: basic-html
```

### 5.2 Issue 連動テスト

1. **Issues**タブで新しい Issue を作成
2. テンプレート「TODO App Generation Request」を選択
3. 以下の内容で作成：

```markdown
@create-todo-app
要件：

- タスクの追加・削除機能
- ローカルストレージでデータ保存
- シンプルな UI

技術：HTML + CSS + JavaScript
```

### 5.3 成功の確認

ワークフローが成功すると：

- ✅ 新しいブランチが作成される
- ✅ 生成されたアプリファイルがコミットされる
- ✅ プルリクエストが自動作成される
- ✅ GitHub Actions Summary に結果が表示される

## トラブルシューティング

### よくあるエラーと対策

**1. `Error: ANTHROPIC_API_KEY not found`**

```bash
# 対策：Secretsの設定を確認
gh secret list --app actions
```

**2. `Permission denied to create branch`**

```bash
# 対策：ワークフロー権限を確認
# Settings → Actions → General → "Read and write permissions"
```

**3. `Claude Code execution failed`**

```bash
# 対策：.claude/settings.json の存在と内容を確認
cat .claude/settings.json
```

**4. `No such file or directory: create-todo-app.yml`**

```bash
# 対策：ワークフローファイルの配置を確認
ls -la .github/workflows/
```

### ログの確認方法

1. **GitHub Actions**タブに移動
2. 失敗したワークフロー実行をクリック
3. 各ジョブのログを展開して確認
4. エラーメッセージから原因を特定

### デバッグのヒント

**ワークフローファイルの構文チェック：**

```bash
# YAML構文の確認
yamllint .github/workflows/create-todo-app.yml
```

**Secrets 設定の確認：**

```bash
# GitHub CLIでSecrets一覧表示
gh secret list --app actions
```

**権限設定の確認：**

```bash
# リポジトリ設定の確認
gh repo view --json defaultBranchRef,permissions
```

## 高度な設定

### カスタムプロンプトの設定

`create-todo-app.yml`の`PROMPT`変数を編集して、生成されるアプリをカスタマイズできます：

```yaml
PROMPT="あなたは経験豊富なフルスタック開発者です。
[カスタムプロンプトをここに記述]"
```

### 新しいアプリタイプの追加

1. `create-todo-app.yml`の`app_type`選択肢に追加
2. `code-generation`ジョブに新しいケースを追加
3. 対応するプロンプトを作成

### 品質検証の強化

`quality-assurance`ジョブに追加の検証ロジックを実装：

```yaml
- name: 追加品質検証
  run: |
    # ESLint実行
    npx eslint $APP_DIR --ext .js,.jsx,.ts,.tsx

    # セキュリティ検証
    npx audit-ci --config audit-ci.json
```

## サポート

問題が解決しない場合：

1. [GitHub Issues](https://github.com/your-repo/issues)で質問
2. ワークフローログを添付
3. 実行環境の詳細を記載

---

🛠️ **Setup Guide** - AI TODO App Generator Workflow
