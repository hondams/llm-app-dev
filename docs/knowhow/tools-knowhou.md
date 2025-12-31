## Tools knowhow

## LLMエージェント

### おすすめスタック（用途別）

- プロトタイプ（早く試す）: LangChain（Python）またはlangchain.js + OpenAI（API） — 高レベルAPIでAgentやRAGが短時間で作れる。
- 本番向けクラウド（安定・スケール）: LangChain + OpenAI/Anthropic/Cohere + QdrantまたはPinecone（ベクタDB） + Prefect/Airflow（ワークフロー）。
- ローカル／オンプレ（機密データ）: transformers/vLLM + llama.cpp（軽量推論） + FAISS/Qdrant（ベクタ検索）。
- RAG（長文知識参照が重要）: LlamaIndex（一時的にGPT Indexと呼ばれた）または- Haystack + ベクタDB（Weaviate/Milvus/Qdrant）。
- Agent（ツール操作や外部API連携）: LangChain Agents（Python/JS）またはAgentic系（Auto-GPT派生） — ツール呼び出し・思考ループが組みやすい。
- 研究/微調整（カスタムモデル）: Hugging Face Transformers + Accelerate + PEFT/bitsandbytes（LoRA等で効率的に微調整）。

### 主要コンポーネントと推奨ライブラリ

- LLMクライアント: openai SDK, anthropic SDK, cohere — プロバイダ依存の呼び出し用。
- Agentフレームワーク: LangChain（エージェント、チェイン、ツール統合が一式）。
- RAG / インデックス: LlamaIndex, Haystack — ドキュメント取り込み・要約・検索の管理。
- ベクタDB（埋め込みストレージ）: Pinecone, Qdrant, Weaviate, Milvus, FAISS（オンメモリ） — 性能/運用性で選択。
- 埋め込み生成: OpenAI embeddings, sentence-transformers（Hugging Face） — 精度とコストのバランスで選ぶ。
- ローカル推論: llama.cpp, transformers, vLLM — GPU/CPUやモデルサイズに応じて。
- オーケストレーション / 並列化: Ray, Prefect, Celery — 大量リクエストや長時間ジョブ向け。
- 評価 / モニタリング: LangSmith, OpenAI Evals, カスタムメトリクス（ログ保存はSentryやPrometheus）。
セキュリティ・ガバナンス: 入力サニタイズ、アクセス制御、データマスキングを自前で追加。

### 選ぶときの判断基準（短く）

- 言語: PythonならLangChainエコシステムが最も豊富。Node.jsならlangchain.js。
- モデル設置場所: クラウドAPIで速く簡単／ローカルで低遅延・機密保持。
- コストとレイテンシ: 大量埋め込みや推論はコストが積み上がるのでembeddingsプロバイダとバッチ戦略を検討。
- 拡張性: ツールコールやチェインの複雑さが増すならLangChain Agentsが便利。
- 運用性: ベクタDBは運用負荷（管理・バックアップ）を見て選ぶ（Pineconeはマネージドで楽）。

### 短い推奨スタック（初心者→本番）

- 初心者プロトタイプ: LangChain (Python) + OpenAI + Qdrant
- 本番（クラウド）: LangChain + Anthropic/OpenAI + Pinecone + Prefect + モニタリング（LangSmith）
- ローカル完全オフライン: transformers/vLLM + llama.cpp（モデルファイル） + FAISS/Qdrant

### 実装のヒント（すぐ使える）

- まずはLangChainのAgentとToolを使い、外部API呼び出しをToolとして登録する形で始めると拡張しやすい。
- RAGをする場合は文書の分割と埋め込み生成を最初に設計する（チャンクサイズ、メタデータ）。
- ローカルモデルを使うときはGPUメモリ制約に注意し、bitsandbytesやLoRAで微調整を行う。
次に何しましょうか？あなたの用途（例：チャットエージェント、ドキュメント検索エージェント、外部ツール操作エージェント）を教えてください。リポジトリ向けに具体的なスターター実装（LangChainエージェントの雛形など）を作成できます。


