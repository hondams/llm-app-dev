# 命名規約

## 基礎知識

### case styles（命名規則のケース）

- snake_case  
  - 小文字＋アンダースコア区切り。例: user_name

- SCREAMING_SNAKE_CASE（UPPER_SNAKE_CASE）  
  - 大文字＋アンダースコア区切り。例: USER_NAME

- camelCase（lowerCamelCase）  
  - 先頭小文字、以降の語頭を大文字。例: userName

- PascalCase（UpperCamelCase / StudlyCaps）  
  - 全ての語の語頭を大文字。例: UserName

- kebab-case（dash-case / hyphen-case）  
  - 小文字＋ハイフン区切り。例: user-name

- Train-Case（Capitalized-Kebab-Case）  
  - 各語の頭が大文字＋ハイフン区切り。例: User-Name

- dot.case（period.case）  
  - 小文字＋ドット区切り。例: user.name

- path/case（slash-case）  
  - 小文字＋スラッシュ区切り。例: user/name

- flatcase（lowercase no separator）  
  - 区切りなし小文字。例: username

- UPPERCASE（no separator）  
  - 区切りなし大文字。例: USERNAME

- Title Case（Title Case / Headline Case）  
  - 各語の頭を大文字、スペース区切り。例: User Name

- Sentence case  
  - 文頭のみ大文字、スペースあり。例: User name

- Train_Snake / Pascal_Snake（混在スタイル）  
  - 例: User_Name（混合は推奨されないが存在）

- kebab_snake 混合（非推奨の混在例）  
  - 例: user-name_value（避けるべき）

- Hungarian notation（プレフィックス型）  
  - 型や用途の接頭辞を付与。例: strUserName, iCount

- Screaming-Hyphen-Case（大文字＋ハイフン）  
  - 例: USER-NAME（稀）

## 用途別命名規約 

以下、主要な用途ごとに「推奨ケース / 許容ケース / 非推奨」を網羅的に示します。例と簡潔な理由を併記します。

# データベース（SQL）
- テーブル名：snake_case（複数形）  
  - 例: users, order_items  
  - 理由: SQLで読みやすく、スキーマ全体で一貫しやすい。  
- カラム名：snake_case（単数形）  
  - 例: user_id, created_at  
- 主キー/外部キー：snake_case（例: id, user_id）  
- インデックス/制約名：snake_case プレフィックス（例: idx_users_email, pk_orders）  
- ビュー：snake_case または vw_ プレフィックス（必要時のみ）  
  - 例: vw_user_stats  
- Enum 値（DBレベル）：SCREAMING_SNAKE_CASE（または小文字列）  
  - 例: ACTIVE, INACTIVE  
- スキーマ名 / 名前空間：小文字（snake_case 推奨）  
- 非推奨：PascalCase / camelCase / 混在スタイル（テーブル・カラムでの混在は避ける）

# バックエンド（コード / ドメイン）
- エンティティ / ドメインクラス：PascalCase（単数形）  
  - 例: User, OrderItem  
- リポジトリクラス：PascalCase + Repository  
  - 例: UserRepository  
- サービスクラス：PascalCase + Service（またはドメイン名）  
  - 例: OrderService  
- DTO / Request / Response 型名：PascalCase + 接尾辞（用途明示）  
  - 例: UserRequest, UserResponse  
- クラスメソッド：camelCase（言語慣習に合わせる）  
  - 例: getUser, createOrder  
- 変数/ローカル変数：camelCase（JS/Java等）または言語の慣習に従う  
- 定数（コード内）：SCREAMING_SNAKE_CASE  
  - 例: MAX_RETRY_COUNT  
- パッケージ/モジュール名：言語慣習に従う（Java: lower.case.path、Python: snake_case、npm: kebab-case）  
- 非推奨：ハンガリアン記法、混在スタイル

# OpenAPI / REST API
- パス（URL）：kebab-case（複数形、小文字）  
  - 例: /users, /order-items  
  - 理由: 読みやすくブラウザで扱いやすい。  
- パスパラメータ名：camelCase または snake_case をプロジェクトで統一  
  - 例: /users/{userId} あるいは /users/{user_id}（どちらかに統一）  
- クエリパラメータ / ヘッダ名：camelCase（JS系）または snake_case（APIガイドラインに依存）で統一  
- components/schemas（モデル名）：PascalCase（単数形）  
  - 例: User, OrderItem  
- JSON プロパティ名（schemaのプロパティ）：camelCase（Web/JSクライアント向け）または snake_case（バックエンド・DB向け）—チームで統一  
  - 例: firstName（camelCase） / first_name（snake_case）  
- operationId / メソッド名：camelCase（動詞+資源）  
  - 例: getUser, listOrderItems  
- 非推奨：URLに動詞を入れる、パス名でケース混在

# JSON / フロントエンド
- JSON キー（APIのボディ/レスポンス）：camelCase（推奨）または snake_case（統一が最優先）  
  - 例: userName / user_name  
- CSS クラス名：kebab-case（BEM 等のルールに従う）  
  - 例: btn-primary, header__title  
- HTML id / data-属性：kebab-case 推奨

# ファイル名 / ディレクトリ
- ソースファイル（言語依存）：
  - JS/TSコンポーネント: kebab-case または PascalCase（React コンポーネントは PascalCase）  
  - Python: snake_case.py  
  - Java: PascalCase.java（クラス名と一致）  
- ディレクトリ名：kebab-case or snake_case（プロジェクトで統一）  
- 非推奨：空白や大文字混在（Windowsで扱いにくくなることがある）

# 環境変数 / コンフィグ
- 環境変数：SCREAMING_SNAKE_CASE（アンダースコア）  
  - 例: DATABASE_URL, REDIS_HOST  
- 設定キー（YAML/JSON内）：プロジェクト標準に従う（通常は camelCase または snake_case）

# CLI / コマンド引数 / Kubernetes リソース
- CLI オプション：kebab-case（一般的）  
  - 例: --config-file  
- Kubernetes リソース名：kebab-case（小文字）  
  - 例: my-app-deployment

# 一般的なガイドライン（短く）
- 最重要：プロジェクト内で一貫させること。  
- APIはクライアント（JS）を意識してcamelCaseが多いが、DBはsnake_caseが一般的。  
- 名前は意味的に明確に、接頭辞/接尾辞は用途が明確な場合のみ限定使用。  
- 混在（camelCase と snake_case を混ぜる等）は避ける。

必要なら、あなたのプロジェクト（c:\projects\github\hondams\llm-app-dev）向けに「用途別推奨一覧」を一枚の設定ファイル（markdown）で作成しますか？

-----

snake_case

小文字＋アンダースコア区切り。例: user_name
SCREAMING_SNAKE_CASE（UPPER_SNAKE_CASE）

大文字＋アンダースコア区切り。例: USER_NAME
camelCase（lowerCamelCase）

先頭小文字、以降の語頭を大文字。例: userName
PascalCase（UpperCamelCase / StudlyCaps）

全ての語の語頭を大文字。例: UserName
kebab-case（dash-case / hyphen-case）

小文字＋ハイフン区切り。例: user-name
Train-Case（Capitalized-Kebab-Case）

各語の頭が大文字＋ハイフン区切り。例: User-Name
dot.case（period.case）

小文字＋ドット区切り。例: user.name
path/case（slash-case）

小文字＋スラッシュ区切り。例: user/name
flatcase（lowercase no separator）

区切りなし小文字。例: username
UPPERCASE（no separator）

区切りなし大文字。例: USERNAME
Title Case（Title Case / Headline Case）

各語の頭を大文字、スペース区切り。例: User Name
Sentence case

文頭のみ大文字、スペースあり。例: User name
Train_Snake / Pascal_Snake（混在スタイル）

例: User_Name（混合は推奨されないが存在）
kebab_snake 混合（非推奨の混在例）

例: user-name_value（避けるべき）
Hungarian notation（プレフィックス型）

型や用途の接頭辞を付与。例: strUserName, iCount
Screaming-Hyphen-Case（大文字＋ハイフン）

例: USER-NAME（稀）


## 


- テーブル名：複数形 英語。Prefix、Suffixなし
- Entity：「単数形 + PascalCase」。Prefix、Suffixなし
- Repository は「単数形 + PascalCase（Aggregate名）＋Repository」
- URLのパス（URL）は「複数形 + 小文字 + ハイフン（kebab-case）」を推奨（例: /users, /order-items）。一貫性が最重要。
- openapiのJsonスキーマ（components/schemas）は「単数形 + PascalCase」（例: User, OrderItem）。Prefix、Suffixなし
  

- Prefix/Suffix の扱い（補足）
  - 原則：テーブル／エンティティ名に一律のプレフィックス／サフィックスは付けない（例: tbl_, _tbl, Model は避ける）。
  - 許容例（用途が明確な場合のみ）
    - ビュー: vw_user_stats
    - 一時テーブル: tmp_import_jobs
    - 監査/履歴: orders_audit / order_history
    - DTO/Request/Response: UserRequest, UserResponse（コード上の用途識別のため）
  - スキーマ（DB）分離が可能な場合はスキーマ／名前空間を優先（例: reports.user_stats）。

## OpenAPI / REST API 命名（追加）

- パス（URL）
  - リソースは複数形の名詞（例: /users, /orders）。
  - 全て小文字。推奨ケース: kebab-case（例: /order-items）。プロジェクトで snake_case を選ぶ場合は統一する。
  - 動詞をパスに含めない（例: /users に対して GET/POST/PUT/DELETE を使う）。
  - バージョンはベースパスで管理（例: /api/v1/users）。リソース名にバージョンを含めない。

- パスパラメータ
  - 単数形で明確に（例: /users/{userId}）。パラメータ命名は camelCase か snake_case を統一。

- components/schemas（モデル）
  - 単数形、PascalCase（例: User, OrderItem）。
  - リクエスト/レスポンスで区別する場合のみ接尾辞を使用（例: UserRequest, UserResponse）。
  - 集合・ページング用は一貫した命名（例: UserList, PaginatedUserList）。

- operationId / メソッド名
  - 動詞＋単数リソース（例: getUser, listUsers）。一意にする。

- タグ・表示名
  - ドメイン言語（Ubiquitous Language）で統一。複数形でも可だがチームで統一。

- Prefix/Suffix の扱い（API）
  - パス・スキーマ名に一律の接頭・接尾辞を付けるのは避ける（例: tbl_ や api_ は不可）。
  - 用途ごとの明確な目的がある場合のみ限定的に付与（例: AuthLoginRequest 等）。

- 運用
  - 命名規約を OpenAPI テンプレート／CI でチェック。自動生成（client/server）との整合性を検証。