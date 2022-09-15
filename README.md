# Docker-Access-Router

HTTPリクエスト等をリバースプロキシを使い複数のコンテナへ分岐するコンテナのテンプレートです

これを使うことで複数のコンテナのWebアプリケーションを動作させることができます

## Use

### 1. リポジトリのクローン

```
git clone https://github.com/Ablaze-MIRAI/Docker-Access-Router.git
cd Docker-Access-Router
```

### 2. リバースプロキシの設定

設定のサンプルをコピー

```sh
cp config/default.conf.sample config/default.conf
```

**Nginxの設定なのでSSL等の設定はそれに従ってください**

### 4. コンテナの起動

```sh
docker compose up -d
```
