---
title: "【DAY99】Firebaseで生活を自動運転"
emoji: "🚀"
type: "tech"
topics: ["Firebase", "JavaScript", "自動化", "生活改善", "クラウド"]
published: true
published_at: "2025-09-17 05:42"
---

## クレームから始まった生活管理

日々の生活の中で、「あれ、また忘れてるよ」「やってないじゃん」が繰り返された。  
ゴミ出し、買い物、洗濯、ちょっとした予定の管理。  
些細なタスクが抜けるたびに、家族からのクレームが溜まっていった。

**タスクを紙に書いても意味がない。**  
**リマインダーは通知が多すぎて無視してしまう。**

じゃあ、生活の流れに「自動で組み込む」しかないと思った。  
選んだのは、FirebaseとJavaScript。

## Firebase × JavaScript で生活をコード化する

構成はこう。

- **Firebase Authentication**：家族ごとのログイン認証
- **Firestore**：日々のタスクやルーチンを保存
- **Cloud Functions**：定時実行でリマインダーを自動送信
- **JavaScript（Web App）**：UIはシンプルなToDoアプリ風に

```js
// Firestore にタスクを追加する例
import { getFirestore, collection, addDoc } from "firebase/firestore"; 

const db = getFirestore();
await addDoc(collection(db, "dailyTasks"), {
  title: "水曜日のゴミ出し",
  due: "2025-09-17T07:00:00",
  user: "uid_12345"
});
````

Cloud Functions を使って、毎朝7時にその日のタスクをSlackとLINEに送るようにもした。

```js
// functions/index.js の一部（定時通知）
exports.sendDailyReminder = functions.pubsub.schedule('every day 07:00').onRun(async (context) => {
  const tasks = await getTodayTasksFromFirestore(); // Firestoreから取得
  const message = formatTasks(tasks);
  await sendToLine(message);
});
```

## 文句が止まるとき、それはシステムが機能している

最初は家族にも「また何か作ってる…」と呆れられた。
通知がうまく届かない、タスクが重複する、リマインダーが夜に飛んでくるなど、トラブルも多かった。
そのたびに「結局面倒になるなら手でやってよ」とクレーム。

だが、Firebaseは成長が早い。
Cloud Functionsで定期実行を整え、Firestoreの構造を見直し、バグを潰していった。
**数週間後、誰も何も言わなくなった。**

通知は適切なタイミングで来るようになり、生活の抜けが減った。
何も言われなくなったことが、**このシステムが"当たり前"になった証拠だった。**

## コードが生活を整える時代

生活管理というと、アプリをダウンロードして使うだけになりがちだけど、
自分の生活に最適化された仕組みは、結局自分で作るしかない。

FirebaseとJavaScriptの組み合わせはその最適解だった。

* ブラウザだけで完結できる
* マルチユーザー対応も簡単
* バックエンドはCloud Functionsに任せられる

生活にプログラミングを組み込むことで、「人が動かなくてもいい仕組み」ができる。
その積み重ねが、ストレスの削減や信頼の積み上げにつながっていく。

---

「生活をコードで回す」って、一見やりすぎに見えるかもしれない。
でも、誰にも文句を言われず、毎日が静かにスムーズに回るようになると、
**その価値は数字では測れないくらい大きい**。