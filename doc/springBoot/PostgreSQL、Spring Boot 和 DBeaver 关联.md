要将 PostgreSQL、Spring Boot 和 DBeaver 关联起来，你需要完成以下几个步骤：

### 1. 安装 PostgreSQL

首先，你需要安装并配置 PostgreSQL 数据库。你可以从 [PostgreSQL 官网](https://www.postgresql.org/download/) 下载并安装适合你操作系统的版本。

### 2. 创建数据库和用户

安装完成后，创建一个新的数据库和用户，并赋予适当的权限。

```sql
-- 以超级用户身份登录 psql
psql -U postgres

-- 创建数据库
CREATE DATABASE mydb;

-- 创建用户
CREATE USER myuser WITH ENCRYPTED PASSWORD 'mypassword';

-- 给用户赋予数据库的所有权限
GRANT ALL PRIVILEGES ON DATABASE mydb TO myuser;
```

### 3. 在 Spring Boot 项目中配置 PostgreSQL

在你的 Spring Boot 项目中，配置 `application.properties` 或 `application.yml` 文件以连接到 PostgreSQL 数据库。

#### application.properties

```properties
spring.datasource.url=jdbc:postgresql://localhost:5432/mydb
spring.datasource.username=myuser
spring.datasource.password=mypassword
spring.datasource.driver-class-name=org.postgresql.Driver

spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect
```

#### application.yml

```yaml
spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/mydb
    username: myuser
    password: mypassword
    driver-class-name: org.postgresql.Driver

  jpa:
    hibernate:
      ddl-auto: update
    show-sql: true
    properties:
      hibernate:
        dialect: org.hibernate.dialect.PostgreSQLDialect
```

### 4. 添加 PostgreSQL 依赖

在你的 `build.gradle` 文件中添加 PostgreSQL 驱动的依赖。

```groovy
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
    implementation 'org.springframework.boot:spring-boot-starter-web'
    runtimeOnly 'org.postgresql:postgresql'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
}
```

### 5. 创建实体类和仓库接口

创建你的实体类和仓库接口。例如：

#### 实体类

```java
package com.example.demo.model;

import javax.persistence.*;
import java.io.Serializable;

@Entity
@Table(name = "help_documents")
public class HelpDocument implements Serializable {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String title;
    private String content;
    private String attachment;
    private Integer displayPosition;
    private Boolean status;
    private Long categoryId;

    // Getters and Setters
}
```

#### 仓库接口

```java
package com.example.demo.repository;

import com.example.demo.model.HelpDocument;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface HelpDocumentRepository extends JpaRepository<HelpDocument, Long> {
}
```

### 6. 使用 DBeaver 连接 PostgreSQL

1. 打开 DBeaver，点击左上角的 "新建连接" 图标。
2. 选择 "PostgreSQL" 并点击 "下一步"。
3. 输入你的数据库连接信息：
   - Host: localhost
   - Port: 5432
   - Database: mydb
   - Username: myuser
   - Password: mypassword
4. 点击 "测试连接" 以确保连接成功，然后点击 "完成"。

### 7. 运行 Spring Boot 应用

确保你的 Spring Boot 应用可以成功连接到 PostgreSQL 数据库并能够进行 CRUD 操作。

```java
package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

### 总结

通过以上步骤，你可以将 PostgreSQL 数据库与 Spring Boot 项目关联起来，并使用 DBeaver 进行数据库管理。这样你可以在 Spring Boot 应用中进行数据的持久化操作，并使用 DBeaver 来方便地查看和管理数据库中的数据。
