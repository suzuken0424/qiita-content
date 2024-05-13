---
title: Swagger Editor 表記法まとめ
tags:
  - Swagger-editor
private: false
updated_at: '2021-05-07T11:54:22+09:00'
id: 20ce2951ef9946470ff6
organization_url_name: null
slide: false
ignorePublish: false
---
#はじめに
自分がswagger editorを使ってみて、わかった表記法を残します。
構成として、jsonでの表示をのせ、その書き方を記述します。
コードを記載しておきますが、Swaggerはインデントが違うだけでエラーを吐くので、インデントには気をつけてください。
##基本
#####オブジェクト
```json:response
{
  "message": "success",
  "uid": 1234
}
```

以上のレスポンスを得たい場合、下記のように書く

```
        200:
          description: "成功時のレスポンス"
          schema:
            type: "object"
            properties:
              message:
                type: "string"
                example: "success"
              uid:
                type: "integer"
                example: 1234
```
***
#####リスト
```json:response
{
  "id": [
    3,
    4,
    5,
    6
  ]
}
```

以上のレスポンスを得たい場合、下記のように書く

```
        200:
          description: "成功時のレスポンス"
          schema:
            type: "object"
            properties:
              id:
                type: "array"
                items:
                  type: "integer"
                example:
                 - 3
                 - 4
                 - 5
                 - 6
```
##応用
#####オブジェクトの中にリスト
```json:response
[
  {
    "uid": 1,
    "name": "田中"
  }
]
```
以上のレスポンスを得たい場合、下記のように書く

```
        200:
          description: "成功時のレスポンス"
          schema:
            type: "array"
            items:
              type: "object"
              properties:
                uid:
                  type: "integer"
                  format: "int64"
                  example: 1
                name:
                  type: "string"
                  example: "田中"
```
***
#####オブジェクトの中にオブジェクト

```json:response
{
  "id": 1002983,
  "category": {
    "id": 1,
    "name": "田中"
  }
}
```
以上のレスポンスを得たい場合、下記のように書く

```
        200:
          description: "成功時のレスポンス"
          schema:
            type: "object"
            properties:
              id:
                type: "integer"
                format: "int64"
                example: 1002983
              category:
                type: "object"
                properties:
                  id:
                    type: "integer"
                    format: "int64"
                    example: 1
                  name:
                    type: "string"
                    example: "田中"
```
