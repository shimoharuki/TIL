# Dockerの良いところ

ローカル環境でruby rails などをインストールせずに、docker環境内で開発を行うことができる

# 手順

railsをdocker環境で起動させたいためdockerにどのrubyを呼び出すかなどの宣言をする`dockerfile`を作成

```

        #FROM(フロム)はDocker に対して ベースとなるRubyイメージを指定する
FROM ruby:2.5.1
        #run(ラン)はdocker-compose buildコマンドで実行される
        #Railsの起動に必要となるnodejsをインストールする
        #apt-getコマンドはUbuntuパッケージ管理システム＝APTライブラリを利用してパッケージ操作・管理するコマンド
        #①updateで常に新しいパッケージinstall →②-qqはエラー以外は表示しない →③常にyesの-y指定
        #→④ubuntuのbuild-essential(OS基礎開発パッケージ) → ⑤libpq-dev(OS開発パッケージ）
        #→⑥nodejs(サーバーサイドのJavaScript)  の順番でインストールする
        # \ バックスラッシュでコードを改行して見易くする       (\ はoption+¥です)
RUN apt-get update -qq && \
        apt-get install -y build-essential \
                                libpq-dev \
                                nodejs
        #今回はapp_nameという名前のディレクトリ（場所）を作ります
        #mkdir(メイクディレ)=(make directory)
        #ディレクトリ名は自由です
RUN mkdir /app_name
        /#WORKDIR(ワークディレ)は、RUNやADDなどの命令実行するカレントディレクトリ
        #カレントディレクトリ（作業位置）をapp_nameに移動（cdコマンドと同じ）
WORKDIR /app_name
        /#COPY(コピー)はローカル側のファイルをdockerイメージ側の指定したディレクトリにコピーする
        #ローカルのGemfileをapp_name/Gemfileにコピーする
        #ローカルのGemfile.lock をapp_name/Gemfile.lockにコピーする
        #docker-compose build 実行する前に、ローカルのGemfile.lock内を全削除しておきます（エラー対策）
COPY Gemfile /app_name/Gemfile
COPY Gemfile.lock /app_name/Gemfile.lock
        #gem install bundler インストールを実行する
        #bundle install を実行する
RUN gem install bundler
RUN bundle install
        #ADD(アド)はローカル側のファイルをdockerイメージ側の指定したディレクトリに追加（コピー）する
        #ローカルの(.)カレントディレクトリをコンテナのapp_nameディレクトリに追加（コピー+解凍）する
ADD . /app_name

```

環境の指定ができれば次はdb,apiなどの設定を行う`dockercompose`を作成

```

        #docker-composeのバージョン(2020.4.12時点では"3"で問題なさそう)
version: '3'
        #servicesは各コンテナを定義します
services:
        #dbは1つのコンテナ名です（１コンテナ１プロセス）
        #ローカルmysql version確認 → mysql  Ver 14.14 Distrib 5.6.47
        #DockerHubでmysql5.6を確認できたので5.6と記述
  db:
    image: mysql:5.6
        #environmentは環境変数を追加します
        #環境変数MYSQL_ROOT_PASSWORDに、今回はpasswordと入れます（パスは自由）
        #パスワードの直打ちは危険なので、環境変数を使って秘匿します
    environment:
      MYSQL_ROOT_PASSWORD: 'password'
        #portsは公開用のポートで、ホスト側とコンテナ側の両方のポートを指定します（ ホスト側:コンテナ側 ）
        #ローカルMYSQLはポート確認すると3306を使用中なので、ホスト側は4306にします（4にした深い意味はありません）
        #ローカル側MYSQLは大抵は自動で3306になっているそうです
        #Sequelproにて4036(例として)を指定すると、DockerのMYSQLがSequelproで見れるようになります。
    ports:
      - "4306:3306"
      #webは1つのコンテナ名です（１コンテナ１プロセス）
      #buildはdockercompose.ymlを基点に指定フォルダー配下のDockerfileを元にコンテナを作成します。
      #指定は .  ルート （つまり、ルート直下に置いた先ほど作成したDockerfileを参照する、という記述です）
  web:
    build: .
      #commandは任意にコマンドを上書きできます。
      #bashはOSカーネルとユーザの橋渡しするプログラムシェルです
      #起動停止の繰り返しで起こるエラー「A server is already running.」の原因、pidをbuildのたび削除する（unicorn master KILLと同じですね）
      #Rails serverを立ち上げた際にURL(http://0.0.0.0:3000)にアクセス=localhost:3000
    command: bash -c "rm -f tmp/pids/server.pid && bundle exec rails s -p 3000 -b '0.0.0.0'"
      #volumesではパスを指定するとDockerエンジンはボリュームを作成します
      #カレントディレクトリ(.)をapp_nameディレクトリにマウントします。
    volumes:
      - .:/app_name
      #portsは公開用のポートのことでホスト側とコンテナ側の両方のポートを指定します（ ホスト側:コンテナ側 ）
      #localhost:3000にアクセスするとコンテナの3000ポートにつながります
    ports:
      - "3000:3000"
      #depends_onはサービス間の依存関係を指定します
      #webコンテナ側からdbコンテナにdbの名前で接続できるようになります
      #docker-compose up を実行したら、web:コンテナのプロセスを開始するより前にdb:コンテナのプロセスを先に実行します
    depends_on:
      - db

```

最後にdb側の`database.yml`を設定

```
default: &default
  adapter: mysql2
  encoding: utf8
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  nickname: root
  password: password　　#（←ここ、passwordを追記します）
  socket: /tmp/mysql.sock
  host: db             #（←ここ、host:dbを追記します）

```
dockerファイルに記述したこのイメージでサーバーを構築してください。

というコマンドを実行

```
docker-compose build

```

構築したイメージ内でdbを作成し、migrateを行う

最後にコンテナ(サーバー)を起動する

```
docker-compose up

```

# dockerfile のよくある命令文

`FROM`どのdockerイメージを使用するか今回だとruby2.5.1

多分これがないとどの言語が使用されるかわからない状態だから必須か?

`COPY`現ディレクトリからファイルをコピー

上記の場合だとGemfile,Gemfile.lockのコピーを行っている

`RUN`命令実行を行うこと

mkdirでapp_nameを作成したり、bundleinstallを実行したり色々なことができると考えて良さそう。

`CMD`コンテナ内で実行するコマンドを指定

今のとこよくわかっていない

# dockerfile作成の注意点

dockerfileを作成するときはエフェメラル(一時的)なコンテナを作成することが望ましい。

すぐに停止、破棄が可能であり、圧倒的に最小限であることが条件

# dockercompose.ymlについて

書き方が大体定まっている感じがする

大きく分けて3つに分かれる

`version`docker-composeのバージョン

`services` db指定の際の場所　　どのdbを使用するか(image)環境変数の指定(environment)公開用のポート(ports)
