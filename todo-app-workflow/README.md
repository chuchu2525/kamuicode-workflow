# AI TODO App Generator Workflow

Claude Code SDK を使用した AI 駆動の TODO アプリ自動生成ワークフロー

## 🎯 概要

この GitHub Actions ワークフローは以下を自動実行します：

1. **要件分析** - ユーザー要求を分析して技術仕様を作成
2. **コード生成** - HTML/CSS/JavaScript または React/Node.js でアプリ生成
3. **品質検証** - 構文チェック・動作確認・セキュリティ検証
4. **プルリクエスト作成** - 生成されたアプリを PR として提出

## 🚀 使用方法

### 方法 1: 手動実行

1. GitHub Actions → `Create TODO App` を選択
2. `Run workflow` をクリック
3. アプリ要件を入力して実行

### 方法 2: Issue 経由

1. 新しい Issue を作成
2. テンプレート「TODO アプリ生成リクエスト」を選択
3. 要件を記入して送信

### 方法 3: コメントでトリガー

```
@create-todo-app
要件：シンプルなタスク管理、ローカルストレージ使用
技術：HTML + CSS + JavaScript
```

## 📊 生成可能なアプリタイプ

### 🌟 Basic (HTML + CSS + JS)

- **技術スタック**: バニラ JavaScript
- **データ保存**: ローカルストレージ
- **特徴**: シンプル、軽量、すぐに動作
- **適用場面**: プロトタイプ、学習用、シンプルなタスク管理

### ⚡ React (React + TypeScript)

- **技術スタック**: React 18 + TypeScript + Vite
- **データ保存**: ローカルストレージ + Context API
- **特徴**: モダンなコンポーネント設計、型安全性
- **適用場面**: 中規模アプリ、チーム開発、拡張性重視

### 🚀 Full Stack (React + Node.js + DB)

- **技術スタック**: React + Node.js + Express + MongoDB
- **データ保存**: データベース + REST API
- **特徴**: 本格的な Web アプリケーション
- **適用場面**: 本番環境、マルチユーザー、高機能

## 📁 出力構造

```
todo-app-[timestamp]/
├── app/                    # 生成されたアプリケーション
│   ├── index.html         # メインHTML (Basic)
│   ├── style.css          # スタイルシート
│   ├── script.js          # JavaScript機能
│   ├── src/               # Reactソースコード (React/Full Stack)
│   ├── backend/           # バックエンドコード (Full Stack)
│   └── README.md          # アプリの使用方法
├── docs/                   # 技術仕様書
│   ├── requirements.md    # 機能要件・非機能要件
│   ├── technical-spec.md  # 技術仕様書
│   ├── file-structure.md  # ファイル構造設計
│   └── implementation-plan.md # 実装計画
├── tests/                  # テストファイル (React/Full Stack)
└── deployment/             # デプロイ設定 (Full Stack)
    ├── Dockerfile
    └── docker-compose.yml
```

## 🛠️ セットアップ

### 必要な環境

- GitHub リポジトリ
- Claude API Key (Anthropic)
- GitHub Actions 有効化

### セットアップ手順

1. **ファイルのコピー**

```bash
# ワークフローファイルをコピー
cp todo-app-workflow/create-todo-app.yml .github/workflows/
cp todo-app-workflow/issue-todo-trigger.yml .github/workflows/

# Issueテンプレートをコピー（オプション）
cp todo-app-workflow/.github/ISSUE_TEMPLATE/todo-app-request.md .github/ISSUE_TEMPLATE/

# Claude設定をコピー
cp todo-app-workflow/.claude/settings.json .claude/
```

2. **GitHub Secrets 設定**

```bash
# GitHub CLI使用の場合
gh secret set ANTHROPIC_API_KEY --app actions
gh secret set PAT_TOKEN --app actions  # オプション
```

3. **権限設定**

- Settings → Actions → General → Workflow permissions
- "Read and write permissions" を選択
- "Allow GitHub Actions to create and approve pull requests" をチェック

## 💡 使用例

### Basic HTML アプリの生成

```
要件：
- タスクの追加・削除・編集機能
- 完了状態の切り替え
- ローカルストレージでデータ保存
- レスポンシブデザイン
- ダークモード対応

技術：HTML + CSS + JavaScript
```

### React アプリの生成

```
要件：
- モダンなUI/UX
- カテゴリ別タスク管理
- 期限設定機能
- 検索・フィルタ機能
- TypeScript使用

技術：React + TypeScript + Vite
```

### Full Stack アプリの生成

```
要件：
- ユーザー認証機能
- マルチユーザー対応
- リアルタイム同期
- REST API
- データベース永続化

技術：React + Node.js + Express + MongoDB
```

## 🔧 カスタマイズ

### プロンプトのカスタマイズ

`create-todo-app.yml` の `PROMPT` 変数を編集して、生成されるアプリの特徴を調整できます。

### 新しいアプリタイプの追加

1. `create-todo-app.yml` の `app_type` choices に新しいオプションを追加
2. `code-generation` ジョブに新しいケースを追加
3. 対応するプロンプトを作成

### 品質検証の強化

`quality-assurance` ジョブに追加の検証ロジックを実装できます。

## 🐛 トラブルシューティング

### よくある問題

**1. ワークフローが起動しない**

- Secrets が正しく設定されているか確認
- ワークフローファイルがデフォルトブランチにあるか確認

**2. コード生成が失敗する**

- Claude API Key の有効性を確認
- プロンプトが複雑すぎないか確認
- 生成ファイル数の制限を確認

**3. 品質検証でエラー**

- 生成されたコードの構文を手動確認
- 依存関係の問題がないか確認

**4. PR 作成が失敗する**

- PAT_TOKEN または GITHUB_TOKEN の権限を確認
- リポジトリの権限設定を確認

### ログの確認方法

1. GitHub Actions → 失敗したワークフロー実行を選択
2. 各ジョブのログを確認
3. エラーメッセージから原因を特定

## 📚 参考資料

- [Claude Code SDK Documentation](https://github.com/anthropics/claude-code)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Anthropic API Documentation](https://docs.anthropic.com/)

## 📄 ライセンス

MIT License

## 👥 貢献

Issues、Pull Requests 大歓迎です！

新しいアプリタイプの追加、品質向上、ドキュメント改善など、どんな貢献でも歓迎します。

---

🤖 **AI TODO App Generator Workflow** - Powered by [Claude Code SDK](https://github.com/anthropics/claude-code)
