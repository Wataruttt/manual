# VS Code

## 基本設定

### 設定ファイルの場所

| ファイル | 場所 |
|---------|------|
| ユーザー設定 | `~/.config/Code/User/settings.json`（Mac: `~/Library/Application Support/Code/User/settings.json`） |
| プロジェクト設定 | `.vscode/settings.json` |
| キーバインド | `~/Library/Application Support/Code/User/keybindings.json` |

設定UIを開く：`Cmd + ,`  
settings.json を直接開く：`Cmd + Shift + P` → `Open User Settings (JSON)`

### よく使う設定項目

```json
{
  "editor.fontSize": 14,
  "editor.tabSize": 2,
  "editor.insertSpaces": true,
  "editor.formatOnSave": true,
  "editor.wordWrap": "on",
  "editor.minimap.enabled": false,
  "editor.renderWhitespace": "boundary",
  "files.autoSave": "onFocusChange",
  "terminal.integrated.fontSize": 13,
  "workbench.colorTheme": "Default Dark Modern",
  "workbench.startupEditor": "none"
}
```

---

## 拡張機能

### インストール方法

```bash
# CLIからインストール
code --install-extension <extension-id>

# 一覧確認
code --list-extensions
```

### おすすめ拡張機能

| 拡張機能 | ID | 用途 |
|---------|-----|------|
| **Claude Code** | `anthropic.claude-code` | AI コーディング支援 |
| **GitLens** | `eamodio.gitlens` | Git 履歴・blame 強化 |
| **Prettier** | `esbenp.prettier-vscode` | コードフォーマッター |
| **ESLint** | `dbaeumer.vscode-eslint` | JavaScript/TypeScript lint |
| **Docker** | `ms-azuretools.vscode-docker` | Docker 管理UI |
| **Remote - Containers** | `ms-vscode-remote.remote-containers` | コンテナ内開発 |
| **indent-rainbow** | `oderwat.indent-rainbow` | インデントの視覚化 |
| **Error Lens** | `usernamehw.errorlens` | エラーをインラインで表示 |
| **Path Intellisense** | `christian-kohler.path-intellisense` | ファイルパス補完 |
| **Markdown Preview Enhanced** | `shd101wyy.markdown-preview-enhanced` | Markdownプレビュー強化 |

---

## ショートカット（Mac）

### 基本操作

| 操作 | ショートカット |
|------|-------------|
| コマンドパレット | `Cmd + Shift + P` |
| ファイルを開く | `Cmd + P` |
| 設定を開く | `Cmd + ,` |
| サイドバー表示切替 | `Cmd + B` |
| ターミナル表示切替 | `Cmd + J` |
| 新しいターミナル | `` Ctrl + ` `` |

### 編集

| 操作 | ショートカット |
|------|-------------|
| 行を上下に移動 | `Alt + ↑ / ↓` |
| 行をコピー（選択なし） | `Cmd + C` |
| 行を削除 | `Cmd + Shift + K` |
| 行をコメントアウト | `Cmd + /` |
| ブロックコメント | `Alt + Shift + A` |
| 複数カーソル追加 | `Alt + クリック` |
| 同じ単語を全選択 | `Cmd + Shift + L` |
| 次の同じ単語を選択 | `Cmd + D` |
| インデント追加/削除 | `Cmd + ]` / `Cmd + [` |
| フォーマット | `Alt + Shift + F` |

### 検索・移動

| 操作 | ショートカット |
|------|-------------|
| ファイル内検索 | `Cmd + F` |
| ファイル内置換 | `Cmd + H` |
| プロジェクト全体検索 | `Cmd + Shift + F` |
| 行番号へジャンプ | `Ctrl + G` |
| 定義へジャンプ | `F12` |
| 参照箇所を全表示 | `Shift + F12` |
| 前後のタブへ移動 | `Cmd + Shift + [ / ]` |
| エクスプローラーへフォーカス | `Cmd + Shift + E` |

### 画面分割

| 操作 | ショートカット |
|------|-------------|
| エディタを分割 | `Cmd + \` |
| 分割エディタ間を移動 | `Cmd + 1 / 2 / 3` |

---

## ターミナル統合

### 基本操作

```
新しいターミナルを開く：Ctrl + `  または  Cmd + J
ターミナルを分割：Cmd + Shift + 5
ターミナルをパネルで表示：Cmd + J
```

### シェルの変更

`Cmd + Shift + P` → `Terminal: Select Default Profile` → zsh / bash を選択

### ターミナルの設定例

```json
{
  "terminal.integrated.defaultProfile.osx": "zsh",
  "terminal.integrated.fontSize": 13,
  "terminal.integrated.scrollback": 5000
}
```

---

## Git 統合

### ソース管理パネル

`Cmd + Shift + G` でソース管理パネルを開く。

- ファイルの差分をクリックで確認
- `+` でステージング
- メッセージ入力 → `Cmd + Enter` でコミット
- `...` メニューからpush/pull

### GitLens の主な機能

- 各行にコミット情報をインライン表示（blame）
- ファイルの変更履歴をタイムライン表示
- ブランチ・タグの管理UI

### エディタ上での差分確認

変更したファイルのエクスプローラーアイコンをクリック → 左右で差分表示

---

## Claude Code 連携

### VS Code 拡張のインストール

拡張機能パネル（`Cmd + Shift + X`）で `Claude Code` を検索してインストール。

または：

```bash
code --install-extension anthropic.claude-code
```

### 使い方

| 操作 | 方法 |
|------|------|
| Claude Code を開く | `Cmd + Shift + P` → `Claude: Open` |
| 選択範囲について質問 | コードを選択 → 右クリック → `Ask Claude` |
| インライン編集 | コード内で `Cmd + K` |

### ターミナルから起動

VS Code 内のターミナルで `claude` コマンドを実行すると、現在開いているプロジェクトのコンテキストで Claude Code が起動する。
