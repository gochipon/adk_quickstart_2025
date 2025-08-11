# Multi Tool Agent

このプロジェクトは、Google ADKを使用したマルチツールエージェントです。天気情報と時刻情報を提供するエージェントが実装されています。

## セットアップ手順

### 1. 仮想環境の作成と有効化

```bash
# 仮想環境の作成
python -m venv .venv

# 仮想環境の有効化（macOS/Linux）
source .venv/bin/activate

# 仮想環境の有効化（Windows）
# .venv\Scripts\activate
```


### 2. 依存関係のインストール

```bash
# requirements.txtから依存関係をインストール
pip install -r requirements.txt
```

### 3. 環境設定

#### 3.1 環境変数の設定

`multi_tool_agent/.env`ファイルを手動で作成し、以下の内容を記載してください：

```bash
# Google AI API設定
GOOGLE_GENAI_USE_VERTEXAI=false
GOOGLE_API_KEY=your_google_api_key_here
```

**作成手順：**
```bash
# .envファイルを作成
touch multi_tool_agent/.env

# エディタで編集（例：vim、nano、VS Codeなど）
nano multi_tool_agent/.env
```

#### 3.2 Google API キーの取得

1. [Google AI Studio](https://aistudio.google.com/)にアクセス
2. 「Get API key」をクリック
3. 新しいAPIキーを作成
4. 作成されたAPIキーを`.env`ファイルの`GOOGLE_API_KEY`に設定

#### 3.3 注意事項

- `.env`ファイルはGitで管理されません（`.gitignore`に含まれています）
- APIキーは秘密情報のため、他人と共有しないでください
- 本番環境では適切な権限管理を行ってください

### 4. サービスの実行

```bash
# エージェントの実行
python -c "from multi_tool_agent.agent import root_agent; print('Agent initialized:', root_agent.name)"
```

## 機能

- **天気情報取得**: 指定された都市（現在はNew Yorkのみ対応）の天気情報を取得
- **時刻情報取得**: 指定された都市（現在はNew Yorkのみ対応）の現在時刻を取得

## 使用方法

エージェントは以下のツールを提供します：

1. `get_weather(city: str)` - 都市の天気情報を取得
2. `get_current_time(city: str)` - 都市の現在時刻を取得

## 注意事項

- 現在はNew Yorkの情報のみ対応しています
- 実際の天気データではなく、サンプルデータを返します
- Google ADKの認証設定が必要です
