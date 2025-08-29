---
title: "【DAY82】GitHub PagesのJSゲームにlocalStorageでセーブ機能を追加"
emoji: "💾"
type: "tech"
topics: ["JavaScript", "localStorage", "ゲーム開発", "フロントエンド", "GitHub Pages"]
published: true
published_at: "2025-08-31 05:42"
---

## GitHub Pages × JavaScript ゲームにセーブ機能をつける

ブラウザで遊べるミニゲームを GitHub Pages で公開しているのだが、プレイヤーの進行状況を保存できるようにしたかった。オンラインゲームほど大げさなサーバー連携は不要なので、今回はブラウザの `localStorage` を使って、端末ごとにセーブ機能を実装してみた。

---

### なぜ localStorage？

`localStorage` はブラウザに組み込まれているストレージ機能で、キーと値のペアを永続的に保存できる。容量は5MB程度と限られているが、ちょっとしたゲームデータ（レベル、スコア、アイテム状況など）を保存するには十分。

特徴：

- クッキーと違ってサーバーに送信されない
- 明示的に削除しない限り保持される
- 同一オリジン（ドメイン + プロトコル + ポート）で共有される

---

### 実装の流れ

#### 1. ゲームの状態をオブジェクトで管理

```js
let gameState = {
  level: 3,
  score: 1500,
  inventory: ['sword', 'potion']
};
````

#### 2. セーブ処理を作成

オブジェクトはそのまま保存できないため、`JSON.stringify()`で文字列に変換して保存する。

```js
const saveGame = () => {
  localStorage.setItem('myGameSave', JSON.stringify(gameState));
};
```

#### 3. ロード処理を作成

文字列を `JSON.parse()` でオブジェクトに戻す。

```js
const loadGame = () => {
  const savedData = localStorage.getItem('myGameSave');
  if (savedData) {
    gameState = JSON.parse(savedData);
  }
};
```

#### 4. ページ読み込み時に自動ロード

```js
window.addEventListener('load', () => {
  loadGame();
});
```

#### 5. ゲーム内での自動セーブ

たとえばステージクリアやアイテム取得後など、ゲーム進行に合わせて `saveGame()` を呼ぶだけ。

---

### 注意点

* `localStorage` は**端末とブラウザに依存**する。別のデバイスや異なるブラウザではデータが共有されない。
* 保存容量に制限があるため、大きなデータ構造は避けた方がよい。
* プライベートブラウジングでは保存できない場合がある。
* JSONに変換できないもの（関数、DOM要素など）は保存不可。

---

### 今後の改善案

* セーブスロットを複数持たせる（例: `myGameSave1`, `myGameSave2`）
* セーブの暗号化（`atob`, `btoa` などの簡易処理）
* UIからセーブ／ロードボタンを追加して視認性アップ
* `sessionStorage`との使い分け（終了時に消える一時データ用）

---

## まとめ

`localStorage` を使えば、外部サーバーやログイン機能を用意しなくても、端末ごとにセーブ機能をつけることができる。特に GitHub Pages 上で動かすような軽量ゲームとの相性は抜群。セーブ／ロードの基本構造もシンプルなので、まずはここから導入してみると良い。

```

---