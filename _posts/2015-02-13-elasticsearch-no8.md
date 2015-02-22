---
layout: post
title: "2015-02-13 第8回 elasticsearch 勉強会 @ 丸の内 リクルート 41Fアカデミーホール"
date: 2015-02-14 10:00:00 +0900
comments: false
---

# Disclaimer

※個人的なメモであり、勉強会の前後で調べたことも書かれています。

レポとして私より全然よくまとまっているのは[こちら](http://blog.yoslab.com/entry/2015/02/13/203251)だと思います。

---

# イベント概要

http://elasticsearch.doorkeeper.jp/events/19923

ハッシュタグ [#elasticsearchjp](https://twitter.com/search?f=realtime&q=%23elasticsearchjp)

<!--more-->

# 19:30 ~ 導入チェックリスト 大谷さん

## 質問

* esつかっているひと、ノ　←　半分くらい？
* es本番で使っている人、ノ　←　半分くらい？
* 導入チェックリスト、公式　→　英語：[preflight checklist](http://www.elasticsearch.org/webinars/elasticsearch-pre-flight-checklist/)
* 大谷さん　日本で一人目、コミッタ。elasticsearch 何でも屋
* lucene-gosen (形態素解析)コミッタ（lucene-gosenは、elasticsearchのkuromojiの前）
* elasticsearch「社」 , kibana, marvel 東京でのトレーニングもやるかも

## インストールから起動

* 解凍　→　bin/elasticsearch 叩くだけ

## 本番で気をつけること

* 最新版を利用する（セキュリティ、パフォーマンス）
* java も新し目を。SDKにバグあるので古いのはNGで lucene壊れる可能性（java7 25 ~ 55はバグあり）

## OS関連

* ファイルデスクリプタ　32,000 ~ 64,000 位をOSに設定（インデックスをファイルに落とすため）
* 確認方法は起動オプション、URLたたく、の2種類

## メモリ

* java heap 32GがMAXで、物理メモリの半分以「下」に設定する
* ヒープのmax と minを同じ値に
* ↑OSレベルでのファイルキャッシュをさせるため（ヒープにGCを走らせないため）
* ６４GBを立てるよりも、３２GB のプロセスを２つ立てる

## メモリ（スワップ）

* OSにスワップさせない設定　←[公式ガイド](http://www.elasticsearch.org/guide/en/elasticsearch/guide/current/heap-sizing.html#_swapping_is_the_death_of_performance)にもあり

## Elasticsearch関連

### cluster.name
* 名前が同じだと、周りのマシンとクラスタが組まれちゃう
* 名前は指定する（事故る↑ので）
* 設定ファイル(yml)か、起動時に指定するか

### node.name
* デフォルト：Marvelのヒーローの名前。キャプテンアメリカとかスパイダーマンとか（起動時にランダム）
* ↑本番とかで設定しないと、nodeを名前ベースでやっているとおかしくなるときがあ
* 設定ファイル(yml)か、起動時に指定するか

## ネットワーク関連

* nic が複数のとき
* 利用するnicの名前やIPアドレスで指定する

### discovery.zen.ping
* 勝手にクラスタを探す・構成する
* unicast 設定にして、クラスタに参加するマシンを指定
* unicast設定を書くだけじゃなく、multicastもfalseにする必要あり

### minimum_master_nodes
（???この辺りちょっと聞き取れてなかった）
* マスターノードになれる人
* (n / 2) + 1
* クラスタは3台以上で組む

## ディレクトリ関連

### path.conf

* 設定ファイルのパス
* ログ
* インデックスデータの保存場所（elasticsearch インストール場所配下にはしない。バージョン変更時の上書きが怖い）
* プラグイン

↑は[公式ガイドのSetup](http://www.elasticsearch.org/guide/en/elasticsearch/guide/current/_important_configuration_changes.html#_paths)に書いてある。オライリー本がOSSで公開されている

## 質問

* java 1.7.25 ~ 1.7.55 はバグあり。それより古いのはバグないけど、おすすめしない。java 8化を進めてる
* メモリの利用用途は？
	* ファイルキャッシュ、検索・集計、ソート　？ヒールドデータ　メモリにインデックスがガツッと乗る。キャッシュ
	* Doc value はメモリ効率いい（一旦ファイルに置く？仕組み）

---



# 20:00 Elasticsearch クエリとスキーマ定義のすごい細かい話 ドワンゴ 藤堂淳也 さん

[@junya_todo](https://twitter.com/junya_todo)

[スライド](https://speakerdeck.com/jtodo/elasticsearch-kueritosukimading-yi-falsexi-kaihua)は上がってる

ドワンゴの検索チームの人
運用寄りの仕事

## 要件に柔軟

* ニコニコの検索基板をElasticsearchに一元化
* 検索フィルタの追加、他サービスのデータを利用。フィールドの追加で対応できる！
* 公式のガイドラインが良い。検索窓でよく引っかかる
* 要件は単純なのと複雑なのがある
* rolling restart でアナライザの書き換え（無停止でフィールド追加！）
* [前回のドワンゴの資料](https://speakerdeck.com/shoito/niconico-elasticsearch)良い資料！
* [refresh_interval](http://www.elasticsearch.org/guide/en/logstash/current/plugins-filters-translate.html#plugins-filters-translate-refresh_interval) 15s

### 利用クエリ

* Aggregate （トップいくつ、とか）

### 複雑なケース

#### 例１．テキスト検索のノイズを減らしたい
* bigramアナライザーを最初使ってた。
* phrase 検索の副作用、booleanより負荷が高い
* ドキュメント読もう！
* phraseには位置情報が必要。 index_optionで削減設定していた。

#### 例２．環境依存文字で検索したい
* ICU 正規化プラグイン

#### 例３．ある時点の検索結果を並べ替えつつ全件取得
* デフォルトの検索タイプ　query then fetch
* scan & scroll　←リアルタイム用ではない

## 負荷検証で自作ツール使ってる。
* キャッシュの動きを知る
* Marvel [marvel知らなかった](http://blog.johtani.info/blog/2014/01/29/simple-introduction-and-first-impression-es-marvel/) → elasticsearchクラスタモニタリングツール
* filter 句ごとにfilter cacheする
* [Benchmark API](http://www.elasticsearch.org/guide/en/elasticsearch/reference/master/search-benchmark.html) ←これかな（APIで負荷検証）

## まとめ
* スキーマ追加楽
* __ドキュメントよめ__

# 質問

* 削除ドキュメントはフラグが立つだけ

---

20:30-35 休憩

---


# 20:35 ~　サイバーエージェント 山田さん

[@satully](https://twitter.com/satully)

[スライド](http://www.slideshare.net/Satully/elasticsearch8-elasticsearchkibana-30reqday)

* [前々回の資料](http://www.slideshare.net/Satully/elasticsearch-study6threaltime20140916)も上がってる
* smalgo
* Adネットワーク、DSP

## smalgo

* ディスプレイ広告の配信

## インフラについて

* Redshift使ってる
* [kibana](http://www.elasticsearch.org/overview/kibana/)
* [Tableau](http://www.tableau.com/ja-jp)（タブロー。BI、可視化）
* ElasticSearch はリアルタイム、Redshift・Tableauはマーケティング、可視化

* 月900億リクエスト
* Fluentdは１サーバ：１プロセスで立ち上げ、
* Kibanaはリアルタイム状態をみる
* Ephemeral ストレージはやい

## Kibana の分析項目（実例）

* Java のException
* Kibana1秒単位で見れる
* パイチャート、割合グラフ（棒、時系列）、積み上げグラフ

---

20:55 - 21:05 41Fから33Fへ移動　（手違いで場所が取れてなかった）

---

つづき（各種グラフ）


## まとめ

* elasticsearch 安定してる！

## 質問

* アーキテクチャ。fluentd で送るときにELB挟んでる。fluentdのLB機能ができるのに、なんでやってない？　→　特に理由ない。fluentdのLB機能で出来そう
* インデックスの構成は？ →　☆not analized １日２インデックス
* ☆SearchNode やめた理由は？　→　不要なのではと気づいてやめた

----

# 21:23 ~ はてな　シニアエンジニア

[@yanbe](https://twitter.com/yanbe)

* 最近はアドテクやってる

## 自社サイトのメディア面を拡張する

はてブのDBを、検索条件を変えるだけで「女性向け」などの見せ方にすることができる（B!KUMA）

* 「自動カテゴリ編成機能」
* はてなブックマーク本体の機械学習の結果もインデックスに
* [query DSL](http://www.elasticsearch.org/guide/en/elasticsearch/reference/current/query-dsl.html#query-dsl) （JSON）
* 非エンジニアでもチューニングできる（管理画面から、query DSLへの変換）

## トピック機能

* 実装　elasticsearch 1.3 [significant terms aggregation](http://www.elasticsearch.org/guide/en/elasticsearch/reference/current/search-aggregations-bucket-significantterms-aggregation.html#search-aggregations-bucket-significantterms-aggregation)
* この機能を最新の記事に使うと、その日の話題っぽいのが取れる（でも精度は悪い）
* aggregationをネストして、フィルタリングしていく感じ？
* 精度がいまひとつだったので、自然言語処理エンジニアに引き継いで、トピック機能としてリリース


## まとめ

* トップダウンのサービス　→　カテゴリ自動編成
* ボトムアップのサービス　→　トピック機能がesの機能からスタートしている

## 余談

* url そのもののインデックスは失敗しやすい
* multi-field をキーワードにドキュメント検索！

## 質問タイム

* クラスタの台数は？ master 3台、データノードレプリケーション数１で４台　7000万URL、ブックマーク1億4000万
* 管理画面からのDSLは？　→ 数10kbくらい
* 管理画面は何でつくった？　→ perlとjavascript

---

# 大谷さんから告知

次回４月中旬頃

---

# 個人まとめ

* es触ったことないのに参加しちゃってすみません
* でも雰囲気はわかりました。
* はてなの件、技術ベースで企画のアイデアが出てくるのが面白いなーと思った

<blockquote class="twitter-tweet" lang="ja"><p>elasticsearch雰囲気しかわからなかったけど、技術的な発見というか発想からの企画のボトムアップってなかなか成り立たないよなーと関心&#10;実行されるには理解されないといけないけど、技術的なところからスタートだとみんなに理解してもらうのが難しい。専門じゃない人、非エンジニアの人</p>&mdash; takudo (@takudo_dev) <a href="https://twitter.com/takudo_dev/status/566220860675993600">2015, 2月 13</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

---
# あわせて読みたい


[勉強会メモ - 第8回elasticsearch勉強会 - よしだのブログ](http://blog.yoslab.com/entry/2015/02/13/203251)

---

# おまけ


<blockquote class="twitter-tweet" lang="ja"><p>あと、10分だけど、80人も来るのかな？</p>&mdash; Jun Ohtani (@johtani) <a href="https://twitter.com/johtani/status/566180703394488320">2015, 2月 13</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

<blockquote class="twitter-tweet" data-partner="tweetdeck"><p>今回の勉強会は、チェックインしたのが112で、枠が180で、キャンセルしてなくて来てない人が67。キャンセル待ちの人が15名。キャンセル待ちで繰り上がらなかった方すみませんでした。 <a href="https://twitter.com/hashtag/elasticsearchjp?src=hash">#elasticsearchjp</a></p>&mdash; Jun Ohtani (@johtani) <a href="https://twitter.com/johtani/status/566256619906801665">February 13, 2015</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

マナーが問われる勉強会