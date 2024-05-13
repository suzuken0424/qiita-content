---
title: javascriptで始めるAtocder
tags:
  - JavaScript
  - AtCoder
private: false
updated_at: '2024-05-12T10:04:48+09:00'
id: d7c35fd513905d5e0fef
organization_url_name: null
slide: false
ignorePublish: false
---
# はじめに
この記事ではJavascriptでAtcoderを始める方向けに、問題を効率よく解くための環境構築方法を紹介します。
この環境構築を行うことで、以下のことがコマンド１つで可能になります。

- テストデータでの自動テスト
- コマンドライン上からの回答の提出

##### 動作環境
MacOS Ventura 13.5

# 目次
1. [ojコマンドを使えるようにする](#ojコマンドを使えるようにする)
1. [atcode-cliを使えるようにする](#atcode-cliを使えるようにする)
1. [テンプレートを追加する](#テンプレートを追加する)
1. [使い方](#使い方)
1. [コマンド一覧](#コマンド一覧)
1. [参考文献](#参考文献)


<a id="#Chapter1"></a>
# ojコマンドを使えるようにする
ojコマンドをインストールします。
```
$ pip3 install online-judge-tools
```
ojコマンドのバージョンを出力し、コマンドが使えるかを確認します。
`online-judge-tools 11.5.1`のようにバージョンが出力されていればOKです。
```
$ oj --version
```
もし`zsh: command not found: oj`のようなエラーが出た場合は、以下の手順を試してみてください。

まずはojのファイル位置を確認します。
```
$ find ~ -name oj 2> /dev/null
```
出力された文字列に`--version`をつけて実行し、実行できるかを確認します。
出力が`/Users/test/Library/Python/3.9/bin/oj`の場合は以下のように実行します。
この時`online-judge-tools 11.5.1`のようにバージョンが出力されていればOKです。
```
$ /Users/test/Library/Python/3.9/bin/oj --version
```
このままでは使いづらいので、`oj`で実行できるようにします。
`.zshrc`にパスを記載します。（筆者の環境でzshを使っていますが、bashなどをお使いの方は.bashrcなど適宜書き込み場所を変えてください）
※ 最後の`/oj`は消してください
```
# ojファイルのパスが /Users/test/Library/Python/3.9/bin/oj の場合
export PATH="/Users/test/Library/Python/3.9/bin:$PATH"
```

<a id="#Chapter2"></a>
# atcode-cliを使えるようにする
atcode-cliコマンドをインストールします。
```
$ npm install -g atcoder-cli
```
accコマンドのバージョンを出力し、コマンドが使えるかを確認します。
`2.2.0`のようにバージョン番号が出力されればOKです。
```
$ acc --version
```

<a id="#Chapter3"></a>
# テンプレートを追加する
以下のコマンドを実行し、config.jsonの場所を確認します。
```
$ acc config-dir
```
この時出力されたディレクトリに移動します。
筆者の場合は`/Users/test/Library/Preferences/atcoder-cli-nodejs`だったので以下を実行しました。
```
$ cd /Users/test/Library/Preferences/atcoder-cli-nodejs
```
移動先にある`config.json`を以下のように編集します。
```json
{
	"oj-path": "/Users/test/Library/Python/3.9/bin/oj", //このパスは先ほどのojのパス
	"default-contest-dirname-format": "{ContestID}",
	"default-task-dirname-format": "{tasklabel}",
	"default-test-dirname-format": "test",
	"default-task-choice": "inquire",
	"default-template": "js"
}
```
jsディレクトリを作成し、`main.js`と`template.json`ファイルを作成します。
```
$ mkdir js
$ cd js
$ touch main.js
$ touch template.json
```
`main.js`に以下を追記します。
```js
"use strict";

const main = arg => {
    const input = arg.trim().split("\n");
}

main(require("fs").readFileSync("/dev/stdin", "utf8"));
```
`template.json`に以下を追記します。
```json
{
    "task": {
        "program": [
            "main.js"
        ],
        "submit": "main.js"
    }
}
```

<a id="#Chapter4"></a>
# 使い方
今回はAtCoder Beginner Contest 314に参加する想定でコマンドを実行していきます。
[AtCoder Beginner Contest 314](https://atcoder.jp/contests/abc314)

まずは参加するコンテストコードを確認します。
ブラウザ上でコンテストページに移動し、URLの`contests/`以下がコンテストコードです。
今回の場合だと`abc314`がコンテストコードだとわかります。
![AtCoder：公式.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1245466/ee77e785-13d6-cdfb-2847-ac32faf9fe13.png)



Atcoder用のディレクトリに移動し、問題のテストデータを取得します。
`acc new {先ほどのコンテストコード}`を実行すると問題選択することができるので、自分が解きたい問題種別を選択していきます。
複数選択する場合は`<space>キー`で選択してください。
選択が終わり`<Enter>キー`で完了するとテストデータを取得できます。
```
$ cd Atcoder
$ acc new abc314

abc314/contest.acc.json created.
create project of AtCoder Beginner Contest 314
? select tasks (Press <space> to select, <a> to toggle all, <i> to invert selection)
❯◉ A 3.14
 ◯ B Roulette
 ◯ C Rotate Colored Subsequence
 ◯ D LOWER
 ◯ E Roulettes
 ◯ F A Certain Game
 ◯ G Amulets
```
実行後は以下のようなディレクトリが作成されます。
今回作成された`main.js`はテンプレートに設定したものが追加されるので、ここに問題の解答をコーディングしてください。
```
.
└── abc314
    ├── a
    │   ├── main.js
    │   └── test
    │       ├── sample-1.in
    │       ├── sample-1.out
    │       ├── sample-2.in
    │       ├── sample-2.out
    │       ├── sample-3.in
    │       └── sample-3.out
    └── contest.acc.json
```
解答ができたら自動テストを実行します。
今回はA問題を解いた想定で自動テストを実行しています。
```
$ cd abc314/a
$ oj t -c "node main.js"
```
以下のように`AC`と出力されれば正解です。
```
[INFO] sample-1
[INFO] time: 0.211756 sec
[SUCCESS] AC

[INFO] sample-2
[INFO] time: 0.106646 sec
[SUCCESS] AC
```
正解したコードを提出します。
提出コマンド実行時に文字列を入力する必要があるので入力してください。
入力するとブラウザ上で提出画面が表示されるので結果を確認できます。
```
$ cd abc314/a
$ acc submit -s -- -l 5009
・・・省略
Are you sure? Please type "abca" 
```

<a id="#Chapter5"></a>
# コマンド一覧
```
// 問題のテストデータを取得
$ acc new xxx （xxxは問題コード）

// テストデータの追加
//（abc314などの問題ディレクトリの場所で実行すると、後からテストデータを追加できます）
$ acc add

// テストの実行
$ oj t -c "node main.js"

// 提出
$ acc submit -s -- -l 5009
```


<a id="#reference"></a>
# 参考文献
- [online-judge-tools/oj](https://github.com/online-judge-tools/oj)
- [atcoder-cliを導入してみた](https://twoooooda.net/post/introduce-atcoder-cli/)
- [JavaScriptでAtCoderを始める方法（環境構築～実際のテストまで）Windows編](https://zenn.dev/deen/articles/137bf151b139ef)
