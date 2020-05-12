# myfav-redmine

## はじめに

Redmine に自分が気に入ったプラグインやテーマを詰め込んだものを簡単に構築できるように、Docker Compose を使って作ってみました。
発端や経緯などは下記の Qiita 記事を参照ください。

* [Docker Compose を使って自分好みの Redmine 実行環境を作ってみた](https://qiita.com/juno-nishizaki/items/45ff4186feb75d512306)
* [Redmine draw.io plugin がすごい便利そうなので紹介したい](https://qiita.com/juno-nishizaki/items/f3e5f192935de01ce49b)
* 執筆予定： Docker Compose で作った Redmine だって Full Text Search plugin と ChupaText サーバーを連携させたい

## 動作確認環境

以下の環境で動作確認しています。

* OS: Ubuntu 18.04
* Docker: 19.03.8
* Docker Compose: 1.25.4

## 起動までの手順

1. リポジトリからファイル一式を取得して、Docker ホストの任意のディレクトリに格納します。
    * 以降は `/opt/myfav-redmine` に格納した前提で説明します。
1. `/opt/myfav-redmine/redmine/config/configuration.yml` にメールの設定などをします。
    * 設定の詳細は [Redmineガイドのメールの設定](http://guide.redmine.jp/Email_Configuration/) を参考にしてください。
    *  Docker Compose からビルド・起動させると、このファイルが Redmine の Docker コンテナに取り込まれます。
1. Redmine 添付ファイルやログの出力先を以下のパスにバインドマウントしているので、必要に応じて `/opt/myfav-redmine/docker-compose.yml` の volumes を変更してください。
    * 添付ファイル： `/srv/redmine/files`
    * ログ
        * Redmine 本体： `/var/log/redmine`
        * ChupaText サーバー： `/var/log/redmine-chupa-text`
1. データベース名／ユーザー／パスワードを `/opt/myfav-redmine/docker-compose.yml` に直書きしているので、必要に応じて変更してください。（動作の確認する程度の利用であればそのままでもよいです）
1. 以下のコマンドを実行して、Docker Compose からビルド・起動させます。

    ```bash
    $ cd /opt/myfav-redmine
    $ docker-compose up -d --build
    ```
1. http://localhost:3000 にアクセスして、Redmine の初期設定を行います。
    * データベースに登録されるタイプの設定は素の Redmine と同等です。そのため、[デフォルトデータのロード](http://redmine.jp/tech_note/first-step/admin/load_default_data/) も必要となります。
    * 添付ファイルの全文検索を有効にするために、Full Text Search plugin の設定（ http://localhost:3000/settings/plugin/full_text_search ）に以下を追加してください。
        * ChupaTextサーバーのURL： http://chupa-text:3000/extraction.json

### 設定の補足

* Redmine にプラグインやテーマを追加したいときは、 `/opt/myfav-redmine/redmine/Dockerfile` を編集してください。（周辺の行を真似れば追加できると思います） `docker-coompose down && docker compose up -d --build` で実行環境を更新できます。
    * Dockerfile のビルド時に毎回プラグインのマイグレート（ `redmine:plugins:migrate` ） がかかるようにしています。
* 機能として必須ではないですが、Web フロントエンドに Nginx などを置いて、Let's Encrypt などで HTTPS 化しておいた方が望ましいです。
    * 僕は Nginx だけは Docker ホスト側にインストールしていて、バックエンドとしてこの Docker Compose 環境を動かしています。同じ Docker ホスト上に他にもいくつかサービスを動かしていることもあって、この構成の方が扱いやすかったという事情があります。
    * 特にセキュリティ上の問題がなく、このあたりの設定が不慣れでしたらそのままでもよいですし、ホスト側にバインドするポート番号を 80 番に変更してもらってもよいです。

## 未搭載の機能

* リマインダーの設定は組み込んでいません。
* ChupaText サーバーのログをローテーションする仕組みは組み込んでいません。


## 構成

### Docker コンテナのサービス群

| サービス    |  使用バージョン | 特記事項 |
|:---|:---|:---|
| Redmine      | 4.1 系の最新版 | 後述のプラグイン、テーマを追加インストール |
| PostgreSQL   | 12 系の最新版 | PGroonga、 TokenMecab を追加インストール |
| ChupaText    | 最新版 | |

### Redmine プラグイン

使用バージョンはすべて最新版です。

* [Redmine Banner plugin](https://github.com/akiko-pusu/redmine_banner)
* [Redmine Issue Badge plugin](https://github.com/akiko-pusu/redmine_issue_badge)
* [Redmine Issue Templates plugin](https://github.com/akiko-pusu/redmine_issue_templates)
* [Sidebar Hide Plugin](https://github.com/bizyman/sidebar_hide)
* [Scheduling Poll plugin](https://github.com/cat-in-136/redmine_scheduling_poll)
* [Full Text Search plugin](https://github.com/clear-code/redmine_full_text_search)
* [DMSF](https://github.com/danmunn/redmine_dmsf)
* [Redmine Theme Changer plugin](https://github.com/haru/redmine_theme_changer)
* [Redmine message customize plugin](https://github.com/ishikawa999/redmine_message_customize)
* [Redmine Drafts plugin](https://github.com/jbbarth/redmine_drafts)
    * 依存関係解決のため [Redmine Base Deface plugin](https://github.com/jbbarth/redmine_base_deface) を追加している
* [Redmine Drawio plugin](https://github.com/mikitex70/redmine_drawio)
* [View Customize plugin](https://github.com/onozaty/redmine-view-customize)

### Redmine テーマ

使用バージョンはすべて最新版です。

* [Redmine用テーマ "farend bleuclair"](https://github.com/farend/redmine_theme_farend_bleuclair)
* [Redmine用テーマ "farend basic"](https://github.com/farend/redmine_theme_farend_basic)
* [Redmine用テーマ "farend fancy"](https://github.com/farend/redmine_theme_farend_fancy)
* [Redmine gitmike theme](https://github.com/makotokw/redmine-theme-gitmike)
* [Redmine theme for kids / Kodomo Redmine](https://github.com/akiko-pusu/redmine_theme_kodomo)
* [Redmine theme for kids midori version / Kodomo Redmine green version](https://github.com/akiko-pusu/redmine_theme_kodomo_midori) 

## ライセンス

本リポジトリに格納している docker-compose.yml および Dockerfile は MIT です。（configuration.yml は Redmine 本体からコピーしてきているので Redmine のライセンスに従います）
Redmine 本体やプラグイン、テーマおよび関連する Docker コンテナのサービス群についてはそれぞれのライセンスに従います。
