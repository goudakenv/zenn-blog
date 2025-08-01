---
title: "【DAY52】Webシステム1本で広がる、エンジニアリングの全技術マップ"
emoji: "🧩"
type: "tech"
topics: ["バックエンド", "フルスタック", "Web開発", "セキュリティ", "インフラ"]
published: true
published_at: "2025-08-01 05:42"
---

## Webシステム開発から見えてくる“技術の地図”

バックエンドのポートフォリオとして、自作のWebシステムを1つ完成させた。  
投稿・検索・削除ができる簡単な構成だが、この小さな実装を通して、多くの「技術の接続点」が見えてきた。とりあえず作ってみることで、実は**フロントエンド／バックエンド／インフラ／データベース／セキュリティ／設計**といった技術全体に触れることになる。

### 1. フロントエンドとの連携で「データフロー」を体感

ReactやVanilla JSでUIを作成し、バックエンドAPIとの通信にfetchやaxiosを使う。ここで、**どのデータをどう表示するか／どのタイミングでAPIを叩くか／非同期処理の設計**といった課題にぶつかる。この工程で「状態管理」や「UXの最適化」に意識が向き始めた。

### 2. API設計と認証の導入でバックエンドに厚みが出る

FirebaseやNode.jsでバックエンドを構築すると、ルーティングやCRUD処理だけでなく、**トークンベースの認証、CORS制御、バリデーション**といったセキュリティ要素が必要になる。ただのロジック記述ではなく、設計力や安全性の確保が問われる領域だ。

### 3. データベースは「ただ保存する場所」ではない

FirestoreやMySQLを使ってみて、初めて**正規化、非正規化、インデックス、読み取りコスト**といった概念が実感として理解できるようになった。さらに、NoSQLとRDBの使い分けや、パフォーマンス設計の大切さにも気づく。

### 4. デプロイ・インフラ周りで「運用視点」が入る

Vercel、Firebase Hosting、Renderなどを使って本番環境へデプロイ。ここでSSL、CDN、環境変数、ログ管理などの「インフラ的な考慮事項」が現実問題として立ち上がってくる。最初は「動けばいい」だったが、ここで「**安心して使える**」に進化する。

### 5. 設計とリファクタで“繋がり”を理解する

完成後のリファクタリングを通して、責務分離、設計パターン（MVC）、コードの再利用性、Gitによる履歴管理の重要性を実感。「ただ動くコード」から「読みやすく、育てやすい設計」へと意識が変わる。

---

## 結論：「全部やってみて、全部のつながりが見える」

技術を1つずつ順番に学ぶのも大事だが、「まず1本作る」ことで、すべての技術要素がどう連動しているかを体験できる。  
バックエンドから始めたこのWebシステムは、**結果的に総合的な技術理解を促す教材**となった。  
それでもまだ理解が浅い分野はある。だが「つながりの全体像」が見えていることで、次に学ぶべきテーマも明確になる。  
技術は“点”ではなく、“線と構造”で覚える。そう実感できたプロジェクトだった。