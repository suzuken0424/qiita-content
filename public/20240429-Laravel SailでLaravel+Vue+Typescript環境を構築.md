---
title: Laravel SailでLaravel+Vue+Typescript環境を構築
tags:
  - Laravel
private: false
updated_at: '2024-04-29T17:50:51+09:00'
id: acd23050ea54f8b547b9
organization_url_name: null
slide: false
ignorePublish: false
---
# 概要
今回はLaravel Sailを使って、Laravel+React+Typescriptの環境を構築します。

# 動作環境
- M1 Mac Ventura 13.4

# 省略事項
- Docker・Composerのインストール

# 環境構築
作成したいアプリのディレクトリを作り、Sailをインストールする
```zsh
$ curl -s "https://laravel.build/アプリ名" | bash
$ cd アプリ名
$ ./vendor/bin/sail up
```

.zshrcにaliasを書き込んで、sailコマンドで呼び出せるようにする
```
alias sail='[ -f sail ] && bash sail || bash vendor/bin/sail'
```

vueを使えるようにする
```zsh
$ sail npm i @vitejs/plugin-vue --save-dev
```
vite.config.jsを変更
```js
import { defineConfig } from 'vite';
import laravel from 'laravel-vite-plugin';
import vue from '@vitejs/plugin-vue'

export default defineConfig({
    plugins: [
        laravel({
            input: ['resources/css/app.css', 'resources/js/app.js'],
            refresh: true,
        }),
        vue(),
    ],
});
```
app.jsを変更
```js
import './bootstrap';

import { createApp, ref } from 'vue/dist/vue.esm-bundler';

createApp({
    setup() {

        const greeting = ref('こんにちは');

        return {
            greeting,
        }

    },
}).mount('#app');
```
welcome.blade.phpを変更
```php
<!doctype html>
<html lang="{{ str_replace('_', '-', app()->getLocale()) }}">
    <head>
        @vite(['resources/css/app.css', 'resources/js/app.js'])
    </head>
    <body>
        <h1>
            Vite のテストです
        </h1>
        <div id="app">
            <span v-text="greeting"></span>
        </div>
    </body>
</html>
```
サーバを起動し画面を確認する
起動時に出力されるAPP_URLにアクセスする
```zsh
$ sail npm run dev
```

tsconfig.jsonを作成
```json
{
    "compilerOptions": {
        "target": "es5",
        "module": "es2020",
        "moduleResolution": "node",
        "baseUrl": "./",
        "strict": true,
        "skipLibCheck": true,
        "noImplicitAny": false
    },
    "include": ["resources/js/**/*"]
}
```
app.jsをapp.tsに変更
vite.config.jsとwelcome.blade.php内に書かれているapp.jsをapp.tsに変更

モジュールをインストール
```zsh
$ sail npm i @typescript-eslint/parser eslint eslint-plugin-vue vue-eslint-parser eslint-webpack-plugin
```
.eslint.ymlを追加
```yaml
env:
  browser: true
  es2021: true
  
extends:
  - eslint:recommended
  - plugin:@typescript-eslint/recommended
  - plugin:vue/vue3-recommended
  - prettier

parserOptions:
  parser: '@typescript-eslint/parser'
  ecmaVersion: latest
  sourceType: module

plugins:
  - vue
  - '@typescript-eslint'

rules: {}
```
package.jsonに追加
```json
"scripts": {
    "lint": "eslint --fix resources/js/**/*.{ts,vue}"
}
```
適当な未使用の変数を定義し、lintを実行する
warningが出ればlinterが効いている
```zsh
$ sail npm run lint
```

prettierのインストール
```zsh
$ sail npm i eslint-config-prettier prettier
```
package.jsonに追加
```json
"scripts": {
    "format": "prettier -w resources/js/**/*.{ts,vue}"
}
```

huskyのインストール
```zsh
$ sail npx husky-init && npm install
```
できたpre-commitファイルにコミット前に実行するスクリプトを記述する
```shell
#!/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.sh"

npm run lint
npm run format
```

larastanのインストール
```zsh
$ composer require nunomaduro/larastan:^2.0 --dev
```
プロジェクトルートにphpstan.neonを作成
```
includes:
   - ./vendor/nunomaduro/larastan/extension.neon

parameters:
    # Level 9 is the highest level
    # https://phpstan.org/user-guide/rule-levels
    level: 6

    paths:
        - app/
        - bootstrap/
        - config/
        - resources/
        - routes/
        - tests/

    excludePaths:
        - routes/console.php
```
composer.jsonのscriptに追加
```json
"lint": [
  "./vendor/bin/phpstan analyse"
]
```
larastanを実行
```
$ composer lint
```
vscodeであればphpstanの拡張を入れておくと、コマンドを実行しなくてもwarningを教えてくれる
https://marketplace.visualstudio.com/items?itemName=SanderRonde.phpstan-vscode

pintの導入
```
$ sail composer require laravel/pint --dev
```
composer.jsonのscriptに追加
```json
"format": [
  "./vendor/bin/pint"
]
```
pintの実行
```
$ composer format
```

# 参考
