---
title: Firebase HostingにNext.jsプロジェクトをデプロイ
tags:
  - Firebase
  - Next.js
private: false
updated_at: '2024-05-13T07:48:31+09:00'
id: 0c5d43e1cf96f5930fb0
organization_url_name: null
slide: false
ignorePublish: false
---
# はじめに
この記事ではFirebase HostingにNext.jsプロジェクトをデプロイし、初期表示画面が出るまでを行います。

##### この記事で取り扱わないこと
- Firebaseへのアカウント登録方法

##### 動作環境
MacOS Ventura 13.5
Node v21.6.2

# 目次
1. [Next.jsプロジェクトを作成する](#Next.jsプロジェクトを作成する)
1. [Firebaseにプロジェクトを作成する](#Firebaseにプロジェクトを作成する)
1. [Firebaseにデプロイする](#Firebaseにデプロイする)
1. [参考文献](#参考文献)

# Next.jsプロジェクトを作成する
Next.jsのプロジェクトを作成します。
`npm run dev`を実行後[http://localhost:3000](http://localhost:3000)を確認し、Next.jsの初期ページが表示されればOKです。
```shell
$ npx create-next-app@latest
$ npm i

$ npm run dev
```
Next.jsの初期ページ
![Create_Next_App.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1245466/21f5866c-18bf-6330-42fa-a42ac6a8a796.png)

# Firebaseにプロジェクトを作成する
[Firebaseのコンソール](https://console.firebase.google.com/u/0/?hl=ja)にアクセスします。

プロジェクトを追加をクリックします。
![Firebase_コンソール.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1245466/300ed440-9acd-61b8-3ce9-5e7368ac07e0.png)

プロジェクト名を入力します。
![プロジェクトの作成_-_Firebase_コンソール.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1245466/63804f11-c4f5-89a0-e9a1-4d0d2f25f190.png)

続行を選択します。
![プロジェクトの作成_-_Firebase_コンソール-2.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1245466/c65fab9f-0210-cf1f-d34a-e413e300ce71.png)

プロジェクトを作成をクリックします。
これで少し待つとプロジェクトが作成されます
![プロジェクトの作成_-_Firebase_コンソール-3.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1245466/4dd962d4-dcf9-b014-6fb8-508df6d23872.png)

# Firebaseにデプロイする
firebase-toolsをインストールし、ログインします。
`firebase login`実行後、Googleログイン画面に遷移するのでご自身のアカウントでログインしてください。
```shell
$ npm install -g firebase-tools
$ firebase login
```

Firebaseにデプロイするためのセッティングを行います。
実行すると質問されるので、以下のように選択してください。
最後に`Firebase initialization complete!`と出力されればOKです。
```
$ firebase init hosting
? Please select an option: Use an existing project
? Select a default Firebase project for this directory: {先ほど作ったプロジェクトを選択}
? What do you want to use as your public directory? out
? Configure as a single-page app (rewrite all urls to /index.html)? No
? Set up automatic builds and deploys with GitHub? Yes
✔  Wrote out/404.html
✔  Wrote out/index.html

・・・・・
✔  Firebase initialization complete!
```

exportの設定を追加します。
```js:next.config.mjs
/** @type {import('next').NextConfig} */
const nextConfig = {
    output: 'export'
};

export default nextConfig;
```

コードをビルドし、Firebaseにデプロイします。
`firebase deploy --only hosting`の実行が終わると、`Hosting URL`が出力されるので、それにアクセスするとデプロイしたものを確認することができます。
```shell
$ npm run build
$ firebase deploy --only hosting
・・・・
Hosting URL: https://test1-c5bdc.web.app
```

# 参考文献
- [Firebase に Next.js のアプリケーションを Hosting する](https://qiita.com/takiguchi-yu/items/5d22048b578bd311b996)

