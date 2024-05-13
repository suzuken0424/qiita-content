---
title: Laravel Sailのバージョン変更
tags:
  - Laravel
private: false
updated_at: '2024-04-29T17:52:14+09:00'
id: 95774fa6f700c5c7c02a
organization_url_name: null
slide: false
ignorePublish: false
---
phpのバージョンを変更する(8.2 → 8.0)
docker-compose.ymlの以下のように変更する
変更前
```yaml
laravel.test:
        build:
            context: ./vendor/laravel/sail/runtimes/8.2
        image: sail-8.2/app
```
変更後
```yaml
laravel.test:
        build:
            context: ./vendor/laravel/sail/runtimes/8.0
        image: sail-8.0/app
```

Laravelのバージョンを指定してSailを導入する
```zsh
composer create-project laravel/laravel アプリ名 --prefer-dist "9.*"
cd アプリ名
php artisan sail:install
```

sail artisan -Vでcomposerのバージョンが合わないとのエラーが出た
```
Composer detected issues in your platform:

Your Composer dependencies require a PHP version ">= 8.1.0". You are running 8.0.29.

PHP Fatal error:  Composer detected issues in your platform: Your Composer dependencies require a PHP version ">= 8.1.0". You are running 8.0.29. in /var/www/html/vendor/composer/platform_check.php on line 24
```
composerにphpのバージョンを指定してupdateしてみると解決
composerのバージョンも特に変わっていなかった
```
$ composer config platform.php 8.0.29
$ composer update
$ sail artisan -V
Laravel Framework 9.52.10
```
