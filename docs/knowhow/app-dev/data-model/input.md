# 入力

## 住所入力

- https://blog.kenall.jp/entry/address-form-best-practice
- https://form.run/media/contents/form-creation-tools/address-entry-form/


郵便番号
都道府県
市区町村・丁目
番地
建物名・部屋番号

### 1. `autocomplete` 属性の最適化（ブラウザへの確実な指示）

ブラウザは `name` 属性や `id` 属性、周囲のテキストも参考にしますが、最も強力なのは `autocomplete` 属性です。日本の住所を各ブラウザに正しく認識させるためのマッピングは以下がベストプラクティスです。

| 項目 | `autocomplete` 値 | ブラウザの挙動への配慮 |
| :--- | :--- | :--- |
| **郵便番号** | `postal-code` | Chrome/Safariが最も確実に認識します。 |
| **都道府県** | `address-level1` | `select`要素でも `text`要素でも機能します。 |
| **市区町村** | `address-level2` | `address-line1` と分ける場合、ここに「〇〇市〇〇区」が入ります。 |
| **町名・番地** | `address-line1` | 自動補完ではここがメインの住所として扱われます。 |
| **建物名・号室** | `address-line2` | 二行目の住所として、ブラウザは「マンション名」などを入れようとします。 |

### 2. フィールド分割の「妥協点」を見つける

ブラウザ（特に海外製）は、住所を「City（市区町村）」と「Street（番地）」に明確に分けるのが苦手です。

*   **改善案:** **「市区町村」と「番地」をあえて一つの入力欄にまとめる**。
    *   理由：Google Chromeの自動補完では、日本の住所を「市区町村＋番地」のセットで記憶していることが多く、細かく分断されたフォームだと正しく流し込めない（あるいは番地が消える）エラーが頻発するためです。

### 3. モバイルブラウザでのキーボード切り替え（`inputmode`）

iOS SafariやAndroid Chromeでは、キーボードの切り替えストレスが離脱に直結します。

*   **郵便番号:** `type="text"` ＋ `inputmode="numeric"` を使用。
    *   ※ `type="number"` は、全角数字を入力した際にブラウザ側でバリデーションエラーになりやすく、また「カンマ」や「ドット」が含まれるリスクがあるため避けます。

### 4. ブラウザ互換性を高めた実装例

```html
<form action="/submit" method="post">
  <fieldset>
    <legend>配送先住所</legend>

    <!-- 郵便番号: inputmodeで数字キーパッドを強制 -->
    <div class="form-group">
      <label for="zip">郵便番号</label>
      <input type="text" id="zip" name="zip" 
             autocomplete="postal-code" 
             inputmode="numeric" 
             pattern="\d{3}-?\d{4}" 
             placeholder="123-4567" required>
      <button type="button" id="search-address">住所検索</button>
    </div>

    <!-- 都道府県: select要素でもautocompleteは有効 -->
    <div class="form-group">
      <label for="pref">都道府県</label>
      <select id="pref" name="pref" autocomplete="address-level1" required>
        <option value="">選択してください</option>
        <option value="東京都">東京都</option>
        <!-- 各都道府県 -->
      </select>
    </div>

    <!-- 市区町村と番地を統合: ブラウザの自動補完ミスを防ぐ -->
    <div class="form-group">
      <label for="addr-l1">市区町村・番地</label>
      <input type="text" id="addr-l1" name="addr-l1" 
             autocomplete="address-line1" 
             placeholder="例：千代田区麹町1-2-3" required>
    </div>

    <!-- 建物名: 任意項目とし、address-line2を割り当て -->
    <div class="form-group">
      <label for="addr-l2">建物名・部屋番号（任意）</label>
      <input type="text" id="addr-l2" name="addr-l2" 
             autocomplete="address-line2" 
             placeholder="例：〇〇ビル 101号室">
    </div>
  </fieldset>
</form>
```

---

### 5. 各ブラウザ特有の挙動への対策

#### ① Safari (iOS/macOS) の「連絡先」補完対策
Safariは、フォームの `label` テキストを非常に重視します。「住所1」「住所2」というラベルだけだと、どこに何を入れればいいかSafariが混乱することがあります。
*   **対策:** `label` には「番地」や「建物名」といった具体的な言葉を含めます。

#### ② Chrome の「住所の重複入力」
Chromeは、一つのページに複数の住所フォーム（例：注文者住所と配送先住所）があると、補完が混ざることがあります。
*   **対策:** `section-` プレフィックスを `autocomplete` に付けます。
    *   `autocomplete="section-billing address-line1"`
    *   `autocomplete="section-shipping address-line1"`

#### ③ 全角・半角問題（日本独自の重要対策）
多くの日本のユーザーは、住所を「全角（ＩＭＥオン）」で入力します。しかし、郵便番号や番地の数字を「半角」でしか受け付けないバリデーションにすると、ブラウザの自動補完が「全角」だった場合にエラーになり、ユーザーは強いストレスを感じます。
*   **対策:** **「フロントエンドまたはサーバーサイドで、全角数字を半角に自動変換する」**。ユーザーに再入力を求めないのが `web.dev` 流の改善です。

### 結論としての提案
1.  **`autocomplete` 属性を完全に付与する**（ブラウザへの最も重要なシグナル）。
2.  **`inputmode="numeric"` を郵便番号に使う**（モバイルの摩擦軽減）。
3.  **「市区町村」と「番地」は一つの入力欄（`address-line1`）にする**（補完エラーの防止）。
4.  **全角入力されてもシステム側で許容・変換する**（ユーザーに修正させない）。


## オートコンプリート
https://web.dev/learn/forms/address?hl=ja
https://form.run/home/blog/efo/overview/mean
https://tech.asoview.co.jp/entry/2022/12/02/103158
https://zenn.dev/yumemi_inc/articles/c1c83cb4cdcaa8
    User-Agent を見てフォームを出し分ける？
