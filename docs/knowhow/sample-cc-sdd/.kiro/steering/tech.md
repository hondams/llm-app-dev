# Technology Stack

## Architecture

Spring Boot 3ベースの非同期バッチアプリケーション。レイヤードアーキテクチャ + DDD的ドメイン層分離。

## Core Technologies

- **Language**: Java 21（Virtual Thread有効化）
- **Framework**: Spring Boot 3.5.3、Spring Batch 5.x
- **Runtime**: Maven 3.x、Spring Boot Starter Parent
- **Database**: H2（dev）/ PostgreSQL（production）
- **Messaging**: Spring JMS + Amazon SQS Java Messaging Library
- **HTTP Client**: Spring WebFlux WebClient

## Key Libraries

- **Persistence**: MyBatis 3.0.4（バッチ処理/カーソルリーダー対応）
- **AWS Integration**: Spring Cloud AWS 3.4.0（Parameter Store/Secrets Manager/CloudWatch Metrics）
- **Resilience**: Spring Cloud Circuit Breaker + Resilience4J
- **Observability**: Micrometer + OTEL Tracing、Spring Boot Actuator
- **Validation**: TERASOLUNA CodePoints（JIS文字セット検証）
- **Local Fakes**: ElasticMQ 1.6.12（SQS）、H2 Database（RDB）
- **Code Generation**: Lombok、MapStruct（implied）

## Development Standards

### Type Safety
- Java 21のRecord/Sealed Classを活用
- Lombokによるボイラープレート削減

### Code Quality
- Virtual Thread活用（`spring.threads.virtual.enabled=true`）
- Transaction rollback on commit failure有効化
- HikariCP autoCommit=false（PostgreSQLカーソル最適化）

### Testing
- Spring Boot Test標準サポート
- Profile分離によるテスト容易性

## Development Environment

### Required Tools
- Java 21+
- Maven 3.x
- IDE Plugin: Lombok、MapStruct

### Common Commands
```bash
# Dev (ElasticMQ/H2): mvnw spring-boot:run
# Production: mvnw spring-boot:run -Dspring-boot.run.profiles=production
# CLI実行: mvnw spring-boot:run -Dspring-boot.run.profiles=dev,log_default,command -Dspring-boot.run.arguments="param01=aaa param02=bbb"
# Build: mvnw clean package
# Test: mvnw test
```

## Key Technical Decisions

### 非同期処理方式
- **SQS + Spring JMS**: メッセージベース非同期実行（標準モード）
- **ElasticMQ**: ローカル開発用SQS互換サーバー組み込み起動
- **ACKタイミング**: ジョブ開始時/完了時選択可能（長時間バッチ対応）

### バッチ実行モデル
- **Spring Batch**: タスクレット/チャンク両対応
- **CLI起動**: `SampleBatchApplicationForCommandLine`による直接実行
- **重複実行防止**: Spring Batch標準機能（JobInstance + JobParameters）

### マルチプロファイル設計
- **dev**: ElasticMQ、H2、S3 Fake（ローカルファイルシステム/LocalStack/MinIO選択可）
- **production**: SQS、PostgreSQL、S3
- **commandline**: CLI実行用プロファイル（JMSリスナー無効化）

### データアクセス
- **MyBatis**: SQLマッピング、Spring Batchリーダー/ライター統合
- **Repository層**: domain/repository配下にインターフェース配置
- **トランザクション**: READ_COMMITTED分離レベル、rollback-on-commit-failure

### 外部連携
- **REST API**: WebClientベース、Circuit Breaker適用
- **S3アクセス**: AWS SDK S3、ローカルFake対応
- **設定外部化**: Parameter Store/Secrets Manager（productionのみ）

### 監視・運用
- **Metrics**: Micrometer + CloudWatch Metrics（production）
- **Tracing**: OTEL Bridge
- **Logging**: Logback（default format/container json format切替）
- **Health Check**: Spring Boot Actuator

---
_Document standards and patterns, not every dependency_
