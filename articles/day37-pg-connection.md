---
title: "【DAY37】PHPララベルDB保存先の簡単な説明"
emoji: "💾"
type: "tech"
topics: ["Laravel", "PHP", "初心者", "データベース", ".env設定"]
published: true
published_at: "2025-07-17 05:42"
---

# 【DAY37】PHPララベルDB保存先の簡単な説明

Laravel（ララベル）は、PHPで作られた人気のWebアプリケーションフレームワークです。その中でも「データベースとのやり取り」は、開発でよく使う重要な部分です。今回は、Laravelでデータを保存する“保存先”について、できるだけシンプルに説明します。

## 保存先＝データベース

Laravelでデータを保存する場合、多くは「データベース（DB）」が保存先になります。LaravelはMySQL、PostgreSQL、SQLiteなど、さまざまなDBに対応しています。

どのDBに保存するかは、`.env`ファイルで設定されます。以下のような記述があります。

```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=my_app
DB_USERNAME=root
DB_PASSWORD=password
```

ここを変えることで、保存先のDBを変更できます。

## モデルとマイグレーション

Laravelでは「モデル」を使って、データの保存先であるテーブルとやり取りします。たとえば、`User`モデルなら、`users`テーブルに自動的に接続します。

```php
$user = new User();
$user->name = 'Taro';
$user->email = 'taro@example.com';
$user->save(); // → DBのusersテーブルに保存される
```

また、マイグレーションを使えば、テーブル構造の作成も簡単にできます。

```bash
php artisan make:migration create_users_table
```

## 保存先の切り替えも可能

複数のDB接続を設定しておけば、保存先を切り替えることも可能です。

```php
$user->setConnection('sqlite')->save();
```

## まとめ

Laravelでは、データの保存先は基本的にデータベースです。`.env`で接続先を設定し、モデル経由でデータを保存します。シンプルな構成ですが、柔軟性が高いのもLaravelの魅力です。

---
