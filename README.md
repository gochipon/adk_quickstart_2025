# Multi Tool Agent

このプロジェクトは、Google ADKを使用したマルチツールエージェントです。天気情報と時刻情報を提供するエージェントが実装されています。

## クイックスタート

uvを使用した最速の起動方法：

```bash
# 1. uvのインストール（まだの場合）
curl -LsSf https://astral.sh/uv/install.sh | sh

# 2. 依存関係のインストール
uv sync

# 3. .envファイルの作成（APIキーを置き換えてください）
cat << EOF > multi_tool_agent/.env
GOOGLE_GENAI_USE_VERTEXAI=false
GOOGLE_API_KEY=your_google_api_key_here
EOF

# 4. adkのバージョンを1.18以上にする
uv add google-adk==1.18.0 

# 5. サービスの起動
uv run adk web --reload_agents
```

http://127.0.0.1:8000 にアクセスできるかを確認してください。

---

## セットアップ手順（方法1：uvを使用）**推奨**

uvは高速で、依存関係の管理が簡単なPythonパッケージマネージャーです。

### ステップ1: uvのインストール

```bash
# macOS/Linux
curl -LsSf https://astral.sh/uv/install.sh | sh

# または、Homebrewを使用する場合
brew install uv
```

### ステップ2: 依存関係のインストール

```bash
# プロジェクトのルートディレクトリで実行
uv sync
```

このコマンドは以下を自動的に行います：
- `.venv`ディレクトリに仮想環境を作成（存在しない場合）
- `pyproject.toml`に記載された依存関係をインストール
- `uv.lock`ファイルを生成（初回実行時のみ。既に存在する場合はそのロックファイルに基づいてインストール）

**注意**: `pyproject.toml`を変更した場合は、`uv lock`コマンドでロックファイルを更新してから`uv sync`を実行してください。

### ステップ3: 環境変数の設定

#### 3.1 Google API キーの取得

Google API キーは講師から提供されます。提供されたAPIキーを次の手順で設定してください。

#### 3.2 .envファイルの作成

`multi_tool_agent/.env`ファイルを作成し、以下の内容を記載してください：

```bash
cat << EOF > multi_tool_agent/.env
# Google AI API設定
GOOGLE_GENAI_USE_VERTEXAI=false
GOOGLE_API_KEY=your_google_api_key_here
EOF
```

**注意事項：**
- `.env`ファイルはGitで管理されません（`.gitignore`に含まれています）
- APIキーは秘密情報のため、他人と共有しないでください
- 本番環境では適切な権限管理を行ってください

### ステップ4: サービスの実行

```bash
# uvを使ってエージェントを実行
uv run adk web
```

`uv run`を使用すると、自動的に仮想環境内でコマンドが実行されます。仮想環境の有効化は不要です。

http://127.0.0.1:8000 にアクセスできるかを確認してください。

---

## セットアップ手順（方法2：venv + pipを使用）

従来のpipとvenvを使用した方法です。

### ステップ1: 仮想環境の作成と有効化

```bash
# 仮想環境の作成（環境によっては、python -m venv .venvになるかも）
python3 -m venv .venv

# 仮想環境の有効化（macOS/Linux）
source .venv/bin/activate

# 仮想環境の有効化（Windows）※未検証
.venv\Scripts\activate
```

### ステップ2: 依存関係のインストール

```bash
# requirements.txtから依存関係をインストール
pip install -r requirements.txt
```

### ステップ3: 環境変数の設定

#### 3.1 Google API キーの取得

Google API キーは講師から提供されます。提供されたAPIキーを次の手順で設定してください。

#### 3.2 .envファイルの作成

`multi_tool_agent/.env`ファイルを作成し、以下の内容を記載してください：

```bash
cat << EOF > multi_tool_agent/.env
# Google AI API設定
GOOGLE_GENAI_USE_VERTEXAI=false
GOOGLE_API_KEY=your_google_api_key_here
EOF
```

**注意事項：**
- `.env`ファイルはGitで管理されません（`.gitignore`に含まれています）
- APIキーは秘密情報のため、他人と共有しないでください
- 本番環境では適切な権限管理を行ってください

### ステップ4: サービスの実行

一度、venvをdeactivateし、再度仮想環境を有効化する（この手順が不要な環境もあります）：

```bash
deactivate
source .venv/bin/activate
```

エージェントを実行：

```bash
adk web
```

http://127.0.0.1:8000 にアクセスできるかを確認してください。

---

## 機能

- **天気情報取得**: 指定された都市（現在はNew Yorkのみ対応）の天気情報を取得
- **時刻情報取得**: 指定された都市（現在はNew Yorkのみ対応）の現在時刻を取得

## 使用方法

エージェントは以下のツールを提供します：

1. `get_weather(city: str)` - 都市の天気情報を取得
2. `get_current_time(city: str)` - 都市の現在時刻を取得

音声でのやり取りを行いたい場合、`multi_tool_agent/agent.py`の`root_agent`作成時に利用するモデルを音声対応モデル（例: `gemini-2.0-flash-live-001`）へ変更してください。

---

## uvの便利なコマンド

uvを使用している場合、以下のコマンドが利用できます。

### 新しいパッケージを追加

```bash
# パッケージを追加してpyproject.tomlとuv.lockを自動更新
uv add <package_name>

# 例: pandasを追加
uv add pandas
```

`uv add`は自動的にpyproject.tomlに依存関係を追加し、uv.lockを更新して、パッケージをインストールします。

### パッケージを削除

```bash
# パッケージを削除してpyproject.tomlとuv.lockを自動更新
uv remove <package_name>

# 例: pandasを削除
uv remove pandas
```

### ロックファイルの更新

```bash
# pyproject.tomlを変更した後、ロックファイルを更新
uv lock
```

### 依存関係の再同期

```bash
# uv.lockに基づいて依存関係を同期
uv sync
```

`uv sync`は既存の`uv.lock`に基づいて環境を同期します。新しいパッケージを追加した場合は`uv add`を使用してください。

### Pythonスクリプトの実行

```bash
# uvを使ってPythonスクリプトを実行
uv run python script.py

# または、任意のコマンドを実行
uv run <command>
```

### uvの利点

- **高速**: pipより10-100倍高速なインストール
- **自動管理**: 仮想環境とロックファイルの自動管理
- **シンプル**: `uv run`で毎回仮想環境を有効化する必要がない
- **再現性**: `uv.lock`により、全ての環境で同じ依存関係を保証

---

## 注意事項

- 現在はNew Yorkの情報のみ対応しています
- 実際の天気データではなく、サンプルデータを返します
- Google ADKの認証設定が必要です
