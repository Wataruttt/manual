# Git

## 初期設定

```bash
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
git config --global core.editor "code --wait"   # VS Code をエディタに
git config --global init.defaultBranch main      # デフォルトブランチを main に

# 設定確認
git config --list
```

---

## GitHub 認証（SSH）

### SSHキーの作成

```bash
# キー生成（ed25519推奨）
ssh-keygen -t ed25519 -C "your@email.com"
# 保存先はデフォルト（~/.ssh/id_ed25519）でEnter
# パスフレーズは任意

# 公開鍵を表示（GitHubに登録する内容）
cat ~/.ssh/id_ed25519.pub
```

### GitHubへの登録

1. GitHub → Settings → SSH and GPG keys → New SSH key
2. 上記の公開鍵をペースト → Add SSH key

### 接続確認

```bash
ssh -T git@github.com
# Hi username! You've successfully authenticated... と表示されれば成功
```

### SSHエージェントへの登録（毎回パスフレーズを入力しないために）

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

---

## 基本コマンド

### リポジトリの作成・取得

```bash
# 新規作成
git init
git init my-project    # ディレクトリも同時作成

# クローン
git clone git@github.com:username/repo.git
git clone git@github.com:username/repo.git my-dir   # ディレクトリ名を指定
```

### 変更の記録

```bash
# 状態確認
git status

# ステージング
git add .                  # 全ファイル
git add src/file.ts        # 特定ファイル

# コミット
git commit -m "メッセージ"
git commit                 # エディタでメッセージ作成

# リモートへ送信
git push
git push origin main       # ブランチを明示
git push -u origin main    # 初回：追跡設定も同時に行う
```

### 変更の取得

```bash
git pull                   # fetch + merge
git fetch                  # リモートの情報だけ取得（ローカルには反映しない）
```

---

## ブランチ操作

```bash
# ブランチ一覧
git branch           # ローカル
git branch -a        # リモート含む全て

# ブランチ作成・切り替え
git switch -c feature/my-feature    # 作成して切り替え（推奨）
git switch main                     # 切り替えのみ

# ブランチ削除
git branch -d feature/my-feature    # マージ済みのみ削除
git branch -D feature/my-feature    # 強制削除

# マージ
git switch main
git merge feature/my-feature

# リベース
git switch feature/my-feature
git rebase main
```

---

## よく使うコマンド

### 差分確認

```bash
git diff                   # ステージング前の差分
git diff --staged          # ステージング済みの差分
git diff main..feature     # ブランチ間の差分
```

### 履歴確認

```bash
git log
git log --oneline          # 1行表示
git log --oneline --graph  # ブランチの分岐もグラフ表示
git log -n 5               # 直近5件
```

### スタッシュ（作業を一時退避）

```bash
git stash                  # 退避
git stash list             # 一覧
git stash pop              # 最新を復元して削除
git stash apply            # 最新を復元（削除しない）
git stash drop             # 最新を削除
```

---

## やり直し・取り消し

### コミットの修正

```bash
# 直前のコミットメッセージを修正（未pushの場合のみ）
git commit --amend -m "修正したメッセージ"

# 直前のコミットにファイルを追加（未pushの場合のみ）
git add 忘れたファイル
git commit --amend --no-edit
```

### ステージングの取り消し

```bash
git restore --staged file.ts    # ステージングを取り消す（ファイルは変更済みのまま）
```

### ファイルの変更を取り消す

```bash
git restore file.ts    # 最後のコミット状態に戻す（変更が消える）
```

### コミットの取り消し

```bash
# 打ち消しコミットを作る（履歴を残す・安全）
git revert HEAD

# コミットを取り消してステージング済みに戻す（未pushの場合のみ）
git reset --soft HEAD~1

# コミットを取り消してワーキングツリーも戻す（変更が消える・要注意）
git reset --hard HEAD~1
```

---

## トラブル対応

### コンフリクトの解消

```bash
git merge feature/my-feature
# CONFLICT と表示されたら

git status    # コンフリクトしているファイルを確認
# ファイルを開いて <<<< ==== >>>> の箇所を手動で編集
git add 修正したファイル
git commit
```

### 誤ってmainにコミットしてしまった

```bash
# 新しいブランチに移動してから
git switch -c feature/rescue

# mainを1つ前に戻す（未pushの場合のみ）
git switch main
git reset --soft HEAD~1
```

### リモートの最新を強制的に取得したい

```bash
git fetch origin
git reset --hard origin/main
```

### .gitignore を後から追加したのに無視されない

```bash
git rm -r --cached .    # キャッシュをクリア
git add .
git commit -m "Apply .gitignore"
```
