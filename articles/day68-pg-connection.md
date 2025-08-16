---
title: "【DAY68】Firebaseセキュリティ見直し：簡単に始めたが、守るのは簡単じゃない"
emoji: "🛡️"
type: "tech"
topics: ["Firebase", "セキュリティ", "Firestoreルール", "クラウド", "認証認可"]
published: true
published_at: "2025-08-17 05:42"
---

## Firebaseセキュリティ見直し：簡単に始めたが、守るのは簡単じゃない

最近、Firebaseで作成したWebアプリを改修している。元は「とにかく素早く動くものを」と思って構築したプロトタイプだったが、使っていくうちにセキュリティの甘さが気になるようになってきた。

今回は特に**Firebaseのセキュリティ強化**にフォーカスして、実際に見直したポイントや課題、今後の対応方針を整理する。

### セキュリティは後回しになりがち

Firebaseは非常に便利で、最初の開発に集中できる構成が揃っている。Authentication、Firestore、Cloud Functions、Hostingなど、一通り揃っていて無料枠でも結構使える。

ただ、その分「とりあえず動くものが簡単にできてしまう」ために、**セキュリティ設計を後回しにしやすい**という弱点もある。

- Firestoreのルールが「allow read, write: if true;」のまま
- 誰でも呼べるCloud Functionになっている
- Authenticationを使っていても、認可（authorization）が弱い

気付いたら、どこからでもデータが読めてしまうような状態になっていた。

### Firestoreルールの見直し

最初に見直したのは、**Firestoreのセキュリティルール**。開発初期に緩めのルールで進めていたが、本番運用を意識して以下のように修正。

```js
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }

    match /adminOnly/{docId} {
      allow read, write: if request.auth != null && request.auth.token.admin == true;
    }
  }
}
````

* **自分のドキュメントしか操作できない**ようにする
* **管理者権限（custom claims）を持ったユーザーだけがアクセスできる領域を作る**

特に `request.auth.token.admin == true` のようなチェックは、Firebase Authでカスタムクレームを設定する必要があるが、これによりロールベースのアクセス制御が実現できる。

### Cloud Functionsの保護

Cloud Functionsもセキュリティ強化が必要。特に以下を対応中：

* **未認証の呼び出しを禁止**
* **関数内で `request.auth` をチェック**
* **関数ごとにアクセス制御を設計**

```js
exports.restrictedFunction = functions.https.onCall((data, context) => {
  if (!context.auth) {
    throw new functions.https.HttpsError('unauthenticated', 'ログインが必要です。');
  }

  // 追加の認可ロジック（たとえば admin 判定）などもここで行う
});
```

呼び出し元で `onCall` を使うことで、自動的に認証情報が渡されるが、**関数側でも必ず認証チェックを行うことが重要**。

### Firebase Authenticationだけでは足りない

Firebase Authを使えばログイン機能はすぐに実装できるが、それだけでは不十分。

* **ログインしているだけ**で全ての操作が可能な状態になりやすい
* **認可処理**が弱く、アプリ側でしっかり制御しないと危険
* **カスタムクレーム**でのロール管理が必要

Spring Bootで開発していた頃は、Spring Securityを使ってルールベースでアクセス制御していたが、Firebaseではそれをアプリと連携して行う必要がある。

### 今後の対応

* **Firestoreルールの細分化**（コレクション単位で制御）
* **監査ログの記録**（ログをBigQueryに連携）
* **セキュリティルールの自動テスト導入**
* **匿名ログイン禁止**＆セッション監視

開発スピードを重視するあまり、セキュリティ設計を甘くしてしまうのは本末転倒。
特にクラウドベースで公開するアプリでは、**想定外のアクセスが常に発生しうる**という前提で考えなければならない。

---

Firebaseは非常に優秀な開発基盤だが、その利便性の裏にある\*\*「セキュリティを自分で設計する責任」\*\*を忘れずに、引き続き改修を進めていく。

---
