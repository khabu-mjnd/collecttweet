# collecttweet

このプロジェクトは、Twitter API を使用して特定のキーワードでツイートを収集し、CSV形式で保存するスクリプトです。具体的には、指定した日時範囲内で指定したキーワードを含むツイートを収集し、そのデータをローカルに保存します。

## 概要

- Twitter APIを使用して、指定したキーワードを含むツイートを収集します。
- `BEARER_TOKEN` は環境変数として `.env` ファイルから読み込まれます。
- ツイートの情報（テキスト、作成日時、リツイート数、いいね数など）を収集します。
- 収集したデータは CSV ファイルとして保存されます。

# 想定環境
櫻井研サーバー`dual-rtx-geoffrey`内にある`/home/sakulab/workspace/All_collect_tweet/Forcoltweet`内にある`ipynb`ファイルに同様のものがあります.

## 設定

### 1. `.env` ファイル

プロジェクトのルートディレクトリに `.env` ファイルを作成し、以下の内容を追加してください.：

```
BEARER_TOKEN=your_bearer_token_here
```

`your_bearer_token_here` を実際の Bearer トークンに置き換えてください。Bear Token は Twitter Developer アカウントから取得できます。

#### 1.1 `.env` ファイルが原因で動かない場合

ipynbファイル内で、`BEARER_TOKEN=`をベタ打ちで直接入れれば動くと思います.

### 2. `pyproject.toml`

依存関係は `pyproject.toml` にも記載されています。想定環境下にある`pyproject.toml` には以下の内容が含まれています。

```toml
[project]
name = "forcoltweet"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "dotenv>=0.9.9",
    "ipykernel>=6.29.5",
    "jupyter>=1.1.1",
    "numpy>=2.3.1",
    "pandas>=2.3.0",
    "tqdm>=4.67.1",
]
```

## 使用方法

スクリプトを実行することで、指定されたキーワードを含むツイートを収集し、CSV形式で保存することができます。

### 1. スクリプトの実行

実行後、収集されたデータは `collected_tweet` フォルダに `tweets_allinfo_<start_time>_to_<end_time>.csv` という名前で保存されます。

### 2. パラメータ設定

以下のパラメータを設定してください。

* **`START_TIME`**: 収集開始日時（日本標準時 JST）
* **`END_TIME`**: 収集終了日時（日本標準時 JST）
* **`QUERY`**: 検索するキーワード（例：`"日経平均 -is:retweet"`）
* **`MAX_RESULTS`**: 1リクエストで取得する最大ツイート数
* **`MAX_PAGES`**: 最大で何回リクエストを送るかの上限

例：

```python
START_TIME = datetime(2023, 11, 1, 0, 0, 0)  # 取得開始日
END_TIME = datetime(2023, 11, 1, 23, 59, 59)  # 取得終了日
QUERY = "日経平均 -is:retweet"  # 抽出するツイートのキーワード
```

### 3. 出力ファイル

ツイートデータは以下の内容で CSV ファイルとして保存されます：

* **`text`**: ツイート本文
* **`created_at`**: ツイートの投稿日時（UTC）
* **`created_at_jst`**: ツイートの投稿日時（JST）
* **`tweet_id`**: ツイートの ID
* **`author_id`**: ツイートの投稿者 ID
* **`retweet_count`**: リツイート数
* **`reply_count`**: リプライ数
* **`like_count`**: いいね数
* **`quote_count`**: 引用ツイート数
* **`followers_count`**: 投稿者のフォロワー数
* **`username`**: 投稿者のユーザー名
* **`user_location`**: 投稿者の位置情報（ある場合）
* その他、メディア情報や場所情報も含まれます。

