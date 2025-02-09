# SQbase

SQbase（エス・キュー・ベース）は、シンプルなサイト構成とサンプル コンテンツを組み込んだ Drupal のインストール プロファイルです。Drupal によるホームページやブログの構築、サイト構築トレーニングなど自由にお使いください。デモサイトを用意しています。

- [http://sqbase.white-root.com/](http://sqbase.white-root.com/)

## 使い方

SQbase のプロジェクトを取得します。

```bash
$ git clone https://github.com/bkenro/sqbase.git sqbase
$ cd sqbase
$ composer install
```

サイトエイリアスのひな型をエディタで開きます。

```bash
$ vi drush/sites/self.site.yml
```

仮想マシンを想定した @local と、DDEV を想定した @ddev というサイトエイリアスのサンプルを用意しています。お使いの環境に合わせて必要な変更を加えて保存してください。

@local を使用する場合はデータベースを作成します。

```bash
$ drush @local sql:create -y
```

準備ができたら最後にサイトをインストールします。`drush site:install` コマンドにインストールプロファイル名 `sqbase` を指定して実行します。

```bash
$ drush @local site:install sqbase -y
```

ブロックが見つからない旨の警告が表示されますが、その後インポートされます。最後まで実行されれば完了です。ブラウザで接続すると初期構成のサイトが開きます。

## レシピのサンプル

このプロジェクトには、次の [Drupal Recipes](https://www.drupal.org/docs/extending-drupal/drupal-recipes) サンプルが含まれています。

### bkenro/sqbase-dev

開発サポートのレシピとして次のモジュールを組み込む。

- [Devel](https://www.drupal.org/project/devel)
- [WebProfiler](https://www.drupal.org/project/webprofiler)

### bkenro/sqbase-backup

次のモジュールを導入して、基本的なデータベースの日次バックアップを構成する。<br>
（データベースの接続情報を適宜設定する必要あり）

- [Backup and Migrate](https://www.drupal.org/project/backup_migrate)

各レシピのソースは、簡易的にプロジェクト内の recipes フォルダ下のサブフォルダとして提供し、composer.json の repositories 設定でリポジトリとして認識させています。適用したレシピのインストール先は web/recipes フォルダの下になります。

使用例：

```bash
$ composer require bkenro/sqbase-dev
〜
$ drush cr
$ cd web
$ php core/scripts/drupal recipe recipes/sqbase-dev
5/5 [▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓]
Applied SQbase dev recipe.

 [OK] SQbase dev applied successfully
 ```
