# SQbase

SQbase（エス・キュー・ベース、愛称「すくべー」）は、シンプルなサイト構成とサンプル コンテンツを組み込んだ Drupal のインストール プロファイルです。[Drupal Meetup 豊田](https://drupal-meetup-toyota.connpass.com/)の書籍執筆活動で得られた知見に基づいて作成したものです。

## 使い方

まず、Drupal 9 が動作する LAMP スタックと Composer がインストールされた環境を用意します。
Vagrant と VirtualBox をお使いの場合は、[ffdsm](https://github.com/bkenro/ffdsm) が利用できます。
以下、ffdsm を例にインストール例を示します。

SQbase のプロジェクトを取得します。

```bash
$ cd /var/www
$ git clone https://github.com/bkenro/sqbase.git sqbase
```

取得したプロジェクトのディレクトリに移動して、依存パッケージをインストールします。

```bash
$ cd /var/www/sqbase
$ composer install
```

サイトのインストール時に参照されるサイトエイリアスのファイルをエディタで開きます。

```bash
$ vi drush/sites/self.site.yml
```

プロジェクトのルートディレクトリ、サイトの URL、およびデータベース作成（sql:create）とサイトインストール（site:install）の Drush コマンドオプションが設定されています。
使用する環境に合わせて必要な変更を加えます。

drush/sites/self.site.yml

```yml
local:
  root: /var/www/sqbase
  uri: http://sqbase.internal
  command:
    sql:
      create:
        options:
          db-su: 'dba'
          db-su-pw: 'dba'
          db-url: 'mysql://sqbase:sqbase@localhost/sqbase'
    site:
      install:
        options:
          db-url: 'mysql://sqbase:sqbase@localhost/sqbase'
          locale: ja
          account-name: 'admin'
          account-pass: 'adminpass'
          account-mail: 'admin@example.com'
          site-name: 'SQbase'
          sites-subdir: 'default'
```
`db-su` と `db-su-pw` はそれぞれ、データベースの管理ユーザーのユーザー名とパスワードです。
`db-url` は作成するデータベースの接続情報を指定する URL です。
初期状態で文字列中に `sqbase` が 3 回出現しますが、最初がユーザー名、2 番目がパスワード、最後がデータベース名です。必要に応じて変更してください。

`account-name`、`account-pass`、`admin@example.com` はそれぞれ、インストールする Drupal サイトの管理ユーザー（uid = 1）のユーザー名、パスワード、メールアドレスです。

`site-name` に指定した文字列は、インストールするサイトのサイト名になります。

必要な変更を加えたら保存して閉じます。

次に、サイトで使用するデータベースを作成します。

```bash
$ drush @local sql:create -y
```

正常にデータベースが作成できたら、最後にサイトをインストールします。
drush site:install コマンドにインストールプロファイル名（sqbase）を指定してください。

```bash
$ drush @local site:install sqbase -y
```

途中、ブロックが見つからない旨の警告が表示されますが、その後のステップでインポートされるので問題ありません。

```bash
 You are about to:
 * DROP all tables in your 'sqbase' database.

 // Do you want to continue?: yes.

 [notice] Starting Drupal installation. This takes a while.
 [notice] Performed install task: install_select_language
 [notice] Performed install task: install_download_translation
 [notice] Performed install task: install_select_profile
 [notice] Performed install task: install_load_profile
 [notice] Performed install task: install_verify_requirements
 [notice] Performed install task: install_verify_database_ready
 [notice] Performed install task: install_base_system
 [notice] Performed install task: install_bootstrap_full
 [notice] Performed install task: install_profile_modules
 [notice] Performed install task: install_profile_themes
 [warning] The "block_content:babe213b-d722-4619-b414-dbe771a653eb" was not found
 [warning] The "block_content:a2e01f8f-52cc-4841-b70e-f689fd602712" was not found
 [warning] The "block_content:64e96fee-0b0d-4a42-b19c-33cbc01bae8b" was not found
 [warning] The "block_content:fc628ea8-385d-47db-929c-9f254fe05ee1" was not found
 [warning] The "block_content:ce5d94d6-6d91-42f0-ac9d-4324c437c7ed" was not found
 [warning] The "block_content:96993c0a-ed1a-4c9e-ae2a-c066fdf1f226" was not found
 [warning] The "block_content:b9cc58c2-8d88-478f-bfd3-b87de8602c3c" was not found
 [warning] The "block_content:2bfd7917-dc97-4339-a797-76d5ae882068" was not found
 [notice] Performed install task: install_install_profile
 [notice] Translations imported: 8513 added, 0 updated, 0 removed.
 [notice] Performed install task: install_import_translations
 [notice] Performed install task: install_configure_form
 [notice] Translations imported: 429 added, 677 updated, 0 removed.
 [warning] No configuration objects have been updated.
 [notice] Performed install task: install_finish_translations
 [notice] Performed install task: install_finished
 [success] Installation complete.
```

以上で完了です。

Web サーバーの設定でプロジェクトの web サブディレクトリを Web ルートに構成し、対応する URL にブラウザでアクセスすると、初期構成のサイトにアクセスできます。

実際にインストールしたサイトの例はこちら：

- [https://sqbase.white-root.com/](https://sqbase.white-root.com/)

Drupal のサイト構築トレーニングやサンプルサイトの初期構成など、どうぞご自由にお使いください。
