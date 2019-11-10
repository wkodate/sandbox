Day11 コンテンツキャッシュとインメモリキャッシュ
==

## やること

* CloudFrontについて
* ElastiCacheについて

## CloudFront

* コンテンツキャッシュパターン
    * 前段にCDNを配置しコンテンツをキャッシュすることでWebサーバの負荷を軽減する
* CloudFront
    * AWSが提供するフルマネージド型CDNサービス
    * 世界中に100を超えるエッジロケーションが存在。リージョンがない場所にも存在
    * 他のAWSサービスと連携
* 導入の流れ
    * オリジンサーバの指定
    * キャッシュの振る舞いの設定
    * キャッシュTTLの設定
    * その他の設定
* レスポンスヘッダのX-Cacheにcloudfrontのキャッシュ情報が返される
* CloudFrontを使う際のポイント
    * キャッシュTTLの設定
    * CloudWatchで監視する
    * Popular Object Reportを参考に、定期的に設定を見直す
* Popular Object Reportは、CloudFrontからキャッシュの統計情報を取得できる

## ElastiCache

* インメモリキャッシュパターン
    * 頻繁にアクセスするデータをインメモリにキャッシュしてDB負荷を軽減する設計パターン
* ElastiCache
    * AWSが提供するフルマネージド型インメモリキャッシュサービス
    * セットアップが簡単
    * 多様なノードタイプが提供
    * Redis
        * シングルスレッドで動作
        * データを永続化可能
    * Memcached
        * マルチスレッドで動作
        * データを永続化できない
* 導入の流れ
    * サブネットグループの作成
    * パラメータグループの作成
    * ノードタイプの設定
    * レプリケーションの設定
    * セキュリティグループの設定
    * その他の設定
* ElastiCacheを使う際のポイント
    * キャッシュするデータの選定
    * 対障害性への考慮
    * 負荷テストを実施してノードのタイプを決定する