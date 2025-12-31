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

## 環境構築（VS Code Extension）

```powershell

code --install-extension ms-python.python
code --install-extension ms-python.debugpy
code --install-extension charliermarsh.ruff
code --install-extension astral-sh.ty
code --install-extension usernamehw.errorlens
code --install-extension oderwat.indent-rainbow
code --install-extension njpwerner.autodocstring

```

