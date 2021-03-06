# Docker Exment

## このイメージについて
ExmentのDockerイメージです。


## イメージの使用方法

- このリポジトリをダウンロードし、zipを解凍します。

- 使用するフォルダを選択します。buildフォルダ内の以下のフォルダから、構築したい環境により、使用するフォルダを選択します。
    - **php72_mysql** : PHP7.2, MySQLもしくはMariaDB
    - **php72_sqldrv** : PHP7.2, SQL Server
    - **php73_mysql** : PHP7.3, MySQLもしくはMariaDB
    - **php73_sqldrv** : PHP7.3, SQL Server
    - **php74_mysql** : PHP7.4, MySQLもしくはMariaDB
    - **php74_sqldrv** : PHP7.4, SQL Server

- コンソールで、上記のフォルダを、カレントディレクトリとして遷移します。

- コンソールで、以下のコマンドを実施し、環境を起動します。

```
docker-compose up
```


## 起動設定

docker-composeで起動する環境を選択します。起動する環境ごとに、「docker-compose.*.yml」ファイルが分かれておりますので、コマンド実行時に"-f"オプションで、ファイルを指定してください。


| 設定ファイル | 環境 |
| ---- | ---- |
| docker-compose.yml | Webサーバー(ngnix、PHP) |
| docker-compose.mysql.yml | MySQL |
| docker-compose.mariadb.yml | MariaDB |
| docker-compose.sqlsrv.yml | SQL Server |
| docker-compose.phpmyadmin.yml | PhpMyAdmin |
| docker-compose.redis.yml | Redis |
| docker-compose.balancer.yml | ロードバランサー |
| docker-compose.https-portal.yml | https |


#### Webサーバーのみ

起動時、「-f docker-compose.yml」のみ追加します。(もしくは、"-f"オプション無しで起動します)

```
docker-compose -f docker-compose.yml up
```


#### Webサーバー・MySQL

起動時、「-f docker-compose.mysql.yml」を追加します。

```
docker-compose -f docker-compose.yml -f docker-compose.mysql.yml up
```


#### Webサーバー・MariaDB

起動時、「-f docker-compose.mariadb.yml」を追加します。

```
docker-compose -f docker-compose.yml -f docker-compose.mariadb.yml up
```


#### Webサーバー・SQL Server

起動時、「-f docker-compose.sqlsrv.yml」を追加します。

```
docker-compose -f docker-compose.yml -f docker-compose.sqlsrv.yml up
```


#### Webサーバー・PhpMyAdmin

起動時、「-f docker-compose.phpmyadmin.yml」を追加します。

```
docker-compose -f docker-compose.yml -f docker-compose.phpmyadmin.yml up
```


#### Webサーバー・Redis

起動時、「-f docker-compose.redis.yml」を追加します。

```
docker-compose -f docker-compose.yml -f docker-compose.redis.yml up
```


#### 冗長構成でWebサーバー起動・ロードバランサー追加

起動時、「-f docker-compose.balancer.yml」を追加し、「--scale web=2」と、起動するWebサーバー数を指定します。    

```
docker-compose -f docker-compose.yml -f docker-compose.balancer.yml up --scale web=2
```


#### Webサーバー・https接続

起動時、「-f docker-compose.https-portal.yml」を追加します。

```
docker-compose -f docker-compose.yml -f docker-compose.https-portal.yml up
```


#### 冗長構成でWebサーバー起動・ロードバランサーにhttps接続・MySQL構築

.envの「EXMENT_DOCKER_HTTPS_TARGET_SERVER」の値を「balancer」に変更してください。

```
docker-compose -f docker-compose.yml -f docker-compose.balancer.yml -f docker-compose.mysql.yml -f docker-compose.https-portal.yml up --scale web=2
```

#### その他、複合して起動する場合

起動時、起動したい環境を「-f docker-compose.XXXX.yml」をそれぞれ追加してください。



## パラメータ設定
「.env」ファイルを開き、設定値を修正します。

#### Webサーバー

| 設定値 | 初期値 | 説明 |
| ---- | ---- | ---- |
| EXMENT_DOCKER_FRONT_DOMAIN | localhost | DockerのExmentにアクセスするためのドメインです。 |
| EXMENT_DOCKER_HTTP_PORTS | 80 | DockerのExmentにhttpアクセスするポートです。※Webサーバーを冗長化する場合に、ポートを複数指定する場合には、"80-90"など、範囲指定を行ってください。 |


#### https

| 設定値 | 初期値 | 説明 |
| ---- | ---- | ---- |
| EXMENT_DOCKER_HTTPS_PORTS | 443 | DockerのExmentにhttpsアクセスするポートです。※Webサーバーを冗長化する場合に、ポートを複数指定する場合には、"443-453"など、範囲指定を行ってください。 |
| EXMENT_DOCKER_HTTPS_STAGE | staging | https環境を構築する設定です。(staging/production/local) 自己証明書を作成する場合、localを指定してください。 |
| EXMENT_DOCKER_HTTPS_TARGET_URL | web | https環境を接続するサーバーのURLです。(例：http://web:80) |


#### バランサー

| 設定値 | 初期値 | 説明 |
| ---- | ---- | ---- |
| EXMENT_DOCKER_BALANCER_HTTP_PORT | 80 | ロードバランサーにhttpアクセスするポートです。 |
| EXMENT_DOCKER_BALANCER_HTTPS_PORT | 443 | ロードバランサーにhttpsアクセスするポートです。 |


#### MySQL・MariaDB

- データベース接続時のホスト名は、MySQLの場合「mysql」、MariaDBの場合「mariadb」になります。

| 設定値 | 初期値 | 説明 |
| ---- | ---- | ---- |
| EXMENT_DOCKER_MYSQL_PORT | 3306 | MySQLにアクセスするポートです。 |
| EXMENT_DOCKER_MYSQL_DATABASE | exment_database | 作成するデータベース名です。 |
| EXMENT_DOCKER_MYSQL_ROOT_PASSWORD | secret | rootアカウントのパスワードです。 |
| EXMENT_DOCKER_MYSQL_USER | exment_user | 作成する一般アカウントのユーザー名です。 |
| EXMENT_DOCKER_MYSQL_PASSWORD | secret | 作成する一般アカウントのパスワードです。 |


#### SQL Server

- SQLServerのデータベース名を変更する場合、ファイル「sqlsrv/initdb.d/setup.sql」のデータベース名を変更してください。初期値は「exment_database」です。
- データベース接続時のホスト名は「sqlsrv」になります。

| 設定値 | 初期値 | 説明 |
| ---- | ---- | ---- |
| EXMENT_DOCKER_SQLSRV_PID | Express | SQL Serverのエディションです。(Developer/Express/Standard/Enterprise/EnterpriseCore) |
| EXMENT_DOCKER_SQLSRV_PORT | 1433 | SQL Serverにアクセスするポートです。 |
| EXMENT_DOCKER_SQLSRV_ROOT_PASSWORD | JKnkdsaj#j3ks | rootアカウントのパスワードです。※8文字以上の複雑なパスワードである必要があります。 |
| EXMENT_DOCKER_MYSQL_USER | exment_user | 作成する一般アカウントのユーザー名です。 |
| EXMENT_DOCKER_MYSQL_PASSWORD | secret | 作成する一般アカウントのパスワードです。 |


#### PhpMyAdmin

| 設定値 | 初期値 | 説明 |
| ---- | ---- | ---- |
| EXMENT_DOCKER_PHPMYADMIN_PORT | 8888 | PhpMyAdminにアクセスするポートです。 |


#### Redis

| 設定値 | 初期値 | 説明 |
| ---- | ---- | ---- |
| EXMENT_DOCKER_REDIS_PORT | 6379 | Redisにアクセスするポートです。 |


## Exmentの設定値を変更する場合
「web/volumes/.env」ファイルが、Exmentのプロジェクトフォルダの「.env」ファイルに該当しますので、そちらを編集してください。
