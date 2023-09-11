# postman

APIのレスポンスを確認するPostmanの使い方

[はじめてでも怖くない！　Postman を使って、Web API を触ってみよう！ \- Morning Girl](https://kageura.hatenadiary.jp/entry/hazimetepostman)

## Postmanの概要

APIのレスポンスのチェックツール

### 機能

- API Client
  - 公開されているAPIのテスト
- Automated Testing
  - API開発者向け
  - エンドポイントに対する自動テスト
- Design & Mock
  - API Schema用のエディタ
  - Mock Server機能
- Documentation
  - API定義から自動でドキュメントを作成
- Monitors
  - APIの定期チェック
- Workspaces
  - チームでのAPI開発のコラボ機能

通常使用するのはAPI Client機能だけっぽい

### インストール

以下の2つがある

- ブラウザ版
- クライアントアプリ版

- https://www.postman.com/downloads/
- アプリ版をインストールした

## Web APIについて

### Web APIに必要なもの

以下の7つが必要

1. Base URI
2. Resource
3. Method
4. Query
5. Authentication
6. Header
7. Body

APIとして公開されているものはRESTで設計されているものが大半を占める

#### 1. Base URI

- エンドポイント
- バージョン名が含まれることもある
- `https://qiita.com/api/v2/items`

#### 2. Resource

- データ本体
- 例) `https://qiita.com/api/v2/items` の`items` の部分

#### 3. Method

- HTTPメソッド
- `GET`、`POST`、`PUT`、`DELETE`等
- サポートしているメソッドはサービス毎に変化する
- 使えるメソッドはドキュメントで確認が必要

#### 4. Query

- URL末尾のパラメータ
- Query String(クエリ文字列)
- URIの末尾の`?`以降にしていする文字列
- APIを触る前にサポートしているQueryを確認する必要がある

例) `$パラメータ名=値` で指定

```
OReillyBookList?$select=Price,Title&$filter=Price le 2000&$orderby=Price&$top=5
```

#### 5. Authentication

- 認証部分
- 認証方法はAPI毎に様々
  - Basic認証
  - OAuth
- 認証に必要なTokenはHeaderかQueryで指定することが多い

#### 6. Header

- APIへの追加情報を渡すためのプロパティ
  - 認証情報
  - フォーマット形式の指定(JSON等)

```
GET decodeapiserverdemo.azurewebsites.net/api.rsc/OReillyBookList
Accept: application/json
Content-Type: application/json
Authorization: Bearer XXXXXXXXXX
```

#### 7. Body

- メインデータ
- POST等のメソッドで送信する値
- APIによってフォーマットが異なる
  - XML
  - CSV
- フォーマットはドキュメントを確認する

```
POST http://decodeapiserverdemo.azurewebsites.net/api.rsc/OReillyBookList/ HTTP/1.1
Content-Type: application/json
x-cdata-authtoken: MY_AUTH_TOKEN

{
    "RowId": 1,
    "ImageUrl": "http://hogehoge/image.jpg",
    "ISBN": "43413241243",
    "Title": "New Book"
}
```

## Postmanの使い方

1. `Collections` にフォルダーを作り、フォルダー内にリクエストを作成する
2. BaseURIを`Enter URL or past text` に入力
  - `http://decodeapiserverdemo.azurewebsites.net/api.rsc/`
3. ResourceをURIの末尾に入力
  - `http://decodeapiserverdemo.azurewebsites.net/api.rsc/OReillyBookList`
4. Methodをプルダウンで選択
  - `GET`、`POST`等
5. Queryを`Params` タブ内の`Query Params` の表に入力
  - `Key` にキー
  - `Value` に値
6. Authorizationを`Auth` タブで設定
  - `TYPE` で認証方式を選択
    - Basic認証: `Basic Auth`
    - OAuth: `OAuth 1.0` or `OAuth 2.0`
  - 認証で使用する値の入力
    - 例) `Username`、`Password`
7. Headerを`Headers` タブで選択
  - 取得: `KEY` に`Accept`、`VALUE` に`application/json` を指定
  - 送信: `KEY` に`Content-Type` 、`VALUE` に`application/json` を指定?
    - `Content-Type` で良いのかは未確認
  
JSON形式の場合は実際のHeaderの内容が
以下のようになるようにする
```
Accept: application/json
Content-Type: application/json
```

上記全ての設定が終わったら`Send` ボタンをクリックする

## RailsアプリでAPIを試す

[karlley/rails\_api: RailsのAPIモードを使った簡易アプリ](https://github.com/karlley/rails_api)

[RailsのAPIのレスポンスをcurlコマンドで確認する \- karlley](https://scrapbox.io/karlley/Rails%E3%81%AEAPI%E3%81%AE%E3%83%AC%E3%82%B9%E3%83%9D%E3%83%B3%E3%82%B9%E3%82%92curl%E3%82%B3%E3%83%9E%E3%83%B3%E3%83%89%E3%81%A7%E7%A2%BA%E8%AA%8D%E3%81%99%E3%82%8B)

- リポジトリをclone
- 対象ディレクトリに移動して`rails s`
- `http://localhost:3000/` にアクセスして起動を確認

### GET

Postmanに以下を設定して`Send` ボタン

1. Base URI: `http://localhost:3000/api/v1/`
2. Resource: `posts`
3. Method: `GET`
4. Query: 無し
5. Authentication: 無し
6. Header:
  - `Key`: `Accept`
  - `Value`: `application/json`
7. Body: 無し

### POST

Postmanに以下を設定して`Send` ボタン

1. Base URI: `http://localhost:3000/api/v1/`
2. Resource: `posts`
3. Method: `POST`
4. Query:
  - `Key`: `title`
  - `Value`: `PostmanからPOSTしたよ👩‍🔧`
5. Authentication: 無し
6. Header:
  - `Key`: `Accept`
  - `Value`: `application/json`
7. Body: 無し

POSTはなぜか上手くいかなかった

## GitHub APIでPOSTを試す

[「Postman」を使用して初めてのREST APIテスト \| Sqripts](https://sqripts.com/2022/11/30/22135/)

[リポジトリ \- GitHub Docs](https://docs.github.com/ja/rest/repos/repos?apiVersion=2022-11-28#create-a-repository-for-the-authenticated-user)

### 事前準備

1. GitHubログイン
2. Token発行
  - `Settings` > `Developer settings`
  - `Taken(classic)` を選択
  - `Generate new token` > `Genereta new token (classic)`
  - `Note` にトークン名を入力
  - `Select scopes` 内の`public_repo` にチェック
  - `Generate token` ボタンをクリック
  - 生成されたトークンをコピー
  - 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
3. 

### ワークスペース内にリクエスト作成

- `My Workspace` の`API test`フォルダーを開く
- `API test` フォルダーを開いた状態で`+` ボタンでリクエストを作成
- 新規でワークスペースを作成する場合はの`Create Collection` を選択、`Add a request` でリクエストを作成

### リクエスト作成

#### メソッドとエンドポイントを指定

- メソッドは`POST` を選択
- `Enter URL or paste text` にエンドポイントを入力
  - `https://api.github.com/user/repos`

#### 認証用トークン設定

- `Auth` タブ内の`Type` を`Bearer Token` を選択
  - 認証方式によって変化する
- `Token` にGitHubで作成したトークンを入力

#### Header設定

APIドキュメントを参照し、`Header` タブ内にクライアントが処理できるメディアタイプを設定

- `KEY`: `accept`
- `VALUE`: `application/vnd.github+json`

#### Body設定

送信するデータを`Body` タブ内にJSON形式で設定

- 左のプルダウンで`raw` を選択
- 右のプルダウンで`JSON` を選択
- 各パラメータの仕様はAPIドキュメントを参照する

```JSON
{
    "name":"Postman Test",
    "description":"This is Postman Test repository",
    "homepage":"https://github.com",
    "private":false,
    "has_issues":true,
    "has_projects":true,
    "has_wiki":true
}
```

#### リクエスト送信

リクエスト情報を入力後、`Send` ボタンをクリック

- リクエストに成功すると`201 Created` が返る
- 成功するとGitHubにリポジトリが作成される
