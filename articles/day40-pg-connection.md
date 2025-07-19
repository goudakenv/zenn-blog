---
title: "【Day40】Swift×BEM記法でフロント実装をやってみた話"
emoji: "🧱"
type: "tech"
topics: ["HTML", "CSS", "BEM", "Swift", "フロントエンド"]
published: true
published_at: "2025-07-20 05:42"
---
```

## BEMと向き合ってみた

決められていたルールは「**BEM記法を徹底すること**」。名前は聞いたことがあったものの、実際に使うのはこれが初めて。Swiftとは構造の捉え方がまるで違う中、どうやって技術的に落とし込んでいったかを書いておきます。

---

## BEM記法とは何者か

BEMは「Block / Element / Modifier」の略。
HTML/CSSのクラス設計手法のひとつで、以下のような構造になります。

```html
<div class="form">
  <label class="form__label">メールアドレス</label>
  <input class="form__input" type="email" />
  <div class="form__error-message">必須です</div>
</div>
```

```css
.form { ... }
.form__label { ... }
.form__input { ... }
.form__error-message { color: red; }
```

それぞれの命名に意味があり、クラスの依存関係が明確になります。
Blockが大枠、Elementが構成要素、Modifierが状態変化。

SwiftUIなどでは状態やレイアウトをコードで完結できますが、HTML/CSSでは名前付けがUI設計そのもの。この点が特に印象的でした。

---

## Swift脳でHTML/CSSに取り組んでみた

HTML/CSSの設計で一番戸惑ったのは「**スタイルの責務がどこにあるか不明瞭になりやすい**」という点です。
たとえば、コンポーネントに対して`margin`をどこで持たせるか、BEMではBlock側で持つべきなのか、Elementが持ってもいいのか…といった細かな判断が必要になります。

SwiftではViewやStateなど、機能と状態が明確に分かれているため、見た目とロジックが分離しやすいですが、CSSの場合は設計次第で管理が難しくなります。

そこで以下の点を意識しました：

* Block単位でUIの責務を切り出す
* 再利用性のある構造は`common.css`へ寄せる
* JSによる動的処理は `js-toggle-password` のようにクラス名で明示
* WFベースで余白・色をピクセル単位で合わせる

---

## ちょっとしたTips

実装中に特に便利だったポイントをいくつか：

* `.form__field` などの汎用的なElementに `--new` のModifierを付けることで差分の管理がしやすかった
* FontAwesomeでのアイコン制御もクラスを統一（例：`.form__toggle-icon`）
* エラーメッセージやバリデーションも構造的に分離して記述

また、Sourcetree＋GitHub運用も行い、ブランチ名・PRコメントもチームルールに従って統一しました。
**技術的な作業をコード／運用両面で最適化する意識**はSwift以上に求められるかもしれません。

---

## まとめ

HTML/CSSはコードというより「設計図に命を与える作業」だと感じました。
Swiftに慣れている人間としては、曖昧さにストレスを感じる場面も多かったですが、**BEMによって構造が明確になり、結果的に効率よくコーディングできた**というのが今回の収穫です。

今後も、より保守性の高いCSS設計を意識して、SwiftとWebの両方で戦える開発者になりたいと思います。

---