---
layout: post
title: 実践でのScala 勉強会に行ってきた @神宮　SmartNewsオフィス
date: 2015-02-24 14:00:00 +0900
comments: false
tags: Scala 勉強会 実践でのScala
description: Scala系の勉強会は大きめのはしょっちゅう出ている感じがする（去年はScalaMatsuriも行った）ので、意外と事例系は聞き慣れてきたなぁという感じ「コンパイルが遅い」は常套句 竹添さんの、サーバは固く、フロントは柔らかく、が名言でした。

---

[これ](https://jissenscala.doorkeeper.jp/events/19660)に参加してきた

「実戦での Scala 〜 6つの事例から知る Scala の勘所〜 2015-02-21（土）14:00 - 20:00」

* 会社の同僚と一緒に参加！
* SmartNewsさんとNulab共同
* Nulab（そめださん）が最初案内

---

# 最初にまとめ

* Scala系の勉強会は大きめのはしょっちゅう出ている感じがする（去年はScalaMatsuriも行った）ので、意外と事例系は聞き慣れてきたなぁという感じ
* 「コンパイルが遅い」は常套句
* 竹添さんの、サーバは固く、フロントは柔らかく、が名言でした。


<!--more-->

---

# 14:00-14:30	ビズリーチの新サービスをScalaで作ってみた ～マイクロサービスの裏側 株式会社ビズリーチ 竹添さん (@takezoen)

<iframe src="//www.slideshare.net/slideshow/embed_code/44962449" width="425" height="355" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//www.slideshare.net/takezoe/scala-jissenscala-44962449" title="ビズリーチの新サービスをScalaで作ってみた 〜マイクロサービスの裏側 #jissenscala" target="_blank">ビズリーチの新サービスをScalaで作ってみた 〜マイクロサービスの裏側 #jissenscala</a> </strong> from <strong><a href="//www.slideshare.net/takezoe" target="_blank">takezoe</a></strong> </div>

新サービスを作ってる、すでに世に出てるサービス
転職サービスがメイン
CodeBreakはバグあるよ！（Githubおすすめ）

* 「求人検索エンジン　スタンバイ」
	* ネイティブもある

* マイクロサービスアーキテクチャ、ApacheSpark、Scala開発の課題　を今日話す

## マイクロサービス

* 疎結合な小さなサービスの集合体としてシステムを構築する（マイクロサービス）
	* 各サービスはプロトコル（HTTP、Messaging）で連携
	* 各サービスは分かれているので別々にメンテ可能
	* クリティカルじゃない末端サービスが落ちても、サービス継続可能
* なぜScala？
	* 並列処理、（メッセージングをHTTPでやる。シリアルでやると遅くなるよ）
	* ↑のミドルウェア、ライブラリ揃ってる
	* 巨大なシステムでの静的型付の安全性
* アーキテクチャの図
	* PCWebやスマホなどのクライアント⇔FrontAPI⇔BackendAPI
* フロントエンドだけのデプロイ（サーバサイドの作業は増やしたくないので↑の分業）
	* フロントエンドの画面のすぐやる系の修正とかは、型つき言語だと厳しい
* サーバサイドはScalaで固く、フロントエンドは柔軟に
* ☆play2-stub, play2-handlebars（つくったプラグイン）
	* stubは開発中のJSON返す
	* フロントエンドだけの開発作業ができるような感じ
* ↑現実問題厳しかった
	* handlebarsの自由度が低い　ヘルパーを大量に作る必要がある　←ヘルパーエンジニアが必要になっちゃった！　
		* シンプルでよかったんだけど、もうちょっと高機能にしたかった。
	* マイクロサービスなのに密結合になってきたところがあった。
		* 大量データを扱うようなバッチ系の課題
* 複数WEBAPIの呼び出し
	* ブロッキング、JSONシリアライズ・デシリアライズもバカにならない。
		* ↑並列でやる
	* Futureでカンタンに書ける
	* PlayのWS APIも
	* 間違ったコード例
* やってみて思ったこと（並列化）
	* 短期的にデメリット多い　オーバーヘッド、サービスごとの冗長化によるコスト増　
	* デバッグが難しい
	* スキル必要、難易度が高い
	* 各サービスをつなげると、どっかでおかしくなる、原因の特定が難しい
		* （？？？そこのあたりの解決はなにか策ある？）
	* サービスが大きくなるタイミングはサービスとしては攻め時。そのあたりは経営判断だけど、どういう形がとれるか。

## ApacheSpark
akka, spark, elasticsearch
（クローリング（何秒かウェイト）、インデクシング）
* spark（高速バッチ処理）
* elastichadoop-spark で、Elasticserch⇔Sparkでデータのやり取り可能に
* [Storm](https://storm.apache.org/)は固有の書き方意識して使うけど、Sparkは学習コスト低いと思う
* Sparkで、Elasticsearchのインデックスを結合できる（複数カラム）
* 複数インデックスの更新
* 注意点
	* IOが多いとCPUを使い切れない
	* Elasticsearchのシャードの並列度も関係

## 一番の課題
* 人がいない
* Scalaの教育・採用に取り組んでいく
* 毎日３０分Scala勉強会

## まとめ
* マイクロサービスはいいことばかりではない
* Future大事（非同期）
* ApacheSparkは使いこなすの難しい
* Scalaプログラマの教育、採用が一番

* ☆勉強会の会場提供やってるよー

## 質問タイム

* 最初からマイクロサービスにするべきかどうか？
	* サービスによるかなー
	* エンジニアだけでやるなら、最初からマイクロサービスかなぁ
		* どっちかというとビジネス上の制約があるので、そっちに合わせるケースがおおい

* Akkaでクローラ　アルゴリズムで工夫
	* サイトごとにクロールのウェイトを調整、時間帯をずらす
	* ブロックされてもいいように、IPアドレスを使い捨てにできるようなプロキシ


---
# 14:30-15:00	Scala が支える医療系ウェブサービス エムスリー株式会社 瀬良さん (@seratch_ja)

<iframe src="//www.slideshare.net/slideshow/embed_code/44952229" width="425" height="355" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//www.slideshare.net/seratch/scala-jissenscala" title="Scala が支える医療系ウェブサービス #jissenscala" target="_blank">Scala が支える医療系ウェブサービス #jissenscala</a> </strong> from <strong><a href="//www.slideshare.net/seratch" target="_blank">Kazuhiro Sera</a></strong> </div>

* アジェンダ
* 2009~　エムスリー
* 基板開発チーム
	* 社内共通ライブラリの管理

* エムスリー
	* ポータルサイト運営
	* 医療系の転職
	* AskDoctors　（月に３回回答が得られる）
	* サービス要件の特徴
		* 医療現場に近いので、安定性第一
		* 創業時からのDB（１５年分）
	* チームの特徴
		* 事業別に１０チームぐらいに別れる
		* オンプレミスが中心。インフラチームがオンプレとAWSをすべて見る
		* 中途が中心
		* 内製の比率が高い（外部に依存しない感じ）
		* Javaの人が多いが、ruby,scalaも全部出来る人も
* 外国人採用の取り組み
	* StackoverflowsやGithubでの活動でアピール
* アプリ開発技術の変遷
	* ☆マトリクス図

* エムスリー.comのリリース（Scala部分が増えた）
	* デザインの一新
	* ☆過去と現在の画面比較と、現在の説明（ポータルサイト、各小カテゴリが独立したサービス）
	* お医者さん版のStackOverflow

	* ☆構成図
		* Play,Rails,Skinny,Spring　[Octoparts](http://m3dev.github.io/octoparts/)
* 導入事例
	* Play
		* 去年から2系を大幅に導入
		* Javaのシステムリプレースとか
		* [Swagger](http://swagger.io/)連携
		* ライブラリ・運用のしかた
	* Octoparts
		* バックエンド呼び出しの並列化だけでなく、キャッシュ、障害検知、切り離し
		* [Hystrix](https://github.com/Netflix/Hystrix)
			* Java製OSS
			* Octopartsがラッピングしている
		* Swagger-ui
		* Hystrixダッシュボード
			* スレッドプールの状況が可視化、負荷状況がある程度見える
		* ES,Kibana。Fluentd経由。本番環境のリアルタイムな状況
	* Skinny
		* 採用増えている（社内で）
		* warにできる（Scalatraのラッパー）
		* アンケートサービス（Angularjs,ServletFilter組み込み）
		* データトラッキング集計　管理画面
		* リアルタイム判定APIサービス（ログイン攻撃検知。ブルートフォース、リストなど）　Futureでやっている

	* PlayとSkinnyはどう使いわける。
		* Playの不便なところ
			* Railsっぽく使いたい
			* 会社として技術選択は、開発チームによる主体的判断を（モチベーションも大事）
	* JenkinsCI
		* [sbt-scoverage](https://github.com/scoverage/sbt-scoverage)　でカバレッジ計測
			* 結果が変わることがあるので、テスト実行と、カバレッジ実行を別のジョブに分ける！
	* [Zipkin](https://github.com/twitter/zipkin)
		* 遅い奴やつのmethodをスタックトレースでどこで時間がかかっているかなど出せる
		* （これ超便利そう）
	* TypesafeSubscription
		* 開発チームに直接質問
	* ApacheSpark
		* 試すのはカンタン。
		* ガチで使うのはもちろん知識必要
* 現場を見ての所感
	* なるべくシンプルに書こう（魔術っぽくしない）
* まとめ
	* 適材適所でScalaを使おう（サーバは固く、フロントは柔らかく）

---

# 15:00-15:30	Java ラブなヌーラボにおける Scala + Playframework 体験記 株式会社ヌーラボ 吉澤さん (@ussy00)

<script async class="speakerdeck-embed" data-id="5f17cbef16c5431fa1c6bc4dc0eb529f" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>

* nulab福岡にある
	* 東京は渋谷にあるよ

* （Scalamatsuriでアンカンファレンスで話した人だった）
* Typetalk
	* Java→Java Seasar2→Scala Play2 → Java8 Spring →　Go
	* NulabStatus、Go（ノリで作った）

## Seasar2との決別（Scala採用）

* Typetalkのプロトタイプを趣味ではじめた　2012年
	* Seasar2はメンテナンスモードだった
	* ほかのフレームワーク探し
* フレームワーク探し
	* rails
		* 静的型付けになれきっていた
		* 自分でメンテし続ける自信ないなぁ。。。
	* Spring（MVC、[JAX-RS](http://d.hatena.ne.jp/shin/20141201/p1)）
		* 置き換えには十分だけど、物足りない感じ。。。
	* Play
		* 1系はオラオラ系だった　Servletなんて知るかという姿勢
		* 2.0 正式リリース直前

* Playの良かったところ
	* URL（routes、controller）
	* HotReloading（開発しやすい！！！）
	* [Seasar2](http://s2container.seasar.org/2.4/ja/index.html)同様、Classloaderの差し替えをしている
	* Type-safe　View
		* 好評（さっきの発表と違うけど）
		* 型構造を変更したらエラーになる安心感
		* ☆メールテンプレートにも
		* でもHTMLコーダにとってはつらいんだろうなー。。。
	* Type-safe Reverse Routing
		* リバースルーティングのコンパイルエラー素敵
	* DatabaseMigration
		* アップグレードとダウングレード
		* ↑いちいちコメントしていたのがなくなって、楽
		* ダウングレードもあるので、フィーチャーブランチと相性がいい。
		* フェイル名は連番で管理で、マージ時にコンフリクト起こせる！（良いこと）

* 大変だったこと
	* 互換性問題
		* 変更の後方互換性無いときがある
			* リードコミッター（[JamesRoperさん](https://github.com/jroper)）になっていい方向になっている
		* 対応策：マイナーバージョンは、とにかく追随！
	* build front-end
		* sbt-web（grunt みたいなやつ）
		* gruntとかと比べると満足いかない。。。
		* 対策：Gulpに全面移行
		* gulpを、PlayRunHookで起動時に連携できる
* Scalaをつかってみてよかったところ
	* 興味　☆スライドキーワードの図
		* Optionでぬるぽなくなった
		* ☆　CaseClass　メソッドを生成、コピー機能、PatternMatch適用
		* ☆　PatternMatch
	* 実行時例外が発生するケース
		* Javaだとぬるぽがおおおい
		* Scalaだと、Option.get、List.headを意図的に呼びだしちゃうとある
	* ☆Self annotation
		* 認証方法をWebとAPIで、Traitで切り替えるという例
	* Scalaライブラリの需要
		* OAuthプロバイダ（nulabが公開）
			* OSSにした
			* 英語で公開sて良かった（英語圏のひとにも使ってもらえる）
			* AwesomeScalaにも掲載
* 運用 munin/mackerel
	* Javaの運用資産をそのまま利用
		* ☆Java8のヒープモニタリングのスライド（nulab）
* バイナリ互換性問題
	* メジャーバージョンが変わるとバイナリ互換性がなくなる
* コンパイル速度問題
	* （scaladocを出さなかったりとか）
	* 良いマシンの速度差（clean compile）
* Scalaって難しそう
	* better javaからやってみる
	* コードレビューで指摘

##まとめ

* play-scala 手軽な開発環境
* Javaよりも堅牢なプログラミング

---

15:30-16:00	休憩

---

# 16:00-16:30	2社でのScala開発経験にもとづいてそれぞれの事例を比較してみる 株式会社はてな 粕谷さん(@daiksy)

http://daiksy.hatenablog.jp/entry/2015/02/22/231144

* 京都拠点
* 先日の大阪の専門学校へのアドバイス、業界動向
	* 学生さん、個人的にScalaやってるひとがいた
		* mapが好きですが、何が好きですか？（学生さん）
			* for好きって答えた
* ２社で、Scalaをやったひとは少ないのでは。どんな違いがあるんだろう？
* 今Mackerel開発してる。
	* Freakoutとか使ってる

##Scalaプロジェクトの会社ごとのちがい

* モバイルゲーム開発　→　はてな　の変遷
* 2012年２月
	* play2.0-RC1 ,scala2.9, Squeryl
* 2013年4月
	* ネイティブアプリ移植の案件
	* 認証サーバ　play2.0.x, scala2.9.x, ScalaQuery（現Slick）
* 2013年9月
	* モバイルゲームリニューアル
	* Play2.1.x, Scala2.10.x(StringInterporation使えるようになって楽に！),mapperdao（また変えたｗ）
* 2014年１１月
	* Mackerel
	* Play2.3.x、Scala2.10.x、Slick

##コードについての所感（いままでの

* 文化の違いが出る部分
* [「ScalaStyleGuide」](https://github.com/vpatryshev/ScalaKittens/blob/master/ScalaStyleGuide.md)など参考に

* 前職
	* Javaプログラマ多数
	* Play-RC1からチャレンジ
	* コップ本の読書会、ペアプロ
	* まとめ：BetterJava　→　少しずつ関数型プログラミングへ。書かれたコードが時期によって書かれ方が結構違う。Javaっぽいところ、関数型っぽいところ

* はてな（現職）
	* Perlプログラマ多数
	* リードエンジニアはHaskellが好き
		* 関数型プログラミングの知見
	* まとめ：最初から関数型っぽい。全体的にコードの雰囲気が統一感有り


* コードの例（better javaと、関数型っぽい）
	* BetterJava　match 式が多い
	* 関数型っぽい　for式が使われている
* 例外の扱い方　ログインしているユーザのOption例
	* Exceptionを丁寧に定義
	* 余談：LL系言語からScala以降の事例も増えてるよね〜

* テンプレートエンジン
	* Velocity（前職）、twirl（現職）
	* twirl　デザイナーつらい。型安全
		* ブランチを切り替えながら、書くのは結構つらい。。。ビュー周りはやっぱり柔らかいほうがいいのかなぁ
	* ☆velocity
		* java製、動的。コンパイル不要。デザイナさんが楽。動かさないとわからない（コンパイルしないから）


## まとめ

* 思いの外ちがいがない。。。？
	* デブサミのCAのScalaの例。自分が経験したこととほとんど同じことを経験しているなぁ。
	* GitHub,JenkinsCI、フレームワークも一緒だしなぁ
* 会社をまたがって、Scalaをやるのは実はカンタン？

---
# 16:30-17:00	とあるScala伝道師の過去と現在 ChatWork株式会社 加藤さん (@j5ik2o)

* dwango →　gree → chatwork、DDD
* 昔話と、Chatworkが何をやっているか？

## 昔話

* d社　スマホ向けAPIサーバの開発でScalaを初導入。
	* DDD、Scrum
	* OCaml経験者がいた
	* 勉強会の開催なし、コードレビューを頻繁に
* g社　
	* リアルタイムチャットサービス
	* [Finagle](https://twitter.github.io/finagle/), [Trinity](https://github.com/sisioh/trinity), ScalikeJDBC
	* ほとんどのロジックFuture（Futureの罠）
	* 最初からマイクロあアーキテクチャ
	* Scala勉強会を開いた（課題あり）
		* 少人数（できるだけ）全員が課題をやる（人任せにしない）。いい悪いじゃなく、説明をする。全員が質問する。←Scalaは楽しいを実感させたい
		* 大事なことは　アルゴリズムを知っていることではない。関数型の書き方になれることが目的

## Chatwork
* 新バージョン開発
* Bizreachとの共同勉強会
    * http://c-note.chatwork.com/post/87584062960/bizreach-chatwork-scala
    * http://codebreak.com/blog/takezoe/page/a52a72
* そもそもなんで刷新？
	* scala 関数型、最初キモかったけど、なれればなんとかなるかも
	* 動的言語のままってどうなの（Python優性だったことをうけて）
* Scalaの理由
	* LLのから移行良さそう
	* AWSのJavaのSDK使いやすい
* Scalaの理由（個人的）
	* コード量、静的、抽象度が高い
	* 「宣言的にやりたいことを書きたい・書ける」
	* 同じjava.lang.stringなのに、なんで拡張メソッド呼べるのか（implicit conversion）
* Falconていうコードネームの刷新プロジェクト
	* まずはiOSのサーバサイド
	* DDD　[書籍「実践ドメイン駆動設計」](http://www.shoeisha.co.jp/book/detail/9784798131610)わかりやすいのでおすすめ
	* ☆アーキテクチャの図
		* （DynamoDBつかってる）
	* ☆sprayつかってる（DSLキモい）
		* CAのアドテクスタジオでよく使われてるよね）
		* Finagleより速い！
		* ☆play-like-adapter（DSLきついから、Playらい行くにかけるように）
	* DDDの話
		* ユビキタス言語に沿った設計にする

## 質問タイム


---
# 17:00-17:30	Scala@SmartNews - 高パフォーマンス広告配信システムを3ヶ月で作った話  スマートニュース株式会社 竹井さん(@taketon_)

* http://t.co/mOJKt0iQho

<iframe src="//www.slideshare.net/slideshow/embed_code/44953234" width="425" height="355" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//www.slideshare.net/ShigekazuTakei/jissenscala-20150221-smartnews" title="Scala@SmartNews_20150221" target="_blank">Scala@SmartNews_20150221</a> </strong> from <strong><a href="//www.slideshare.net/ShigekazuTakei" target="_blank">Shigekazu Takei</a></strong> </div>

* 脳科学研究してた
	* 会社「電脳」
	* 型安全好き
* SmartNewsのゆるキャラ（地球くん：地球のご当地キャラ）
* 広告を始めた。
	* コンテンツとしての広告

* 2014 8月　１２月のリリースに向けチーム招集
* 要件が出てきた、全部つくってね
* 動画広告サーバ２ヶ月位
	* finagle, AWS(ELB, Redshift,DynamoDB, s3), Fluentd, redis, nginx
	* 2000qpsに対して、avg.20ms
* finagle
	* Twitter RPCフレームワーク
	* Sprayも速いけど、こっちも速いよね
	* Nettyラップ
	* 選んだ理由
		* サーバが簡単に作れる
		* Nettyベース
		* Futureのハンドリング
		* ☆thriftのIDLのモデル定義
		* ☆twitter-server, Tracerによる監視
		* 自分が慣れていた。。。
	* ネガティブ
		* 更新がおそい。。。
		* 2.11対応がやっと
		* twitter.util.Future （Scalaのやつと挙動違う）
		* [NewRelic](http://newrelic.com/)
* 今日の目的：会場のみんながAdサーバがなんとなく書けるようになる
* 処理の流れ
	* ☆コード例　IOのところにFutureを。
* 「Server as a function」
* JSONParse
	* ☆コード例　JacksonStreamingAPI
* Data Load
	* ☆コード例
* Campain Filtering　（←☆広告処理の、これはなんだろう？）
* FutureChain
	* ☆コード例
* ☆コード例
	* ☆for comprehension遅いという危惧
* Filter
	* ☆コード例
	* 100msでタイムアウト
* Routing
* パフォーマンスTips
	* Future,Optionのインスタンス生成コストは気にならない（と体感）
	* （聞き取れてない。。。）

## まとめ

* 重そうなところはｍutableもつかっている
* 過度にチューニングする必要はない
* Finagle流行るといいなぁ。。

---

# 17:23 ~ Scala@SmartNews @kjim

<iframe src="//www.slideshare.net/slideshow/embed_code/44958797" width="425" height="355" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//www.slideshare.net/ssuser4227af/jissen-scala" title="Scala@SmartNews AdFrontend を Scala で書いた話" target="_blank">Scala@SmartNews AdFrontend を Scala で書いた話</a> </strong> from <strong><a href="//www.slideshare.net/ssuser4227af" target="_blank">Keiji Muraishi</a></strong> </div>

* 基本はJavaな人
* 「Ads as Content」
	* コンテンツとして評価される広告、が目標
* 欲しい機能の羅列（☆スライド）
* Scala-Play2-Slick（アプリサーバ部分）
	* ☆↑の利点をスライド
	* Slickはバージョン管理はflywayでやってる。Evolutionはつかってない。
		* ☆DDDのRepository層
	* Scala ☆on the fly
		* やっぱコンパイル速度がきになる。。。
		* tuple22 はネストで回避

## まとめ
* ☆スライドまとめ
* 業務系にむいているなー

* ☆SmartNewsのオフィススペースも提供しているよー

---

アンケート
https://docs.google.com/forms/d/1EkSNWyNm5SA5sjKxQgfU8bSBiE2RDg3D8Cp92CfO8zA/viewform?c=0&w=1

---
# 17:30-20:00	懇親会 & LT
---

# LT メニュー
* 再帰で脱Javaライク by yugolf さん
* Scala Times について (仮) by mogproject さん
* 猫について by xuwei_k さん
* 実戦OSSの始め方 by busterdayo さん
* Scala初心者がPlay/ScalaでロックなWebアプリを作ったお話 by omiend さん
* Scalaを導入した話（仮）by moc_yuto さん
* おれたちのScala年表 by benzookapi さん

※LTは抜け多いです。

---

# 再帰で脱Javaライク by yugolf さん

* TIS株市会社のひと（←どっかできいたことあるな）
* セミコロンｗ
* 末尾再帰
	* @tailrec（アノテーション）
		* 末尾再帰になっていないやつを検出
	* アキュムレーター

---

# Scala にまつわるNewsな話 by mogproject さん

* 情報収集
* ☆「ScalaTimes」ポーランドの会社が運営
	* 週に一回木曜日に発行
* ☆「FunctionalNews.com」（日本語）

---

# 猫について by xuwei_k さん

* typelevelっていうコミュニティ？
* cats scalaz のいまのところfork

---

# 実戦OSSの始め方 by busterdayo さん

* rpscala のハッカソンでつくったものを公開した
* github / [Travis CI](https://travis-ci.org/)
	* ymlで設定

---

# Scala初心者がPlay/ScalaでロックなWebアプリを作ったお話 by omiend さん

* ロックフェスのタイムテーブルがぶつかる
* ↑タイムテーブル作った
* Herokuに
* Scala辛かったこと
	* 学習コスト
	* Anorm → Slick
	* Javaライクに書けることが課題。。。

---

# Scala導入奮闘日記　by moc_yuto さん
* CyberZ
* tuple22 問題を、[HList](http://d.hatena.ne.jp/xuwei/20140715/1405352022)で回避

---

# おれたちのScala年表 by benzookapi さん

* シャノンのひと、API好き
* シャノンで最初にScala書き始めた

---

