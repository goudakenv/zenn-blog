---
title: "Day34 - DB接続"
emoji: "☕️"
type: "tech"
topics: ["Java", "JDBC", "Database", "プログラミング学習", "学習ログ"]
published: false
published_at: 2025-07-14 10:00
---

# Day34 - JavaでのDB接続

本日は Java の DB 接続についてJava におけるデータベースとの接続には JDBC（Java Database Connectivity）を使用します。以下では、ポイントを5つのトピックに分けて整理します。

---

## 1. JDBCとは？

JDBC は Java 標準のデータベース接続用 API です。これを利用することで、Java プログラムから SQL を使ってリレーショナルデータベースにアクセスできます。

JDBC の主な役割は以下のとおりです：

- データベースへの接続
- SQL の送信（SELECT, INSERT など）
- 結果の取得と処理
- 接続の終了（リソース開放）

---

## 2. Java DB接続に必要な基本構成

Java で DB に接続するには、以下のコンポーネントが必要になります：

| クラス名 | 役割 |
|---------|------|
| `DriverManager` | DB接続の初期化・管理 |
| `Connection` | DBとのコネクション |
| `PreparedStatement` | パラメータ付き SQL の送信 |
| `ResultSet` | SQLの実行結果を受け取る |
| `SQLException` | エラー時の例外処理用 |

それぞれのクラスは明確な責任を持ち、組み合わせて DB 操作を実現します。

---

## 3. DB接続コードの基本形

以下は、JDBC を用いた MySQL への接続と、`users` テーブルの情報を取得する基本コードです：

```java
String url = "jdbc:mysql://localhost:3306/testdb";
String user = "user";
String password = "password";

try (Connection conn = DriverManager.getConnection(url, user, password);
     PreparedStatement stmt = conn.prepareStatement("SELECT * FROM users");
     ResultSet rs = stmt.executeQuery()) {

    while (rs.next()) {
        System.out.println(rs.getString("name"));
    }

} catch (SQLException e) {
    e.printStackTrace();
}
````

ポイントは `try-with-resources` を使ってリソースを自動解放している点です。

---

## 4. よくあるエラーと対処法

実行時に遭遇しやすいエラーとその原因は以下の通りです：

* `ClassNotFoundException`：JDBC ドライバが読み込めていない
* `SQLException`：URL・認証情報ミス、SQL文の構文エラーなど
* `CommunicationsException`：DB サーバーに接続できない

それぞれスタックトレースを読み解く力が求められ、慣れてくると原因の特定が早くなります。

---

## 5. 明日以降の課題と展望

今後の課題としては、以下のようなトピックを学習予定です：

* トランザクション管理（commit / rollback）
* バッチ処理（複数 SQL をまとめて処理）
* SQLインジェクション対策の再確認
* DB 設計と正規化の基礎

Java における DB 操作は最初こそ冗長に感じるが、安全性・柔軟性を確保するために重要なステップ。今後は Spring Framework などの ORM ツールとの比較も行っていきたい。

---

💬 **感想：**
JDBC は覚えることが多いが、構造を理解すればパターン化できる。エラー処理や接続管理の考え方は他言語でも役立つスキル

---
