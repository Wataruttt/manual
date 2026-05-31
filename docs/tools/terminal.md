# Terminal

macOS の zsh 環境を前提としています。

---

## zsh 設定

### 設定ファイルの場所

| ファイル | 用途 |
|---------|------|
| `~/.zshrc` | インタラクティブシェルの設定（エイリアス・関数・プロンプト等） |
| `~/.zprofile` | ログイン時のみ読み込まれる設定（PATH等） |

設定を反映：

```bash
source ~/.zshrc
```

### よく使う設定項目（.zshrc）

```zsh
# 補完を有効化
autoload -Uz compinit && compinit

# 大文字小文字を区別しない補完
zstyle ':completion:*' matcher-list 'm:{a-z}={A-Z}'

# コマンド履歴
HISTSIZE=10000
SAVEHIST=10000
HISTFILE=~/.zsh_history
setopt HIST_IGNORE_DUPS      # 重複を記録しない
setopt SHARE_HISTORY         # 複数ターミナル間で履歴を共有

# ディレクトリ移動を cd なしで行う
setopt AUTO_CD
```

---

## 基本コマンド

### ファイル・ディレクトリ操作

```bash
# 確認
ls -la                    # 詳細一覧（隠しファイル含む）
ls -lh                    # サイズを人間が読みやすい形式で
pwd                       # 現在のディレクトリを表示

# 移動・作成
cd ~/Documents
mkdir -p path/to/dir      # 中間ディレクトリも含めて作成
touch file.txt            # 空ファイル作成

# コピー・移動・削除
cp file.txt copy.txt
cp -r dir/ newdir/        # ディレクトリをコピー
mv file.txt ~/Desktop/    # 移動
mv old.txt new.txt        # リネーム
rm file.txt
rm -rf dir/               # ディレクトリを中身ごと削除（要注意）
```

### テキスト操作

```bash
cat file.txt              # ファイル内容を表示
less file.txt             # ページャで表示（q で終了）
head -n 20 file.txt       # 先頭20行
tail -n 20 file.txt       # 末尾20行
tail -f log.txt           # リアルタイムで末尾を追跡

grep "keyword" file.txt          # ファイル内を検索
grep -r "keyword" ./src/         # ディレクトリ内を再帰検索
grep -r "keyword" . --include="*.ts"   # 拡張子を絞って検索
```

### 検索

```bash
# ファイルを検索
find . -name "*.md"               # 名前で検索
find . -type f -newer file.txt    # 指定ファイルより新しいファイル
find . -name "node_modules" -prune -o -name "*.ts" -print  # node_modules除外

# コマンド履歴を検索
history | grep git
```

### パーミッション

```bash
ls -l           # パーミッションを確認（rwxr-xr-x の形式）
chmod 755 script.sh     # 実行権限を付与
chmod +x script.sh      # 実行権限を追加
chown user:group file   # オーナーを変更
```

---

## エイリアス

`~/.zshrc` に追記する。

```zsh
# ナビゲーション
alias ..='cd ..'
alias ...='cd ../..'
alias ll='ls -la'
alias la='ls -A'

# Git
alias g='git'
alias gs='git status'
alias ga='git add'
alias gc='git commit'
alias gp='git push'
alias gl='git log --oneline --graph'
alias gd='git diff'

# 安全な削除（確認あり）
alias rm='rm -i'
alias cp='cp -i'
alias mv='mv -i'

# よく使うディレクトリ
alias dev='cd ~/Development'
```

設定反映：

```bash
source ~/.zshrc
```

---

## 環境変数・PATH

### 環境変数の設定

```zsh
# .zshrc または .zprofile に追記
export MY_VAR="value"
export API_BASE_URL="https://example.com"

# 確認
echo $MY_VAR
env | grep MY_VAR
```

### PATH の追加

```zsh
# 既存の PATH に追記（.zprofile 推奨）
export PATH="$HOME/.local/bin:$PATH"
export PATH="/opt/homebrew/bin:$PATH"   # Apple Silicon の Homebrew

# 現在の PATH を確認
echo $PATH | tr ':' '\n'   # 1行ずつ表示
```

### よく使う環境変数

```zsh
export EDITOR="code --wait"       # デフォルトエディタを VS Code に
export LANG="ja_JP.UTF-8"         # 日本語ロケール
export NODE_ENV="development"
```

---

## プロセス管理

### プロセスの確認・終了

```bash
# 確認
ps aux                    # 全プロセスを表示
ps aux | grep node        # 名前でフィルタ
top                       # リアルタイム表示（q で終了）

# ポート使用状況の確認
lsof -i :3000             # ポート3000を使っているプロセス

# 終了
kill <PID>                # 通常終了
kill -9 <PID>             # 強制終了
pkill node                # 名前で一括終了
```

### バックグラウンド実行

```bash
command &                 # バックグラウンドで実行
jobs                      # バックグラウンドジョブ一覧
fg %1                     # ジョブ番号1をフォアグラウンドに戻す
bg %1                     # 停止中のジョブをバックグラウンドで再開
Ctrl + Z                  # 実行中のプロセスを一時停止
Ctrl + C                  # 実行中のプロセスを終了
```

---

## Homebrew

macOS のパッケージマネージャー。

### インストール

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### よく使うコマンド

```bash
# パッケージ操作
brew install <package>       # インストール
brew uninstall <package>     # アンインストール
brew upgrade <package>       # アップデート
brew upgrade                 # 全パッケージをアップデート

# 確認
brew list                    # インストール済み一覧
brew info <package>          # パッケージ情報
brew search <keyword>        # 検索

# メンテナンス
brew update                  # Homebrew 自体を更新
brew cleanup                 # 古いバージョンを削除
brew doctor                  # 問題を診断
```

### Cask（GUIアプリ）

```bash
brew install --cask visual-studio-code
brew install --cask docker
brew list --cask              # インストール済みCask一覧
```

---

## 便利なツール

### fzf（ファジーファインダー）

インクリメンタルな絞り込み検索。

```bash
brew install fzf
$(brew --prefix)/opt/fzf/install   # キーバインドを設定

# 使い方
Ctrl + R    # 履歴をファジー検索
Ctrl + T    # カレントディレクトリのファイルをファジー選択
cd **Tab    # ディレクトリをファジー選択
```

### zoxide（スマートなcd）

過去に移動したディレクトリを学習して素早く移動できる。

```bash
brew install zoxide

# .zshrc に追記
eval "$(zoxide init zsh)"

# 使い方
z manual      # "manual" を含む最近のディレクトリに移動
zi            # ファジー検索でディレクトリを選択（fzf必要）
```

### bat（catの強化版）

シンタックスハイライト・行番号付きでファイルを表示。

```bash
brew install bat

bat file.ts          # シンタックスハイライト表示
bat --plain file.ts  # 行番号なし
```

### eza（lsの強化版）

カラー・アイコン付きのファイル一覧表示。

```bash
brew install eza

eza -la              # 詳細一覧
eza --tree           # ツリー表示
eza --tree --level=2 # 深さ2まで

# エイリアスとして設定
alias ls='eza'
alias ll='eza -la'
```
