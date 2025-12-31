# Python

## 環境構築

- Pythonのパッケージマネージャー（uv）のインストール

```powershell

powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"

```

### 3. 主な使い方

`uv` には大きく分けて「プロジェクト管理モード」と「pip互換モード」の2つがあります。現在は**「プロジェクト管理モード」**が推奨されています。

#### ① プロジェクトを始める

新しいフォルダを作成して初期化します。

```bash
mkdir my-project
cd my-project
uv init
```

これだけで `pyproject.toml`（設定ファイル）が作成されます。

#### ② Pythonのバージョンを指定・インストール

Python 3.12 を使いたい場合、自分でダウンロードしなくても `uv` が勝手にやってくれます。

```bash
uv python install 3.14
uv python pin 3.14
```

#### ③ パッケージを追加する

`requests` を追加してみます。

```bash
uv add requests
```
これだけで、**自動的に仮想環境（.venv）が作られ**、パッケージがインストールされ、`pyproject.toml` と `uv.lock` が更新されます。

#### ④ プログラムを実行する

仮想環境を `source .venv/bin/activate` する必要はありません（しても良いですが）。

```bash
uv run main.py
```

`uv run` を使うと、適切な環境でスクリプトを実行してくれます。

#### ⑤ ツールを一時的に実行する (uvx)

`pipx` のように、ライブラリをインストールせずにツールだけ実行したい時に便利です。

```bash
uvx ruff check  # Ruffをインストールせずに実行
uvx cowsay hello
```

---

### 4. 従来のツールとの対応表

| やりたいこと | 従来のツール | uv でのコマンド |
| :--- | :--- | :--- |
| Pythonのインストール | pyenv / 公式インストーラー | `uv python install` |
| パッケージのインストール | pip install | `uv pip install` または `uv add` |
| 仮想環境の作成 | python -m venv | `uv venv` (自動で行われる) |
| プロジェクト管理 | poetry / pipenv | `uv init` / `uv add` |
| ツールの実行 | pipx | `uvx` |

---

## 環境構築（VS Code）

```powershell

winget install -e --id Microsoft.VisualStudioCode

```

ms-python.python
ms-python.debugpy
charliermarsh.ruff
astral-sh.ty
usernamehw.errorlens
oderwat.indent-rainbow
njpwerner.autodocstring

## 参考URL

- https://www.langchain.com/
- https://github.com/langchain-ai/langchain
- https://docs.langchain.com/oss/python/langchain/overview
  - Python 3.10 以降

- https://zenn.dev/loglass/articles/b5fb24f0718775
  -  Edge → Shell の境界（非決定性の固定化）
  -  Shell → Core の境界（副作用の分離）
  -  GOAP による境界の抽象化（最適な手続き選択）
- https://zenn.dev/loglass/articles/797fe3fc60399a
  - ありもの（Cursor）から始め、Bedrock Agentを経て、最終的にLangChainへと移行
- https://zenn.dev/loglass/articles/08a238976938a9
  - 万能なエージェントから、確実なワークフローへ
- https://zenn.dev/loglass/articles/c7f4499ec8320b
  - AI エージェント開発のデザインパターン
    - Level 1: 単一プロンプト
      - 0. Single Prompt / Simple RAG
    - Level 2: Workflow（決定論的な処理）
      - 1. Prompt Chaining（連鎖）
      - 2. Routing（振り分け）
      - 3. Parallelization（並列化）
      - 4. Orchestrator-Workers（指揮者と作業者）
    - Level 3: Agent（動的・自律的な処理）
      - 5. ReAct (Reasoning + Acting)
        - 「思考（Thought）→ 行動（Action）→ 観察（Observation）」のループを回す。
      - 6. Planner-Executor（計画と実行の分離）
        - 最初に全体の計画（Plan）を立て、Executor がそれを順番に消化する。ReAct の弱点である「近視眼的な行動」を防ぐ。
    - Utility & Governance（品質と信頼性のための機能）
      - 7. Reflection / Critic（内省）
        - 生成された出力に対して、LLM 自身が「批評」を行い、修正版を作成する。
      - 8. Evaluator-Optimizer（評価と最適化）
        -  Evaluator（評価者）が出力を定量的に評価し、基準を満たすまで Optimizer（最適化者）がループで改善する。
      - 9. Human-in-the-Loop (HITL)
        - 重要な意思決定の前に人間の承認を挟む。
      - 10. Memory-Augmented（長期記憶）
        - 会話履歴だけでなく、ユーザーの好みや過去の事実をデータベース（e.g., Vector DB）に保存・参照する。

- https://zenn.dev/loglass/articles/1684a451ce06e1
  - AIで生成した、ArchUnitを使った、カスタムLintで、生成したソースをチェック
- https://zenn.dev/erukiti/articles/2512-full-ai-cofing
  - 指示を増やせば、未知は減少しますが、矛盾は増えます。
    - つまり、プロンプトを頑張れば頑張るほど、矛盾を抱えます。
    - 矛盾と未知をいい案配でなんとかしなければならない。
  - コーディングエージェントは、ユーザーが命令したことだけではなく、LLM が考えたトンチキな推論や、LLM が誤った方法で観測したデータも「絶対に正しいと思い込むことがある」という悪癖を持っています。
    - そのセッション(コンテキスト)は汚染されているので捨てる
    - 観測データが、より正しいものになる言語・ライブラリを選定する（要するにマイナーな言語・ライブラリを使ってると、誤った観測をしがちである）
    - Playwright MCP のように、適切に状況を観測できる MCP を導入して正しいコンテキストを作れるようにする（ただし、限度はある。過信すべきではない）
    - 「批判系」の MCP を導入する。思考フレームワークを拡張することで、過ちを疑うようにはなる（ただし、限度はある。過信すべきではない）
  - AI が吐き出す設計書は毒が紛れ込んでいるため人力レビューが必須です。
    - AI コーディングにおいて、コードの品質は、人間による不断の努力と、いくつもノウハウによってのみ実現できる
  - LLM は人間基準から見ると、要約が下手くそすぎる
    - 前述のような勝手な解釈をして、落とすべき要素をランダムで勝手な解釈をして選びがち
    - 常識的な推論に失敗しがち
    - 具体例
      - 設計書を書かせたら、重要なことがやたら抜け落ちる
      - コンテキストが増大しすぎて memory compaction が発生したら、処理がおかしくなる
      - thinking/reasoning でなんか変なこと言い出して、その後はそれに引っ張られる
      - いつに間にか「目的」や「最初の指示」を忘れる
      - ドキュメントやコメントに書いてほしいことが、そぎ落とされがち
      - ユニットテストも、人間が重要だと思うことを端折りたがる
    - 不利になる言語やライブラリを選定することは強く非推奨
    - linter が必須
    - レイヤー構造は必須
    - テストは、カバレッジ 100%が望ましい
      - 「絶対にテストしたいこと」は明示的に指定をする必要がある（勝手に消されることが多い）
      - 大局観がどうしても足りないため、結合テストが最重要
    - コメントと TSDoc を手厚く書かせる
      - TSDoc には、目的・内容・注意事項を書かせる
      - テストは、コメントを過剰なほど手厚く書かせるのが必須
      - 結合テストが生命線です。レビューコストの大半をここに使ってください。
      - 「プロジェクト外のジュニアエンジニアでも絶対に読めるようにすること」という指示を与える
      - 前提条件、事前条件、検証項目は必ずコメントで文書化させる
    - リファクタ観点で、テストの変更と、実装の変更を分離する
  - AI コーディングの問題点
    - レビューに時間がかかりすぎる
    - レビューに神経を使い過ぎる。どうしても抜け漏れは生じるし、ストレスもたまる
    - 何をどう頑張っても、一定の確率で品質の悪いコードを書くことがあるため、ストレスがたまる
    - AI の世界は進化が早すぎて、情報が洪水となって押し寄せてくる
    - そもそも AI と会話してるとめちゃくちゃイライラさせられることがある

```markdown
- ソースコードを変更するときはユニットテストの変更を禁じる
- ユニットテストを変更するときはソースコードの変更を禁じる
- 適切にフェイズ分割を行え
```

- https://zenn.dev/erukiti/articles/2510-ai-coding
  - 
- https://speakerdeck.com/yuitosato/cursor-rules-for-coding-styles-domain-knowledges-and-operations
  - Cursorルール
    - コード規約（How
      - レイヤ×コンポーネント×（実装、テスト）
    - ドメイン知識（What）
      - ドメインに即した文脈、背景、仕様の情報
        - 仕様書
        - ドメインモデル図（集約はどの範囲やモデルとモデルの関係性、ふるまいなど）
        - ⽤語集
        - ユースケース図など
      - DDDなどで、コードで仕様を表現できれば、ドメイン知識のルールが減る。
        - コードコメントに仕様を記述する。
          - テストの入力にもなる。
    - オペレーション（ルーチンワークを自動化するためのルール）
      - ルーチンワークに関するルール
        - 要求定義、要件定義、
        - タスク管理
          - 設計、実装、コンパイル、静的解析、自動テスト、リファクタリング
        - コードレビュー、手動テスト
        - マージ、リリース
        - ユーザーフィードバック
- https://cursor.com/ja/docs/context/rules#-1
  - プロジェクトルール
    - コードベースに関するドメイン固有の知識を表現する
    - プロジェクト固有のワークフローやテンプレートを自動化する
    - スタイルやアーキテクチャに関する方針を標準化する
  - ベストプラクティス
    - よいルールは、焦点が明確で、実行可能で、スコープが適切に絞られています。
    - ルールは 500 行以内に収める
    - 大きなルールは分割し、複数の組み合わせ可能なルールにする
    - 具体的な例や参照ファイルを提示する
    - あいまいな指示は避ける。社内ドキュメントのように明確に記述する
    - チャットで同じようなプロンプトを繰り返す場合は、ルールを再利用する
- https://zenn.dev/loglass/articles/c356dbc3062137
  - 「レビューと改善を3回繰り返して」を、プロンプトに組み込む。
- https://zenn.dev/loglass/articles/0bbe0d693594f1
  - 各フェーズにおけるアウトプットのレビューコストが高い
  - spec-kit が真価を発揮するタスクが限定的
    - 頭の中で実装の筋道ができているもの
    - 頭の中で実装の筋道ができているが、一部検討が必要なもの
    - 仕様は整理できているが、どのように実装するかは実装してみないとわからないもの
- https://tech.algomatic.jp/entry/2025/09/22/143931
  - SDDツール比較
- https://zenn.dev/loglass/articles/f9a2f78cc70826
  - DDD
- AGENTS.mdのサンプル
  - https://zenn.dev/erukiti/articles/2512-full-ai-cofing
- https://qiita.com/Nozomuts/items/430261fa996cba5fd466
  - GitHub Copilotの使い方メモ
- https://docs.github.com/en/copilot
  - 