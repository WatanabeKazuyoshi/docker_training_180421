# README

### 1)Cloneしたら、以下を編集（必須）

/docker-compose.yml

「testapp」フォルダ＆「docker-compose.yml」を置くフォルダのパスを記入します。

```/docker-compose.yml
version: '2'

services:
  db:
    image: mysql
    volumes:
      - [開発するアプリを置く、PC上のフォルダのパス]:/home
    environment:
      MYSQL_ROOT_PASSWORD: mysql
      TZ: Asia/Tokyo

  app:
    image: rails:5
    # build:
    #   context: .
    #   dockerfile: Dockerfile-rails
    stdin_open: true
    tty: true
    volumes:
    - [開発するアプリを置く、PC上のフォルダのパス]:/home
    environment:
      TZ: Asia/Tokyo
    ports:
      - "3000:3000"
    links:
      - db:db
```



### 2) 「testapp」フォルダのフォルダ名を編集（任意）

好きな名前に変更。確か、docker上のアプリの名前になります。



### 3)アプリを作る

アプリの環境を作ります。

```
$ cd testapp
$ docker-compose build
$ docker-compose up -d
```

Appの仮想環境に入ります

```
$ docker exec -it testapp_app_1 /bin/bash #アプリの名前は自分が名付けた名前に変えてください
```

Appの仮想環境で、アプリを入れたフォルダ（/home）に移動します

```
$ cd /home
```

アプリを作ります

```
# rails new アプリ名 --database=mysql
```



#### アプリの階層に移って、アプリを起動

```
cd アプリ名
rails s
```

mysqlの設定をしていないのでエラーが起きるが問題ない。



#### database.ymlを設定: password, host

＜変更前＞

```
default: &default
  adapter: mysql2
  encoding: utf8
  pool: 5
  username: root
  password:
  host: localhost
```

＜変更後＞

```
default: &default
  adapter: mysql2
  encoding: utf8
  pool: 5
  username: root
  password: mysql #ここは先ほどdocker-compose.ymlで設定した
  host: db #ここは先ほどdocker-compose.ymlで設定した
```

またエラーが起こる：Unknown database 'XXX'

#### データベースを作る

別のターミナルを立ち上げる（左下の小さいアイコンが、Terminal）

##### dbの環境に入る

```
$ docker exec -it railsapp180421_db_1 /bin/bash
```

##### 仮想環境場で、mysqlを起動

```
# mysql -u root -p
```

パスワードを聞かれるのでmysqlと入力

いよいよDBを作る

```
mysql> create database アプリ名_development;
```

mysqlからexitする

Localhost:3000 にアクセスすると、アプリが立ち上がってるはず。

あああ