# Obsidian

Markdown ベースのローカルファースト・ナレッジベースツール。

---

## インストール・初期設定

### インストール

```bash
brew install --cask obsidian
```

または https://obsidian.md/ からダウンロード。

### Vault の作成

起動後「Create new vault」→ 保存先ディレクトリを選択。

- Vault = ノートを格納するルートフォルダ
- Vault 内のすべての `.md` ファイルが管理対象になる
- 複数の Vault を切り替えて使うことも可能

### 基本設定（Settings）

`Cmd + ,` で設定を開く。

| 設定 | 推奨値 | 場所 |
|------|--------|------|
| デフォルト編集モード | Live Preview | Editor |
| スペルチェック | オフ（日本語使用時） | Editor |
| 新規ノートの保存先 | 特定フォルダを指定 | Files & Links |
| 内部リンクの形式 | Shortest path | Files & Links |
| 添付ファイルの保存先 | `_attachments` フォルダ | Files & Links |

---

## 基本操作

### ノートの作成・管理

```
新規ノート作成          Cmd + N
ファイルエクスプローラ  Cmd + Shift + E
コマンドパレット        Cmd + P
設定を開く              Cmd + ,
```

### 内部リンク

```markdown
[[ノート名]]                    # 別ノートへのリンク
[[ノート名|表示テキスト]]       # テキストを変えてリンク
[[ノート名#見出し]]             # 特定の見出しへリンク
![[ノート名]]                   # ノートを埋め込み表示
![[画像ファイル名.png]]         # 画像を埋め込み
```

### タグ

```markdown
#tag               # インラインタグ
#parent/child      # 階層タグ
```

フロントマターでも指定可能：

```yaml
---
tags:
  - dev
  - docker
---
```

### 検索

```
クイック検索（ファイル名）    Cmd + O
全文検索                      Cmd + Shift + F
バックリンク確認              サイドパネルのBacklinksから
```

---

## ショートカット

### 編集

| 操作 | ショートカット |
|------|-------------|
| 太字 | `Cmd + B` |
| 斜体 | `Cmd + I` |
| インラインコード | `Cmd + E` |
| 見出し切り替え | `Cmd + 1〜6` |
| リスト追加 | `Cmd + Enter` |
| チェックボックス | `Cmd + L` |
| 取り消し線 | `Cmd + Shift + S` |
| リンク挿入 | `Cmd + K` |

### 表示・移動

| 操作 | ショートカット |
|------|-------------|
| グラフビュー | `Cmd + Shift + G` |
| バックリンク | `Cmd + Alt + B` |
| 編集 / 読み取りモード切替 | `Cmd + E` |
| 左サイドバー切替 | `Cmd + Shift + L` |  
| 右サイドバー切替 | `Cmd + Shift + R` |
| タブを閉じる | `Cmd + W` |
| 前のタブへ | `Cmd + Shift + [` |
| 次のタブへ | `Cmd + Shift + ]` |

---

## おすすめプラグイン

### コアプラグイン（設定で有効化）

| プラグイン | 用途 |
|-----------|------|
| **Backlinks** | 現在のノートへのリンク一覧 |
| **Outgoing links** | 現在のノートからのリンク一覧 |
| **Templates** | テンプレートからノートを作成 |
| **Daily notes** | 日付ごとのノートを自動作成 |
| **Tag pane** | タグ一覧をサイドパネルで表示 |
| **Word count** | 文字数・単語数を表示 |
| **Slash commands** | `/` でコマンドを呼び出し |

### コミュニティプラグイン（おすすめ）

| プラグイン | 用途 |
|-----------|------|
| **Dataview** | ノートをデータベースのようにクエリ・一覧表示 |
| **Templater** | 高機能テンプレート（日付・変数・スクリプト対応） |
| **Calendar** | カレンダービューで Daily notes を管理 |
| **Git** | Vault を Git で自動同期 |
| **Linter** | Markdown の自動整形 |
| **Excalidraw** | ノート内に手書き図を埋め込み |
| **Advanced Tables** | テーブルの入力補助・整形 |

コミュニティプラグインの有効化：Settings → Community plugins → Turn on community plugins → Browse

---

## Vault 構成・運用ルール

### フォルダ構成例

```
Vault/
├── _inbox/          # 未整理のメモを一時的に置く場所
├── _templates/      # テンプレートファイル
├── _attachments/    # 画像・PDFなどの添付ファイル
├── daily/           # Daily notes
├── dev/             # 開発関連（Git・Docker・言語メモ等）
├── ai/              # AI関連のメモ
└── projects/        # プロジェクトごとのノート
```

### 命名規則

```
# 一般ノート
tool-git-cheatsheet.md
dev-docker-compose.md

# Daily notes（Templaterで自動生成）
2026-06-01.md
```

### テンプレート例（_templates/daily.md）

```markdown
---
date: {{date:YYYY-MM-DD}}
tags:
  - daily
---

# {{date:YYYY-MM-DD}}

## 今日やること
- [ ] 

## メモ

## 明日に持ち越し
- [ ] 
```

---

## 他ツールとの連携

### Git 同期（obsidian-git プラグイン）

コミュニティプラグイン「Git」をインストールして設定。

**主な設定：**

| 設定 | 推奨値 |
|------|--------|
| Auto pull interval | 10分 |
| Auto commit interval | 20分 |
| Auto push | オン |
| Commit message | `vault backup: {{date}}` |

**手動操作：**
```
Cmd + P → Git: Create backup   # commit & push
Cmd + P → Git: Pull            # pull
```

### VS Code 連携

Vault フォルダを VS Code で開くと `.md` ファイルを直接編集できる。  
大量のファイルを一括編集・検索する場合に便利。

```bash
code ~/path/to/vault
```

### Claudeとの連携

ノートのMarkdownをそのままClaudeに貼り付けて質問・加工できる。  
RAW URLをClaudeに渡すとファイル内容を直接参照させることも可能。

```
https://raw.githubusercontent.com/username/vault/main/dev/docker.md
```

※ Vault を GitHub で管理している場合のみ利用可能。
