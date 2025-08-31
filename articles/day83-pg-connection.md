---
title: "【DAY83】Unityのセーブ機能を考える"
emoji: "💾"
type: "tech"
topics: ["Unity", "セーブデータ", "PlayerPrefs", "localStorage", "クロスプラットフォーム"]
published: true
published_at: "2025-09-01 05:42"
---

## Unityでのセーブ処理とJavaScriptとの違い

最近、GitHub Pagesで公開しているJavaScript製のミニゲームに`localStorage`を使ったセーブ機能を実装した。これにより、ユーザーのブラウザ単位で進行状況を保存でき、ページを再読み込みしてもデータが残る仕組みが完成した。

しかし、これと全く同じことを**Unityでやろうとすると、少し勝手が違う**。Unityでは、標準で用意されている`PlayerPrefs`というクラスを使ってローカルにデータを保存できる。ただし、`PlayerPrefs`はあくまで**小規模なデータ向け**で、例えばハイスコアや設定値、進行ステージのインデックス程度が適している。

```csharp
// ステージ番号を保存
PlayerPrefs.SetInt("stage", 3);
PlayerPrefs.Save();

// ステージ番号を読み込み
int stage = PlayerPrefs.GetInt("stage", 0);
````

保存先はプラットフォームごとに異なり、Windowsではレジストリ、AndroidやiOSではアプリごとのサンドボックス領域に保存される。これはJavaScriptの`localStorage`と似て非なるもので、例えばスマホとPCでデータ共有は基本的にできない。

一方で、JavaScriptの`localStorage`は以下のように非常に手軽で、文字列であればなんでも保存できる。

```js
// ゲームデータ保存
localStorage.setItem("score", "1200");

// 取得
const score = localStorage.getItem("score");
```

## Unityで本格的なセーブ機能を作るには？

`PlayerPrefs`だけでは足りないと感じる場合、UnityではJSONファイルを用いた独自のセーブシステムを構築するのが定番だ。これにより、ゲームの状態（キャラクター情報、アイテム、座標など）を構造的に保存できる。

```csharp
[System.Serializable]
public class SaveData {
    public int level;
    public float health;
}

void SaveGame() {
    SaveData data = new SaveData();
    data.level = 5;
    data.health = 78.5f;

    string json = JsonUtility.ToJson(data);
    File.WriteAllText(Application.persistentDataPath + "/save.json", json);
}
```

このやり方であれば、`Application.persistentDataPath`を利用して、プラットフォームごとに適切な保存ディレクトリにデータを書き出すことができる。セキュリティやアクセス権の面でも安全性が高い。

## JavaScriptとの棲み分け

JavaScriptゲームでは、環境が完全にブラウザ上に限定されているため、セーブデータは軽量かつセッション的な使い方が主となる。一方、Unityではモバイル、PC、コンソールなど多様な環境で動作するため、セーブ機能もより**堅牢で柔軟な設計**が求められる。

また、UnityのWebGLビルドでは`PlayerPrefs`の実体も内部的には`IndexedDB`を使用しているため、実はJavaScriptと似た構造を取っている。しかし、WebGLではファイルの読み書きに制限が多く、通常のJSON保存手法はそのまま使えない点に注意が必要だ。

## まとめ

GitHub Pagesに公開しているJavaScriptゲームで実装した`localStorage`セーブは、手軽で良い方法だったが、Unityでは環境に応じたセーブ方式を選ぶ必要がある。特にクロスプラットフォーム展開を視野に入れるなら、ファイルベースのJSON保存、クラウド連携など、**拡張性のある設計が重要**になる。Unityでのセーブは単なる「保存」ではなく、設計思想そのものが問われるポイントだと感じた。

---
