# Product Overview

Spring Bootベースの非同期/バッチ処理アプリケーション。SQSメッセージング経由でバッチジョブを非同期実行し、REST API連携やファイル処理を行う。

## Core Capabilities

- **非同期ジョブ実行**: Spring JMS + AWS SQSによるメッセージベース非同期処理
- **バッチ処理**: Spring Batchによる大量データ処理（タスクレット/チャンクモデル対応）
- **コマンドライン実行**: SQSメッセージング以外にCLIによる直接ジョブ起動
- **マルチ環境対応**: ローカル開発（ElasticMQ/H2DB）とプロダクション（SQS/PostgreSQL）の切り替え
- **REST API連携**: sample-backendへのWebClient経由API呼び出し（Circuit Breaker対応）

## Target Use Cases

- **画面起動バッチ**: sample-bffからのユーザ操作によるバッチ処理（TODO一括登録など）
- **スケジュールバッチ**: sample-schedulelaunchからの定期実行依頼
- **コンテナ実行**: ECS/AWS Batchでのコマンドライン起動
- **長時間処理**: SQS可視性タイムアウト考慮のジョブ開始時ACK対応

## Value Proposition

- **ローカル完結開発**: AWS環境不要（ElasticMQ/H2DB/S3 Fake）
- **柔軟な実行モデル**: メッセージング/CLI/コンテナの3方式対応
- **エンタープライズ対応**: 例外ハンドリング、メトリクス、分散トレーシング標準装備
- **プロファイル分離**: dev/production環境の明確な設定分離

---
_Focus on patterns and purpose, not exhaustive feature lists_
