# test-app
検証用アプリ

検証用アプリを作成するにあたって、DB設計、herokuへのデプロイ方法などを以下に示す。
開発環境はローカルで行う。

## Rubyの環境構築
Homebrewやrbenvはインストールされた前提とする。
ホームディレクトリで以下の順でterminalにコマンドを入力

1.rbenv install -l
最新版のRubyを確認する。
検証用アプリのため、常に最新版のRubyをインストールすること

2.rbenv install <version>

3.rbenv rehash

4.rbenv versions
インストールされているバージョンを確認

5.rbenv local <version>
検証用アプリのディレクトリでコマンドを実行。

6.rbenv versions
検証用アプリのrubyのバージョンが最新かを確認
（アスタリスクがついているバージョンが検証用アプリのバージョン）

6.gem install bundler pry pry-byebug pry-doc
必要なgemをインストール

## Railsの環境構築

1.gem install rails
最新のrailsをインストール

2.brew install nodenv
nodenvを使って、Node.jsとyarnをインストールする。
公式：https://github.com/nodenv/nodenv

3.eval "$(nodenv init -)"をzshrcへ記述
nodenvのpathを指定する。

4.nodenv install -l
Node.jsのインストールできるバージョンを確認する。
検証用アプリではとりあえず、16.15.0をインストール。

5.nodenv install <versions>

6.nodenv local <versions>
railsと同様にglobalとlocalで全体かアプリごとかでバージョン管理ができる。

7.npm install -g yarn
インストールしたnodenvのバージョンに合わせたyarnをインストールする

8.yarn add --global eslint prettier
yarnで導入するツールをあらかじめ導入するように設定

## rails new

1.rails _<version>_ new . --database=postgresql --skip-bundle
rails -vでインストールした最新のrailsのバージョンを確認

2.bundle config set --local path "vendor/bundle"
bundle install を実行した際のインストール先が vendor/bundle 配下となり、プロジェクト毎に Gem を導入できるので管理しやすくする

3.bundle install

4.bundle exec rails l
ブラウザでlocalhost:3000で表示されれば完了

## DBの構築

1.herokuでデプロイするため、postgresqlを選択
brew install postgresql
posgresqlをインストールしていない場合は上記コマンドでインストール

2.brew services start postgresql
ターミナルで上記コマンドを入力し、postgresqlのサーバーを立ち上げる

3.psql -d postgres
ターミナルの別タブで上記コマンドを入力し、サーバーに入る

4.CREATE ROLE 名前 LOGIN;
サーバーに入ったら、上記コマンドで新規ユーザーの登録及び、ログイン権限を与える

5.ALTER ROLE 名前 CREATEDB;
上記コマンドでDB作成の権限を与える

6.bundle exec rails db:create
設定ができたら、DBを作る

## herokuへのデプロイ

1.ターミナル上でheroku login
デプロイしたいアプリのディレクトリパスで行うこと
（herokuへの登録は済んでいる前提）

2.ターミナル上でheroku create ○○
サーバーの構築

3.git push heroku main
サーバーの構築ができたら、上記コマンドを実行
herokuのサーバに、アップロードするためのコマンド

4.
