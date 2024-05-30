# Spring Boot 项目目录结构及其作用

Spring Boot 项目一般遵循 Maven 或 Gradle 的标准目录结构。以下是一个典型的 Spring Boot 项目的目录结构及其详细说明。

## 目录结构概览

```plaintext
demo-help
├── build
├── gradle
├── logs
├── src
│   ├── main
│   │   ├── java
│   │   │   └── com
│   │   │       └── Muliminty
│   │   │           └── demo
│   │   │               ├── help
│   │   │               │   ├── controller
│   │   │               │   ├── dao
│   │   │               │   ├── model
│   │   │               │   ├── service
│   │   │               │   │   ├── impl
│   │   │               │   └── vo
│   │   │               └── DemoHelpApplication.java
│   │   └── resources
│   │       ├── i18n
│   │       ├── permission
│   │       ├── application-dev.yml
│   │       ├── application-staging.yml
│   │       ├── application.yml
│   │       └── logback.xml
│   └── test
│       ├── java
│       │   └── com
│       │       └── Muliminty
│       │           └── demo
│       │               ├── controller
│       │               └── service
│       └── resources
│           └── json
├── Dockerfile
├── README.md
├── build.gradle
├── gradlew
├── gradlew.bat
└── settings.gradle
```

### 顶层目录

- `build/`: 编译过程中生成的文件目录。
  - `classes/`: 编译生成的字节码文件。
  - `generated/`: 编译过程中自动生成的源代码。
  - `resources/`: 编译过程中打包的资源文件。
  - `tmp/`: 临时文件目录。

- `gradle/`: Gradle 构建工具相关文件。
  - `wrapper/`: Gradle Wrapper 文件，用于确保构建过程中使用相同版本的 Gradle。

- `logs/`: 存放项目运行时生成的日志文件。

- `src/`: 源代码和资源文件目录。
  - `main/`: 主代码目录。
    - `java/`: Java 源代码。
      - `com/Muliminty/demo/help/`: 项目根包。
        - `controller/`: 控制器层，处理 HTTP 请求，调用 Service 层，并返回结果。
        - `dao/`: 数据访问层，与数据库交互，执行 CRUD 操作。
        - `model/`: 实体类，定义与数据库表结构对应的 Java 对象。
        - `service/`: 业务逻辑层，编写业务逻辑代码。
          - `impl/`: 业务逻辑实现类。
        - `vo/`: 数据传输对象（DTO），定义请求和响应的对象结构。
      - `DemoHelpApplication.java`: Spring Boot 启动类，包含 `main` 方法，启动 Spring Boot 应用。
    - `resources/`: 资源文件目录。
      - `i18n/`: 国际化资源文件。
      - `permission/`: 权限配置文件。
      - `application-dev.yml`: 开发环境配置文件。
      - `application-staging.yml`: 预生产环境配置文件。
      - `application.yml`: 默认配置文件。
      - `logback.xml`: 日志配置文件。

  - `test/`: 测试代码目录。
    - `java/`: Java 测试代码。
      - `com/Muliminty/demo/help/`: 项目根包。
        - `controller/`: 控制器层的测试代码。
        - `service/`: 业务逻辑层的测试代码。
    - `resources/`: 测试资源文件。
      - `json/`: JSON 测试数据文件。

### 根目录文件

- `Dockerfile`: Docker 配置文件，用于容器化部署。
- `README.md`: 项目说明文件，包含项目简介、使用方法等。
- `build.gradle`: Gradle 构建脚本，定义项目依赖和构建配置。
- `gradlew`: Unix 环境下的 Gradle Wrapper 可执行文件。
- `gradlew.bat`: Windows 环境下的 Gradle Wrapper 可执行文件。
- `settings.gradle`: Gradle 设置文件，定义项目名称及多项目构建配置。

## 详细解释

### `src/main/java/com/Muliminty/demo/help/`

#### `controller/`

控制器层，处理客户端请求，通常使用 `@RestController` 注解。每个控制器对应一个 RESTful API 资源。

示例：
```java
@RestController
@RequestMapping("/categories")
public class HelpCategoriesController {
    @Autowired
    private HelpCategoriesService helpCategoriesService;

    @PostMapping("/create")
    public ResponseEntity<?> createCategory(@RequestBody HelpCategoriesCreateVo createVo) {
        // 调用业务逻辑
    }
}
```

#### `dao/`

数据访问层，通常使用 Spring Data JPA 提供的接口来操作数据库。主要接口是 `JpaRepository` 或 `CrudRepository`。

示例：
```java
public interface HelpCategoriesDao extends JpaRepository<HelpCategories, Long> {
    // 自定义查询方法
}
```

#### `model/`

实体类，对应数据库表结构，使用 `@Entity` 注解。

示例：
```java
@Entity
@Table(name = "HELP_CATEGORIES")
public class HelpCategories {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(name = "PARENT_ID")
    private Integer parentId;

    @Column(name = "NAME", length = 255)
    private String name;
}
```

#### `service/`

业务逻辑层，编写业务逻辑，通常会调用 DAO 层进行数据操作。接口定义服务方法，实现类编写具体业务逻辑。

示例：
```java
public interface HelpCategoriesService {
    HelpCategoriesGetVo findById(Long id);
    HelpCategoriesPageVo findByPage(HelpCategoriesConditionVo conditionVo);
}
```

实现类：
```java
@Service
public class HelpCategoriesServiceImpl implements HelpCategoriesService {
    @Autowired
    private HelpCategoriesDao helpCategoriesDao;

    @Override
    public HelpCategoriesGetVo findById(Long id) {
        // 业务逻辑
    }
}
```

#### `vo/`

数据传输对象（DTO），定义请求和响应的数据结构。

- `request/`: 定义请求参数对象。
- `response/`: 定义响应结果对象。

示例：
```java
public class HelpCategoriesCreateVo {
    private String name;
    private Integer parentId;
}
```

### `src/main/resources/`

#### `application.yml`

Spring Boot 配置文件，定义应用程序的配置属性。可以分环境管理多个配置文件，如 `application-dev.yml`。

示例：
```yaml
server:
  port: 8080

spring:
  datasource:
    url: jdbc:mysql://localhost:3306/demo
    username: root
    password: password
```

#### `logback.xml`

日志配置文件，定义日志的输出格式和级别。

示例：
```xml
<configuration>
  <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
    <encoder>
      <pattern>%d{yyyy-MM-dd HH:mm:ss} - %msg%n</pattern>
    </encoder>
  </appender>

  <root level="info">
    <appender-ref ref="STDOUT" />
  </root>
</configuration>
```

### `src/test/`

测试代码和资源文件目录，通常使用 JUnit 或其他测试框架编写单元测试和集成测试。

#### `java/`

测试代码，按照与主代码相同的包结构组织。

#### `resources/`

测试资源文件，如测试用的 JSON 数据。

---
