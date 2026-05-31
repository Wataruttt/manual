# Docker

## インストール・初期設定

### Docker Desktop のインストール（macOS）

```bash
brew install --cask docker
```

またはhttps://www.docker.com/products/docker-desktop/ からDMGをダウンロード。

### インストール確認

```bash
docker --version
docker compose version
```

### よく使う設定（Docker Desktop）

- **Resources** — CPUコア数・メモリ上限を環境に合わせて調整
- **General** — "Start Docker Desktop when you log in" でログイン時に自動起動

---

## 基本コマンド

### イメージ操作

```bash
# 取得・確認
docker pull nginx:latest          # イメージを取得
docker images                     # ローカルのイメージ一覧
docker image ls

# 削除
docker rmi nginx:latest           # イメージを削除
docker image prune                # 未使用イメージを一括削除
docker image prune -a             # タグなし・未使用すべて削除

# ビルド
docker build -t myapp:latest .           # カレントディレクトリの Dockerfile でビルド
docker build -t myapp:latest -f path/to/Dockerfile .
```

### コンテナ操作

```bash
# 起動
docker run nginx                            # フォアグラウンドで起動
docker run -d nginx                         # バックグラウンドで起動
docker run -d -p 8080:80 nginx              # ポートマッピング（ホスト:コンテナ）
docker run -d --name my-nginx nginx         # 名前をつけて起動
docker run -it ubuntu bash                  # インタラクティブに起動

# 確認
docker ps                                   # 起動中のコンテナ
docker ps -a                                # 停止中も含む全コンテナ

# 停止・削除
docker stop my-nginx                        # コンテナを停止
docker start my-nginx                       # コンテナを再起動
docker restart my-nginx
docker rm my-nginx                          # コンテナを削除（停止後）
docker rm -f my-nginx                       # 強制削除（起動中でも）
docker container prune                      # 停止中のコンテナを一括削除

# コンテナ内で操作
docker exec -it my-nginx bash              # コンテナ内でbashを実行
docker exec my-nginx ls /etc/nginx         # コンテナ内でコマンドを実行
docker logs my-nginx                        # ログを確認
docker logs -f my-nginx                     # ログをリアルタイムで追跡
```

### 全体のクリーンアップ

```bash
docker system prune           # 未使用のコンテナ・ネットワーク・イメージを削除
docker system prune -a        # 未使用イメージもすべて削除
docker system df              # ディスク使用量を確認
```

---

## Docker Compose

複数コンテナをまとめて管理するためのツール。

### compose.yml の基本構成

```yaml
services:
  app:
    build: .
    ports:
      - "3000:3000"
    volumes:
      - .:/app
      - /app/node_modules
    environment:
      - NODE_ENV=development
    depends_on:
      - db

  db:
    image: postgres:16
    ports:
      - "5432:5432"
    environment:
      POSTGRES_DB: mydb
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
```

### よく使うコマンド

```bash
# 起動・停止
docker compose up              # フォアグラウンドで起動
docker compose up -d           # バックグラウンドで起動
docker compose up --build      # イメージを再ビルドして起動
docker compose down            # コンテナを停止・削除
docker compose down -v         # ボリュームも削除

# 確認
docker compose ps              # コンテナ一覧
docker compose logs            # 全サービスのログ
docker compose logs -f app     # 特定サービスをリアルタイムで追跡

# コンテナ内で操作
docker compose exec app bash   # appサービスのコンテナに入る
docker compose run app sh      # 新しいコンテナを起動してコマンドを実行

# 再起動
docker compose restart app     # 特定サービスを再起動

# ビルドのみ
docker compose build
docker compose build app       # 特定サービスのみ
```

---

## よく使うパターン

### 開発環境の立ち上げ

```bash
# 初回・イメージ更新時
docker compose up --build -d

# 通常の起動
docker compose up -d

# ログ確認しながら起動
docker compose up
```

### アプリのログをリアルタイム確認

```bash
docker compose logs -f app
docker logs -f <container_name>

# 最新50行から追跡
docker compose logs --tail=50 -f app
```

### コンテナ内でコマンドを実行

```bash
# DB のマイグレーション実行など
docker compose exec app npm run migrate
docker compose exec app rails db:migrate
docker compose exec db psql -U user -d mydb
```

### 特定サービスだけ再ビルド

```bash
docker compose up -d --build app
```

### 環境変数を .env で管理

```bash
# .env ファイルに記述
POSTGRES_PASSWORD=secret
NODE_ENV=development

# compose.yml から参照
environment:
  - POSTGRES_PASSWORD=${POSTGRES_PASSWORD}
```

---

## ボリューム・ネットワーク

### ボリューム（データ永続化）

```bash
# 確認・削除
docker volume ls
docker volume rm my_volume
docker volume prune          # 未使用のボリュームを一括削除
```

**compose.yml でのボリューム設定：**

```yaml
volumes:
  # 名前付きボリューム（データ永続化）
  - postgres_data:/var/lib/postgresql/data

  # バインドマウント（ホストのディレクトリをマウント）
  - ./src:/app/src

  # 匿名ボリューム（node_modules 等をホストと分離）
  - /app/node_modules
```

### ネットワーク

```bash
# 確認
docker network ls
docker network inspect bridge

# compose.yml 内のサービス間通信
# サービス名をホスト名として使える
# 例: app から db に接続するとき → ホスト名は "db"
```

**compose.yml でのネットワーク設定：**

```yaml
services:
  app:
    networks:
      - frontend
      - backend
  db:
    networks:
      - backend

networks:
  frontend:
  backend:
```

---

## トラブル対応

### コンテナが起動しない

```bash
# ログを確認
docker compose logs app
docker logs <container_id>

# 終了コードを確認
docker ps -a    # STATUS 列を見る（Exited (1) など）

# インタラクティブに起動してデバッグ
docker compose run --rm app sh
```

### ポートが競合している

```bash
# ポートを使用しているプロセスを確認
lsof -i :3000

# 別のポートにマッピングし直す
ports:
  - "3001:3000"
```

### イメージが古い・更新が反映されない

```bash
docker compose down
docker compose build --no-cache    # キャッシュを使わず再ビルド
docker compose up -d
```

### ボリュームのデータをリセットしたい

```bash
docker compose down -v    # コンテナ停止＋ボリューム削除
docker compose up -d      # 再起動（DBが初期化された状態で起動）
```

### コンテナ内からホストに接続したい

```bash
# Mac / Windows の場合、ホストIPは以下で参照できる
host.docker.internal
```

### ディスク容量が不足している

```bash
docker system df              # 使用量を確認
docker system prune -a -f     # 未使用リソースをすべて削除
```
