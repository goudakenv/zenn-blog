---
title: "【DAY35】Java最低限のクラス"
emoji: "☕"
type: "tech"
topics: ["Java", "コーディング", "初心者", "クラス設計", "基本構文"]
published: false
published_at: "2025-07-15 15:27"
---

## ✅ Spring Boot × DB：最小構成（4ファイル）

### ① `Main.java` – アプリ起動

```java
@SpringBootApplication
public class Main {
    public static void main(String[] args) {
        SpringApplication.run(Main.class, args);
    }
}
```

---

### ② `User.java` – エンティティ（テーブルに対応）

```java
import jakarta.persistence.*;

@Entity
public class User {
    @Id
    @GeneratedValue
    private Long id;

    private String name;

    public User() {}

    public User(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }
}
```

---

### ③ `UserRepository.java` – DBアクセス用（DAO）

```java
import org.springframework.data.jpa.repository.JpaRepository;

public interface UserRepository extends JpaRepository<User, Long> {
}
```

---

### ④ `UserController.java` – Web API用

```java
import org.springframework.web.bind.annotation.*;
import java.util.List;

@RestController
public class UserController {
    private final UserRepository repo;

    public UserController(UserRepository repo) {
        this.repo = repo;
    }

    @GetMapping("/users")
    public List<User> getAll() {
        return repo.findAll();
    }
}
```

---

### ✅ application.properties の設定（H2メモリDB）

```properties
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
spring.h2.console.enabled=true
spring.jpa.hibernate.ddl-auto=update
```

これで、Spring Boot 起動時に**自動でメモリ上に User テーブルが作られ、GET `/users` で一覧取得**できます。

---

## ✅ まとめ：最小の Spring Boot + DB 構成

| ファイル名                    | 役割          |
| ------------------------ | ----------- |
| `Main.java`              | アプリ起動       |
| `User.java`              | DBテーブル対応クラス |
| `UserRepository.java`    | DB操作        |
| `UserController.java`    | HTTPエンドポイント |
| `application.properties` | DB接続設定      |

---
