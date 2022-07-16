# Docker-Access-Router

HTTPリクエスト等をリバースプロキシを使い複数のコンテナへ分岐するコンテナのテンプレートです

これを使うことで複数のコンテナのWebアプリケーションを動作させることができます

## Use

### 1. リポジトリのクローン

```
git clone [REPO]
cd [REPO]
```

### 2. ネットワークの作成と接続

下記コマンドで共有ネットワークの作成

```sh
docker network create reverse-proxy-network
```

下記のように`networks`を追加して`reverse-proxy-network`へ接続します

```yml
version: "3"

services:
  web_app:
# ~~~~~~~~~~~~~~~~~~~~~~ #    
    networks: # Add
      - reverse-proxy-network
      - default

networks:
  reverse-proxy-network: #Add
    external: true
```

### 3. リバースプロキシの設定

設定のサンプルをコピー

```sh
cp config/sample.conf config/default.conf
```

下記コマンドでプロキシ先コンテナのアドレスを確認

```sh
docker network inspect reverse-proxy-network
```

`Containers`の`name`がアドレスとなります

```json
"Internal": false,
"Attachable": false,
"Ingress": false,
"ConfigFrom": {
  "Network": ""
},
"ConfigOnly": false,
"Containers": {},
"Options": {},
"Labels": {}
```

`default.conf`に下記のように記述します

```
server {
    listen 80;
    server_name  example.net;
    location / {
        proxy_pass http://[CONTAINER NAME]:[PORT]/;
    }
}
```

**Nginxの設定なのでSSL等の設定はそれに従ってください**

### 4. コンテナの起動

```sh
docker compose up -d
```