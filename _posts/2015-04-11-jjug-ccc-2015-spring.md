---
layout: post
title: "勉強会：JJUG CCC 2015 Spring @ベルサール新宿 #jjug_ccc"
date: 2015-04-12 21:30:00 +0900
comments: false
tags: 勉強会 Java jjug ccc 
description: 勉強会 Java jjug ccc JJUG CCCは初参戦 なんか知ってそうな話題のセッションは避けたり、見たことないけどフォローしてるひとを、という観点でセッションを選んだ Cassandra使いたくなった。ワークスアプリケーションズはやっぱ良い会社なんだろうなーと思った。パーフェクトJava持って行って井上さんにサイン貰えばよかった... 標準コレクションの効率の悪さの話はためになった 生のこざけさん初めて見た。業務でのAndroidこわくない！ にゃんぱす〜

---
 
http://www.java-users.jp/?page_id=1663

# 先に感想

午前中は用事があったので、13:00〜参戦

* JJUG CCCは初参戦
* なんか知ってそうな話題のセッションは避けたり、見たことないけどフォローしてるひとを、という観点でセッションを選んだ
* Cassandra使いたくなった。ワークスアプリケーションズはやっぱ良い会社なんだろうなーと思った。パーフェクトJava持って行って井上さんにサイン貰えばよかった...
* 標準コレクションの効率の悪さの話はためになった
* 生のこざけさん初めて見た。業務でのAndroidこわくない！
* にゃんぱす〜
* ...しかし全体的にスライドが（まだ）上がってない...

<!--more-->

---

# AB-1 社長が脱RDBと言い出して困りましたが、開き直って楽しんでいる話 by 井上 誠一郎 #ccc_ab1

ワークスアプリケーションズ　エグゼクティブフェロー

* 自己紹介
	* Java8 会社でつかってるよ
	* 著書多し、パーフェクトJavaも
	* ☆Lotus Notes（Document DBのはしり）の開発
	* 2000年代前半　アリエル　設立　→　P2Pアプリ開発
	* 2010年代前半　最近：脱RDB
* RDBを使わずに業務アプリを作れそうに「思えてきた」
	* 捨てたほうがいいとまでは思ってはいない。
* はなさないこと
	* NoSQLの個々の比較
	* ☆CAP定理とかその周辺
* 今日の話の中では、DBにアクセスするものはAP(アプリケーション）として用語を統一
* さきに結論
	* ミュータブルデータ→イミュータブルデータ＆リトライ
	* ☆楽観的CC（Concurency Controle）
	* RDB（☆MVCC、☆ShadowPaging）
* 目次
	* 前置き
	* 何を目指す
	* アーキテクチャ決定まで
	* RDB無しの不安（たくさんある）
	* まとめ

## 前置き
 
* ワークスアプリケーションズの、SaaS　ERP「HUE」
* 「RDB遅い」は誤解。※ちゃんと使えば。
	 * NoSQLでの必要機能の実装により、最終てきには同じ早さになるよね（直感）。RDBMS枯れてるし、総合的には有利じゃね？
* 一方、教科書的なリレーショナルモデルだけでもともと業務アプリをつくっていない。
	 * キャッシュ、非正規化、アプリのバリデーション、アプリのトランザクション（楽観的並行制御、早い者勝ち）
* 今は...
	* 脱RDBでも面白い
	* 脱RDBで圧倒的に早くするにはデータモデル等、ちゃんとしないと
 
## 目指しているもの
* 応答速度100ミリ秒の業務アプリ
	 * ↑はトライアンドエラーを待たずに業務を遂行
		 * 例：キーワードを入力してすぐ候補がでる
		 * 例：帳票レイアウトの素早い切り替え
		 * 例：ゼロベースで作ったスプレッドシート。業務アプリとデータが連動している。スプレッドシートの数式候補。

* 複雑なクエリをDBに寄せるか、アプリに寄せるか。
	* DBに寄せたほうが効率だけど、複雑なバリデーション制御はアプリじゃないと
* RDBの世界観
	* リアルタイムインデクシング（シンプルインデクシング）と、リッチクエリの両立
		* リッチさには限界があり、アプリで頑張らないといけない←ジレンマ
* 宗教論争
	* ☆OLTP、☆OLAPを統合する世界（SAP HANA、VoltDB）
	* リアルタイムインデクシングを諦める世界
* リアルタイムインデクシングの限界
	* リッチなクエリ（自然言語処理、機械学習）は困難
	* クエリのオプティマイザが優秀でも、性能劣化してしまう。
* バックグラウンド処理で割り切る世界：更新系
	* 更新は遅延OKとそうでないものがある。
	* 遅延OKなのは、並列処理、☆投機処理で高速化
* バックグラウンド処理で割り切る世界
	* 並列処理のデータ構造、イミュータブル、楽観的CC
* イミュータブルの、DBとプログラミングの対比（相関）
* バックグラウンド処理で割り切る世界　が失うもの
	* one fact in one place
	* リアルタイムでの一貫性の維持が困難
* Cassandraの特徴：データモデル
	* KVS　☆ワイドローデータモデル
	* ☆CQL（クエリ言語）により開発者の概念モデルをRDB風に
	* ☆スーパーカラム（非推奨）
	* 軽量トランザクション（☆CAS相当のアトミック操作）
* Cassandraの特徴：ネットワークモデル
	* 分散ハッシュテーブル（負荷分散、可用性）
* Cassandraの特徴：ストレージモデル
	* 行指向ストア（OLTP寄り）
	* 書き込み性能も高い
* 選択した戦略：ネットワークモデルの設定
	* ランダムパーティション
		* 高いスケーラビリティ。レンジクエリが出来ない。
	* レプリケーション
		* 3つに冗長化

* 選択した戦略：データモデルの指針
	* イベント、事実の蓄積
	* チェンジセット、変更の蓄積
* 選択した戦略：アプリ開発者に見える世界
	* アプリ開発者はDTOだけを意識。クエリの稲ペイ
	* DTOの変更で、バックグラウンドで別テーブルを自動更新（RDBのCreateIndexとおなじ感覚で使う）
* 選択した戦略：主キーの設計例

* RDBの再発明に近くなってるぶぶんが（コミットログ）
* ☆ラムダアーキテクチャ
	* 裏のインデクシングと、インメモリのデータをマージして、表示する（インデクシングの遅延に対応）

## RDBなしの不安

* リレーショナルモデルがなくても大丈夫？
	* できるけど、効率化と整合性が問題。
	* RDBは実行状態よりも永続状態を表現している、イメージ
	* 結合、集計、ソート
* JOIN
* ACID
	* 緩める、最悪直列でLOCK
* コンシステンシー
* 枯れた実装
	* Cassandra安定してて困ってない
* 開発者の慣れ
	* むしろ長くやっているひとが大変？
	* RDBのアンチパターンが、そのままアンチパターンになるわけではなく、発送の転換が必要
* Cassandraの課題
* それでも残る懐疑心
	* 本当に早くなる？　並列制御を考えずに普通に作ると、ある程度の複雑さを持つアプリでは遅くなっちゃう

## まとめ
* 脱RDBが技術的に正しいか　→　分からない

* Cassandra Summit 2015 やるよー (2015)

---

# G-2 クラウド時代の Spring Framework by 三宅 剛史 （Pivotalジャパン株式会社） #ccc_g2 @TsuyoshiMiyake

* メジャー番号のはなし：いまは４だよ～
* OSSで１０年間残っている。なかなかないね
* Pivotalの人
	* Pivotal
		* Spring, RabbitMQ, redis, Tomcat, ☆CLOUD FOUNDARY, ☆GEMFIRE, ☆Greenplum
		* 全部のプロダクトをOSSに。
* もくじ
	* 振り返り
	* 2015
	* Cloud Foundary
	* Spring Cloud

* （spring の偏見（前セッションでぼろくそにｗ）を解きたい）

# 振り返り

* Spring →　DI
	* ハリウッドの原則
* Springより昔
	* 「J2EE Development without EJB」の7章で初めて紹介。
		* DI,AOP, DAO, JDBC, テンプレートパターン

## Spring 2015

* 図
* Spring Foundationと、3rd party Dependencies が、SpringFrameworkと呼ばれる部分。
* Spring Boot
* ☆Spring IO Platform Distribution
* ☆Spring XD
* ☆Spring Data
* ☆Spring Cloud
* Spring が使いにくい理由
	* XML
		* →コンフィグに変えるよー
	* 依存性がややこしい
		* →親の依存性を宣言しておけば、矛盾なく解決するよー（Spring IO）

* SpringとCloudFoundaryのちょっとした歴史
	* SpringがCloudFoundaryを買収
* Spring/Javaの開発者にとってのCloudFoundary
	* エンタープライズのセキュリティ
		* ↑SpringSecurity（今までのエンタープライズで培ってきたもの）
	* Eclipse CF Plugin 
	* GroovyとGrailsのPivotalやめちゃった

## Spring Cloud

* 分散環境でCloud Nativeなあアプリケーションを制作するためのフレームワーク
* Paas上で利用できるサービスをアプリから使いやすくするためのもの
* SpringCloudConfig
	* 設定（Profile, Property）を外出しにして、REST経由でアクセス
	* モノリシックなアプリ→マイクロサービスの組み合わせ
	* ConfigServerが設定を提供
	* バックエンドはGitのリポジトリ
	* 暗号化のサポート
* Spring Cloud Bus
	* ぶら下がっている分散環境の設定の同期をとる。
	* AMQPのみサポート
* Spring Cloud Netflix
	* Netflix OSS （が公開されている）とSpring Bootと組み合わせて、便利に（アノテーションで）使う仕組み
	* Netflix、Groovyつかってる Pivotalとのコラボ
	* ☆Service Discovery（Eureka）
		* Service Locator パターン
		* 自動で負荷分散（ラウンドロビン）
	* ☆Client Side Load Balancing（Ribbon）
		* 負荷分散ルールがいくつか（ラウンドロビン、平均レスポンス時間、ランダム）
		* Fault tolerance機能
		* Spring のテンプレート　→ JDBCテンプレートが有名。SQLだけが違うようなものを吸収するラッパ
		* ☆RestTemplate
	* ☆Circuit Breaker (Hystrix)
		* Circuilt Breakerパターン（家のブレーカみたいな。過電流でブレーカが上がる）
		* 問題があったときのFallbackメソッドをアノテーションで定義
		* ↑のアノテーションを定義したメソッドのメトリクスが取れる
	* ☆Spring Cloud Connectors
		* サービスの抽象化
		* 対応は、CloudFoundry, Heroku, Local
		* Springが昔目指したもの
			* 図
			* コンテナ上に乗っかるアプリ、アプリに対して、サービスとコンテキストがDIされる
		* いま目指しているもの
			* 図
			* アプリはコンテナじゃなく、クラウドに乗っかる
			* java コマンドでアプリが立ち上がって来るべき（Nodeのイメージ）
	* ☆CloudFoundary
		* Javaなら使えるよー。Springだともっと便利に。

	* 新しいクラウド環境への拡張（CloudPlatform）
		* Paasが増えてもつかえる
		* CloudFonundaryの場合の実装例
* JSUG 2015 5/8 にイベントあるよ～

---

# F-3 GS CollectionsとJava 8：実用的で流暢なAPIで楽しい開発を！ by 伊藤 博志（ゴールドマン･サックス） #ccc_f3

スライド
http://www.goldmansachs.com/gs-collections/presentations/2015-04-11_JJUG_CCC.pdf

* Java8 本番環境使っている人　ノ　→少ない
* GS Connections はJava5から使えるよー
* ゴールドマン・サックスのエンジニアリングについて
	* 金融
	* 自社開発、決済、トレーディング、社内クラウド
	* 複雑な問題にSolutionを提供「We BUILD」
	* 全社員の1/4がテクノロジスト（8000人くらい）
* 「GS Connections = Don Raab's passion」

* 目次
	* イントロ
	* 古き良き
	* Java8のStreamAPI
	* GSConnctions
	* フレームワーク比較

## イントロ
* Donが10年以上前に開発を始めた、OSSでコレクションフレームワーク
* GS Collections Kata
	* トレーニング教材
	* 1,500人以上のGS Javaエンジニアが受講
	* ぎっはぶで公開済

## 古き良き時代
* ケント・ベックのSmalltalkのベストプラクティスパターン
* →実装パターン（ケント・ベック）　Map, List, Set しか残らなかった
* GS Collections で
	* 遅延実行（メソッドチェーンごとにイテレーションではなく、終端処理のときにまとめて）
## java 8 Stream API
* ↑は大海原（GS Collections）への入り口！

## GS Collections
* 即時実行
* 豊富なイテレーションパターン
* ☆共変型の戻り値
* 新しいコレクション型
* メモリ効率の良い、SetとMapの実装
* プリミティブ型のコンテナ、イミュータブルなコンテナ
* 並列イテレータ

### API 
* UtilityとAPIの対比
* Java8とGS Collectionsの実装比較（※要スライド）

### 流暢さ
* Java8とGS Collectionsの実装比較（※要スライド）

### メモリ効率
* Mapのメモリの比較グラフ
	* JDKのHashMapのメモリ効率がそもそも悪い
		* GS Connectionsと比べると、約半分（GSC UnifiedMap）
* Setのメモリの比較グラフ
	* JDKのHashSetのメモリ効率悪い（GCS は約1/4）

* プリミティブコレクションの比較グラフ
	* GSC IntArrayList

### メソッド参照
* KataをJava7→Java8へアップデート
* ※要スライド

## フレームワーク比較
* 比較表　※要スライド

## 付録
* 簡単な実行例
* ☆RichIterable(Java Doc）
* Kata
	* TDD型のトレーニングマテリアル
* Scalaのカンファレンスにも（GSC）登壇したりとか... 

---

# G-4 あなたとAndroid!? 今すぐダウンロード！～Android開発で変わる SIerのJava技術事情について～ by こざけ（ほげ駆動 / 株式会社 第一コンピュータリソース） #ccc_g4

ナマのこざけさんを初めて見る...

https://github.com/kozake/jjug2015/blob/master/talk.txt

* ビールｗ
* 大阪支店所属、アーキテクト。

* 質問
	* SIerのひと？　そこそこ多い
	* JJUG初参加の人？　はんぶんくらい
	* Android開発仕事したことある人？　半分くらい？

* Android Beginner からの知見
* アジェンダ
	* 事例紹介
	* ここが変わるよAndroid開発（Web系からの）


## 事例紹介

### 背景

* 300人月超えの、大規模、ミッションクリティカル案件
	* 稼働率99:99%
* 結果的には無事成功（むしろ大成功）
↓
* 200人月超えのAndroidタブレット基幹システム
	* 900人がつかうようなやつ
	* 「Javaならなんとかなるだろ」
	* チーム初のAndroid開発
* なぜAndroidか
	* ベンダロックしたくない、端末代を安くしたい
	* 技術者調達楽、Windowsで開発できる
	* バージョン差異は大変だけど、今回は固定

### システム構成

* タブレット900台、モバイルプリンタ、Oracle,Tomcat,MyBatis,NAS

## ここが変わるよAndroid開発

* ☓Eclipse　◯Android Studio使おう
* Android Studio 勉強しないといけない？
	* これだけじゃなくで勉強することいっぱいある
* 開発環境：Gradle, git bucket, Android Studio, Jenkins, Artifactory
* Gradle がキモ
	* Maven、大規模になってくると大変な感じ
	* AntとMavenの良い所どりしてる感じ
	* 『Gradle徹底入門』が良書
	* ロジック書ける
	* GroovyのSimpleTempateEngineが強力。
		* テーブル定義のXMLからDAOと、SQLのDDLが出るという例
* 何のライブラリ使うか
	* いろいろ入れよう！　→　容量が大きくなって怒られた
	* 簡単な処理なのに、汎用的なライブラリ（容量大きめ）が必要かは要検討。バッテリーの持ちも考える。
	* AndroidAnnotations
		* 定形記述の省略化
		* ↑はソースが自動生成されているのを見ればわかるよ
	* ActiveAndroid
	* Jackson
	* クライアントとサーバで共通のものを使うときは、Javaのバージョンに気をつけましょう
* ☓MyBatis Generator Plugin　◯SQLite Open Helper
	* ↑を生で使っているわけではない。ORM使ってる。
	* ORMの比較
	* ActiveAndroid
		* こまったところ
			* マスタをタブレットのDBにそのまま持って行きたい。副問い合わせの自作ライブラリ作った
			* トランザクション
			* 全然Activeじゃない（活動が）
		* データベースファイルの中身を見たい
			* パーミッションの問題
* PDF生成。Jasper Reportつかいたい　→ AWT無い
	* →PDFはサーバ側で生成することに。
	* PDF作成は鬼門
* 写真管理
	* UUID管理なら行けるはず　→　JavaのライブラリはUUIDはタイプ４だった。
		* タイプ４は完全ランダム。一意かどうかは性能による
	* UUIDはモバイルで取得するのはセキュリティ的にまずいという風潮
* HttpClient
	* Androidにはすでにある。が、HttpURLConnectionがベスト

* まとめ
	* 業務へのタブレット導入
		* 実はサーバサイドも新技術が必要だった（Java8、JAX-RS、☆Apache OLTE、☆Jersey）
		* Android開発に強い会社と組むのがいい
		* ネットワーク技術は必須

---

# M-5 プログラミング言語Clojureのニャンパスでの活用事例 by 太田 正悟 （ニャンパス株式会社） #ccc_m5

* clojureコントリビュータ
* アジェンダ
	* 言語の概要
	* 使う理由
	* 活用事例

## 概要
* 使ったことがあるひと？ノ　→少ない
* Lisp系言語
	* S式
		* Lisp系、言語の作りがシンプル
	* 言語シンプル（覚えること少ない）
	* マクロでの言語拡張
	* REPLでのインタラクティブな開発
* 関数型言語
	* 多くの値がイミュータブル
		* vector, map
	* ファーストクラスな関数
	* 遅延シーケンス（無限個をあつかえる）
* JVM言語
	* Javaオブジェクトを扱うことができる
	* 無名クラスもつかえる

## 使う理由
* アジャイル開発
	* REPL、イミュータブル、後付のポリモーフィズム
	* データの表現をMapで。
		* ？このあたりの説明、よくわからない
	* ☆マルチメソッド
		* 後付で振る舞いの追加。
* 実用性
	* JVMで動作
	* Javaとの相互運用性（ライブラリ）
	* エコシステム　☆Leiningen, ☆Clojars

* 言語の自由度
	* 実行時のプログラム変更、リロード、REPL
	* 問題領域にあった言語の定義　→　マクロ
	* 静的型チェック
	* 非同期プログラミング
	* パターンマッチ
	* 論理プロラミング

## 事例
「baasday」

* AWS 
* MBaaS 
* のべ20万インストール
* スループット 150req/sec

「Lesson Supporter」

* 個人レッスンの「先生」の支援サービス
* 簡易CMS
* 近日公開予定

* CMSのデータを、Mapとして表現
	* 着手から3日でCMSモデルと描画処理のコアが完成。
	* ページテンプレート毎のカスタム描画方法をマルチメソッドで定義できるように
* インタラクティブなAPIの確認
	* WebPayのAPIをREPLで確認

## Clojureコミュニティに向けた取り組み

* OSS
	* ニャンパスで提供しているものが幾つか
	* [clojurnal](http://clojournal.com/) （日本語情報のサイト）
	* clojureのチャットボット
* 勉強会
	* Tokyo.clj
	* Laketown.clj
	* テーマ特化勉強会
* HaLake（コワーキングスペース）
	* ハンモック駆動開発
	* Clojure割引
	* チェックインアプリ　HaLake　API（OSS公開）

## まとめ
* 日本でもちょっとずつコミュニティが活発化

* 社内、正社員は2人
* JVMコンパイルだけじゃなく、JavaScriptコンパイルもある。
* （わりと会場内にClojureユーザ多そうな感じ）

---

18:00前に退散！お疲れ様でした。
