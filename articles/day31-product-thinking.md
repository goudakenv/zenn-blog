---
title: "【DAY31】システム開発における小さな一歩の積み重ね"
emoji: "📑"
type: "tech"
topics: ["システム開発", "Next.js", "Firebase", "GitHub"]
published: true
published_at: "2025-07-11 10:00"
---

# 【DAY31】システム開発における小さな一歩の積み重ね

こんにちは、Keisukeです。

今回は、個人開発を進める中で実際に感じた「小さな一歩を積み重ねることの大切さ」について、技術的な視点から書いていきます。

---

## 1. Firebase + Next.jsで最小構成のプロトタイプを作る

最近、**Firebase Authentication**と**Next.js**を使ってログイン機能付きのミニアプリを開発しました。  
初期段階では以下のような構成で始めました：

- Next.js（App Router）
- Firebase Authentication（メール＆パスワード認証）
- Firestore（ユーザーデータ管理）

開発初期は、完璧を求めず「とにかくログインできて、データが保存できる」ことだけをゴールにしました。

```js
import { getAuth, signInWithEmailAndPassword } from "firebase/auth";

const login = async (email, password) => {
  const auth = getAuth();
  await signInWithEmailAndPassword(auth, email, password);
};
````

↑これだけでも「動くもの」が完成する達成感は大きいです。

---

## 2. GitHubでバージョン管理しつつ、小さな改善を続ける

リポジトリはすべて**GitHub上で管理**しており、以下の工夫をしています：

* ブランチ運用（`main` / `develop`）
* GitHub Projectsでタスク管理
* GitHub ActionsによるCI（ESLint / Prettierチェック）

```yaml
# .github/workflows/lint.yml
name: Lint Check
on: [push]
jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Install deps
        run: npm install
      - name: Run ESLint
        run: npm run lint
```

こうした小さな整備の積み重ねが、のちの大規模変更を楽にします。

---

## 3. フィードバック駆動でUI改善も段階的に

ユーザー（＝自分）からのフィードバックで、UIも改善しています。
最初は `input` タグだけだったものを、Material UI を導入しアクセシビリティ向上を図りました。

```tsx
import { TextField } from "@mui/material";

<TextField
  label="メールアドレス"
  variant="outlined"
  fullWidth
  margin="normal"
/>
```

---

## 4. Vercelへの継続的デプロイで即反映

最終的には**Vercelにデプロイ**し、`main` ブランチにマージされると即座に本番反映されるよう設定。

こうして、開発・検証・改善・公開がすべて自動化されています。

---

## ✅ 今日の気づき

* 技術選定に悩むより、まず小さく実装する
* 少しずつ改善していけば、プロダクトは自然と成長する
* FirebaseやVercelなどのツールを使えば、**最短距離でリリース可能**

---

### 🛠 使用技術まとめ

| 技術                            | 用途                    |
| ----------------------------- | --------------------- |
| **Next.js**                   | フロントエンド（App Router構成） |
| **Firebase Auth / Firestore** | 認証・DB                 |
| **GitHub + GitHub Actions**   | 管理・CI                 |
| **Vercel**                    | デプロイ環境                |
| **Material UI**               | UIコンポーネント             |

```

---

### ✅ 保存方法

1. VS Code で `zenn-blog/articles/` フォルダを開く  
2. ファイル名：`day31-small-steps-dev.md` など
3. 上記内容をコピペ → 保存
4. `git add .` → `git commit -m "Add DAY31 article"` → `git push`

---