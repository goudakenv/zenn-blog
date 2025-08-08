---
title: "【DAY63】業務アプリに必要なDB設計とエンティティモデリングの進め方"
emoji: "🗂️"
type: "tech"
topics: ["DB設計", "Spring Data JPA", "正規化", "ER設計", "リレーション"]
published: true
published_at: "2025-08-12 05:42"
---

## 業務アプリを支える“データの設計”が甘いと、全部崩れる

Spring Bootでの再構築において、**DB設計の再整理は非常に重要**。  
ここを雑にすると、後でロジックが破綻し、パフォーマンスも落ち、保守性も最悪になる。

---

### 📌 再構築対象のドメインモデル（例）

今回は「業務申請アプリ」を想定し、以下のエンティティを想定：

- `User`：ユーザー情報（ログイン、ロール）
- `Request`：申請データ
- `Approval`：承認履歴（多段階承認）
- `Comment`：申請に対するコメント
- `Notification`：通知履歴（未読既読など）

---

### 🔁 リレーション設計例

- `User 1:N Request`（ユーザーは複数申請できる）
- `Request 1:N Approval`（申請に複数の承認が紐づく）
- `Request 1:N Comment`
- `User 1:N Notification`

---

### ✅ 技術的観点

- @OneToMany / @ManyToOne / @JoinColumn の適切な使い分け
- Lazy / Eager の読み込み戦略（パフォーマンス最適化）
- DTOとの分離：Entityの肥大化を防ぐ
- Repositoryインターフェースの設計
- Flyway or Liquibase でのスキーママイグレーション管理

---

### 💡 Tips：業務データの「状態」設計

申請には「下書き」「申請中」「承認済」「差戻し」などの状態がある。  
これらをEnum＋Stateパターンで管理することで、状態遷移をコードで明確にできる。

```java
public enum RequestStatus {
  DRAFT, SUBMITTED, APPROVED, REJECTED
}