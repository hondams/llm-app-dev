# Project Structure

## Organization Philosophy

**レイヤードアーキテクチャ + ドメイン分離**: 
- `com.example.batch.*` - アプリケーション層（Job/Infra/Domain）
- `com.example.fw.*` - 共通フレームワーク層（batch/common）
- パッケージ単位での関心事分離

## Directory Patterns

### Application Layer (`src/main/java/com/example/batch/`)
**Purpose**: バッチアプリケーション本体  
**Structure**:
- **Root**: 設定クラス（`JobConfig.java`、`SQSConfig.java`、`InfraConfig.java`）、起動クラス
- **domain/**: ドメインロジック層
  - `model/` - エンティティ、Value Object
  - `repository/` - リポジトリインターフェース
  - `rule/` - ビジネスルール
  - `message/` - メッセージID定数
  - `sharedservice/` - 共有ドメインサービス
- **job/**: ジョブ実装層
  - `job001/`、`job002/`... - ジョブIDごとのパッケージ（Step/Tasklet/Listener）
  - `common/` - ジョブ共通部品（Record、Validator等）
- **infra/**: インフラストラクチャ層
  - `repository/` - リポジトリ実装（MyBatis Mapper XML連携）
  - `httpclient/` - 外部API呼び出し
  - `resource/` - S3等リソースアクセス

### Framework Layer (`src/main/java/com/example/fw/`)
**Purpose**: 再利用可能フレームワーク  
**Structure**:
- **batch/**: バッチ固有フレームワーク（Exception Handler、Listener、Async Config）
- **common/**: 汎用フレームワーク（SystemDate、Validation、ObjectStorage、Exception）

### Resources (`src/main/resources/`)
**Purpose**: 設定ファイルとデータ  
**Structure**:
- `application*.yml` - Spring Boot設定（プロファイル分離）
- `messages*.properties` - メッセージリソース（i18n対応）
- `schema-h2.sql`、`data-h2.sql` - H2 DB初期化スクリプト
- `logback-spring.xml` - ロギング設定
- `com/example/batch/domain/` - MyBatis Mapper XMLファイル

### Test Resources (`src/test/java/`)
**Purpose**: テストコード  
**Pattern**: `*Tests.java`、`*Gens.java`（メッセージID定数生成）

### Build Artifacts (`target/`)
**Note**: ビルド成果物、Git管理外

### Infrastructure (`k8s/`, `localstack/`, `buildspec*.yml`, `Dockerfile*`)
**Purpose**: デプロイ・インフラ設定  
**Files**: 
- Kubernetes manifest、LocalStack compose、CodeBuild/CodePipeline設定、Dockerイメージ

## Naming Conventions

- **Packages**: 小文字スネークケース（`com.example.batch.job.job001`）
- **Classes**: PascalCase（`SampleBatchApplication`、`TodoRecord`）
- **Config Classes**: 役割 + `Config` サフィックス（`JobConfig`、`SQSConfig`）
- **Job Packages**: `job` + 数字3桁（`job001`、`job002`）
- **Application Classes**: `SampleBatchApplication`（通常）、`SampleBatchApplicationForCommandLine`（CLI用）

## Import Organization

```java
// Spring Framework imports
import org.springframework.batch.core.*;
import org.springframework.context.annotation.*;

// AWS SDK imports
import software.amazon.awssdk.services.s3.*;

// Application imports
import com.example.batch.domain.model.User;
import com.example.fw.common.exception.SystemException;
```

**Path Aliases**: N/A（標準Javaパッケージ構造）

## Code Organization Principles

### レイヤー依存ルール
- **Job → Domain → Infra**: 単方向依存
- **Job → Framework**: フレームワーク機能利用
- **Domain → Framework**: ドメインロジックは共通例外・バリデーションのみ利用
- **Infra → Framework**: インフラ層はフレームワーク完全利用可

### ジョブ実装パターン
- **ジョブIDパッケージ分離**: `job001/`、`job002/` - 各ジョブの独立性確保
- **タスクレット vs チャンク**: ジョブ特性に応じて選択（job001=Tasklet、job002=Chunk）
- **設定分離**: `JobConfig.java`でBean定義、各ジョブで`@StepScope`活用

### 設定クラス配置
- **Root直下**: `JobConfig`、`SQSConfig`、`InfraConfig` - 機能別設定集約
- **ComponentScan**: フレームワーク層のパッケージマーカークラス参照

### プロファイル分離戦略
- **application.yml**: 共通設定
- **application-dev.yml**: ローカル開発（ElasticMQ、H2、S3 Fake）
- **application-production.yml**: 本番環境（SQS、PostgreSQL、S3）
- **application-commandline.yml**: CLI実行用

### メッセージリソース
- **messages.properties**: アプリケーション固有メッセージ
- **messages-fw-*.properties**: フレームワークメッセージ
- **messages-common.properties**: 共通メッセージ

---
_Document patterns, not file trees. New files following patterns shouldn't require updates_
