# Claude

## Claude Code とは

Anthropic が提供するCLIツール。ターミナルから直接Claudeと対話しながらコーディング作業ができる。

---

## インストール・起動

```bash
# インストール
npm install -g @anthropic-ai/claude-code

# 起動
claude

# 特定ディレクトリで起動
claude /path/to/project
```

---

## スラッシュコマンド

| コマンド | 説明 |
|---------|------|
| `/help` | ヘルプ表示 |
| `/status` | 現在の設定・モデル確認 |
| `/config` | 設定ダイアログを開く |
| `/model` | 使用モデルの切り替え |
| `/clear` | 会話履歴をリセット |
| `/compact` | 会話を圧縮してコンテキストを節約 |
| `/review` | 現在のdiffをコードレビュー |
| `/cost` | 現在のセッションのトークン使用量確認 |
| `/exit` | Claude Code を終了 |

### ターミナルコマンドの直接実行

プロンプトに `! <コマンド>` と入力すると、シェルコマンドを直接実行してその出力を会話に取り込める。

```
! git status
! ls -la
```

---

## モデル

| モデル | ID | 特徴 |
|-------|----|------|
| **Opus 4** | `claude-opus-4-8` | 最高性能。複雑な推論・設計に |
| **Sonnet 4** | `claude-sonnet-4-6` | バランス型。日常的な開発作業に（デフォルト） |
| **Haiku 4** | `claude-haiku-4-5-20251001` | 高速・軽量。簡単な質問や確認に |

モデルは `/model` コマンドまたは `/config` から切り替え可能。

---

## 効果的な使い方

### 具体的に伝える
曖昧な指示より、何をどうしたいかを明示する。

```
# 曖昧
「認証を直して」

# 具体的
「login() 関数でトークンの有効期限チェックが抜けている。
期限切れの場合は 401 を返すように修正して」
```

### コンテキストを渡す
- ファイルパスを明示する（例：`src/auth.ts の login 関数を見て`）
- エラーメッセージはそのまま貼り付ける
- 「なぜそうしたいか」の背景を添えると精度が上がる

### 段階的に進める
大きなタスクは一度に頼まず、ステップごとに確認しながら進める。

### CLAUDE.md でプロジェクトコンテキストを共有
プロジェクトルートに `CLAUDE.md` を置くと、Claude が自動で読み込む。
プロジェクトの概要・規約・注意事項を書いておくと毎回説明しなくて済む。

---

## 設定ファイル

設定は JSON ファイルで管理される。

| ファイル | 対象 |
|---------|------|
| `~/.claude/settings.json` | グローバル設定（全プロジェクト共通） |
| `.claude/settings.json` | プロジェクト固有の設定 |

主な設定項目：

```json
{
  "model": "claude-sonnet-4-6",
  "theme": "dark",
  "permissions": {
    "allow": ["Bash(git *)", "Bash(npm *)"]
  }
}
```

`/config` コマンドからGUIで変更することもできる。

---

## このマニュアルをClaudeに参照させる方法

### 会話中にURLを渡す

```
以下のマニュアルを参照してください：
https://Wataruttt.github.io/manual/
```

### CLAUDE.md に記載する

プロジェクトの `CLAUDE.md` にマニュアルのURLを書いておくと、
Claude が自動で参照できる。

```markdown
## 参考資料
- 個人マニュアル: https://Wataruttt.github.io/manual/
```

### MDファイルを直接渡す

GitHubのRAWファイルURLを渡すと、ページの内容をそのまま読み込める。

```
https://raw.githubusercontent.com/Wataruttt/manual/main/docs/ai/claude.md
```
