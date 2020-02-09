# 使い方
masterブランチへのpushをトリガーとしてGithub Actionsが作動し, ReactアプリのbuildとGithub Pagesへの公開を自動で行います。

このリポジトリをclone後、gitのremoteを自分のgithubのリポジトリのURLに変更してpushする。
※ forkした場合、Github Actionsが動作しない。

## 新しくReactアプリを作成する場合

既にサンプルのhello-worldアプリが作成されているのでこれを削除し、以下のコマンドでcreate-react-appを実行する。

```
docker-compose run --rm node sh -c "create-react-app my-app"
# my-appを自分のアプリ名に変える
```

docker-compose.ymlのenvironmentのREACT_APP_NAMEを自分のアプリ名に変更


```
version: "3"
services:
  node:
    build:
      context: ./app
    environment:
      - NODE_ENV=production
      - REACT_APP_NAME=my-app # ここのmy-appを修正
    volumes:
      - ./app:/usr/src/app
    ports:
      - "3000:3000"
```

また、app/my-app/package.jsonにhomepageを追加します。

```
{
  "homepage": ".", # ここを追加
  "name": "my-app",
  "version": "0.1.0",
  "private": true,
...
```

## 作成済みのアプリを動かす場合(初回のみ)

- packageのインストールが必要

```
docker-compose run --rm --service-ports node ash -c "cd \$REACT_APP_NAME; yarn install"
```

# アプリケーションの開発を行う

- 開発サーバーを立ち上げる

```
docker-compose run --rm --service-ports node ash -c "cd \$REACT_APP_NAME; yarn start"
```

- コードを修正する

この状態でホストマシン上で'app/my-app'内を修正すればビルドされます。

- パッケージを追加する

```
docker-compose run --rm  --service-ports node ash -c "cd \$REACT_APP_NAME; yarn add package-name"
```


# Github Pagesが公開されない

github pagesへの反映には若干時間がかかるようですが、公開されない場合以下を試すと表示されることがありました。

リポジトリのSettingsのGithub Pagesで使用するブランチを指定できます。これを

- デフォルトのgh-pagesからmasterに変更
- masterからgh-pagesに戻す

を行うと公開されました。

