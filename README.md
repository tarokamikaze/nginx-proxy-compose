![License MIT](https://img.shields.io/badge/license-MIT-blue.svg)

nginx-proxy ひとつで、多数のコンテナをハンドルするための設定です。  
ローカル環境や、テストサーバーなどにどうぞ。

## Config
[nginx-proxy 本家](https://github.com/jwilder/nginx-proxy) をご覧ください。

## SetUp

```
$ git clone git@github.com:tarokamikaze/nginx-proxy-compose.git
$ cd nginx-proxy-compose
$ docker-compose up -d
```

## 新しいコンテナ/サービスの追加

新しいサービスの docker-compose.yml に下記を追記してください。

```
services:
  web:
    image: php:7-apache
    environment:
      VIRTUAL_HOST: test.local # host名
      HTTPS_METHOD: noredirect # http/https 両方使いたい場合。ssl専用、またはhttpならこの項目は不要
    volumes:
      - ./webroot:/var/www/html
networks: #nginx-proxy と疎通するためのおまじない
  default:
    external:
      name: nginxproxycompose_default
```

SSLを使いたい場合、path/to/nginx-proxy-compose/certs/ 以下に、${VIRTUAL_HOST}.crt, ${VIRTUAL_HOST}.key を配置します。  

その後、${VIRTUAL_HOST} の向き先をホストに向け、新サービスのコンテナをを立ち上げたら完了です。
