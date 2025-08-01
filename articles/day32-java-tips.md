---
title: "【DAY32】Javaを書くときに意識したいコツ"
emoji: "☕"
type: "tech"
topics: ["Java", "コーディング", "ベストプラクティス"]
published: true
publishedAt: "2025-07-12T10:00:00+09:00"
---


# 【DAY32】Javaを書くときに意識しておきたいコツ 
## 5つのコツ

最近はJavaを扱う機会が減ってきているとはいえ、業務システムや企業案件ではまだまだ現役の言語。  
今回は、初級〜中級のJava実装時に気をつけたいポイントや、「これは覚えておくと便利」と感じたことを5つ紹介します。

---

### 1. 変数やクラス名は「具体的に」

Javaは静的型付き言語なので、**型やスコープ、クラス設計が非常に重要**です。  
つい `data` や `obj` といった抽象的な名前をつけがちですが、  
```java
Customer customer = new Customer();
OrderDetail detail = new OrderDetail();
のように、**「役割そのものを名前に含める」**ことでコードが一気に読みやすくなります。 また、DTO（データ転送用オブジェクト）やVO（値オブジェクト）も命名を意識するだけで、可読性が大幅に変わります。

2. nullチェックは必ず丁寧に
Javaでは NullPointerException が非常に起きやすいです。 if文で丁寧にチェックするのはもちろん、最近は Optional を使うことでnullの扱いを明示的にできます。
Optional<String> maybeName = Optional.ofNullable(name);
maybeName.ifPresent(System.out::println);
これにより、「nullかどうかの判断を、呼び出し元ではなくデータ側で済ませられる」設計になります。

3. 処理の分離はクラス単位で
長くなりがちなJavaコードでは、**「責任の分離（SRP）」**を意識するだけで一気にスッキリします。 たとえば、ユーザー認証処理・DB接続・ログ出力が1つのクラスにまとまっていると修正が困難になります。
// Bad
class UserService {
    void login() { ... }
    void connectDB() { ... }
    void writeLog() { ... }
}

// Good
class UserService { ... }
class DBConnector { ... }
class Logger { ... }
機能単位で役割を分けるだけでも、後からの保守性が格段にアップします。

4. try-catchは広げすぎない
Javaでありがちなのが「try-catchで全体を囲って終わり」にするコード。 しかし、これはエラーの原因を隠してしまう場合があります。 ポイントは、本当に例外が起きる部分だけをtryに入れること。
try {
    db.connect();
    db.saveData(data);
} catch (SQLException e) {
    logger.error("DB保存時にエラー", e);
}
catchブロックでは 例外の詳細をログに残すことも重要。エラー解析が後でグッと楽になります。

5. 「mainで全部書かない」を脱する
初学者のうちは public static void main(String[] args) にすべてを書きがちですが、 Javaの真価は「クラスとメソッドを分けて構造化できる」ことにあります。
* 処理は必ず別メソッドに切り出す 
* データはクラス化して渡す 
* 何度も使う処理はユーティリティクラスへ 
といった基本を守るだけで、「Javaっぽさ」がグッと増します。

まとめ
Javaは記述が冗長と言われがちですが、設計力が如実に出る言語でもあります。 しっかりクラス設計・メソッド分離・例外処理・命名を意識することで、 保守性・拡張性の高いコードに仕上がります。
何より、「読み返したときに理解できる自分のコード」を目指すことが、 実は一番の上達の近道かもしれません。
---
