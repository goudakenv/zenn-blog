---
title: "【DAY62】Spring Securityで構築する認証・認可のベストプラクティス"
emoji: "🔐"
type: "tech"
topics: ["Spring Security", "JWT", "認可", "ユーザー認証", "セキュリティ"]
published: true
published_at: "2025-08-11 05:42"
---

## Java/Spring Bootで“認証・認可”をどう設計すべきか？

今回は、WEBシステムの根幹機能である**ログイン機能（認証）とアクセス制御（認可）**について、Spring Securityを使って構築する。

Firebase Authなどとは異なり、Javaでは**セキュリティ層も完全に自分でコントロールできる**。その分、柔軟で、強力な設計が可能になる。

---

### 🔐 構成要素（最低限）

- **ユーザー管理**：`User`, `Role`, `Permission` のEntity設計
- **JWT認証**：トークンベースでセッションレスなAPI認証
- **Spring Security設定**：`SecurityFilterChain`, `AuthenticationManager`
- **パス制限**：管理者と一般ユーザーでアクセスルートを分離

---

### JWTトークンの仕組み（簡易）

- ユーザーがログイン（ID/PW認証）
- 認証成功時にJWTトークンを発行
- クライアントは次回以降トークンをAuthorizationヘッダに付与
- サーバー側はトークンを検証してユーザー情報を取得

---

### 認可（RBAC）の設計

- `ROLE_USER`, `ROLE_ADMIN` などのロール定義
- 特定のURLに対して `hasRole("ADMIN")` のように制限を設定
- 業務ロジック側にもロールに応じた処理分岐を用意

---

### 技術スタック（使用予定）

- Spring Security 6.x
- JWT（jjwt or java-jwt）
- PasswordEncoder（BCrypt）
- `@PreAuthorize`, `@Secured` でのアノテーション認可制御

---

### 今後の課題

- ログイン/ログアウトのエンドポイント設計
- トークン失効の管理（ブラックリスト or 有効期限短縮）
- ユーザー登録時のバリデーション、エラー処理の洗練

---

次回は、データベース設計を改めて見直し、「業務アプリに必要な正規化とリレーション設計」に踏み込んでいく。

---