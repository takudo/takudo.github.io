---
layout: post
title: "勉強会メモ：第十回 #渋谷java @渋谷クロスタワー12F ビズリーチ"
date: 2015-03-07 22:30:00 +0900
comments: false
tags: 勉強会 java 渋谷Java
description: 勉強会 java 渋谷Java k.hasunuma 「Date and Time APIのTemporalAdjuster活用法」 nabedge「あなたのプロジェクトで気軽にjavaをバージョンアップするために必要なこと」 emaggame 「Droolsについて」 lino 「Jenkins実践入門第二版What's New」 n_agetsu 「Tomcat実装から学ぶClassLoaderLeak」 masakkuma 「SonarQubeについて」 komiya_atsushi jflute 「Java8 stream() にススメ (わりと入門)」 kawachi「最小Hello World!チャレンジ」

---

14:00 ~

---

# さきに感想

* java再勉強中なのもあって、結構話がわかる感じがあってうれしかった
* 怖くない
* タイミング合えば今度はLTしてみたいかなぁと思った

<!--more-->

---

# お品書き
* セッション枠（20分)
	* k.hasunuma 「Date and Time APIのTemporalAdjuster活用法」
	* nabedge 「あなたのプロジェクトで気軽にjavaをバージョンアップするために必要なこと」
	* 休憩10分

* 通常枠（10分）
	* emaggame 「Droolsについて」
	* lino 「Jenkins実践入門第二版What's New」
	* n_agetsu 「Tomcat実装から学ぶClassLoaderLeak」
	* 休憩10分
	* masakkuma 「SonarQubeについて」
	* komiya_atsushi
	* jflute 「Java8 stream() にススメ (わりと入門)」
	* kawachi「最小Hello World!チャレンジ」

---

恒例自己紹介

---

# @khasunuma 「Date and Time APIのTemporalAdjuster活用法」

<iframe src="//www.slideshare.net/slideshow/embed_code/45525285" width="425" height="355" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//www.slideshare.net/btnrouge/jsr310-adjuster" title="JSR 310 - TemporalAdjuster" target="_blank">JSR 310 - TemporalAdjuster</a> </strong> from <strong><a href="//www.slideshare.net/btnrouge" target="_blank">Kenji Hasunuma</a></strong> </div>

* 佐世保の提督
* Package java.time.*
* Temporal object
	* Temporal インターフェースの実装
* Temporal Adjuster
	* たとえば今日を基準に、、、
		* firstDayOfMonth
		* lastDayOfMonth
		* next(MONDAY)
	* Strategy パターン（デザパタ）
	* ＠FunctionalInterfaceなのでラムダで置き換える
	* 使い方の話
		* LocalDate のwith()に、adjuster.firstDayOfYear()とかを渡す
		* 年初とか、いろんな例
		* ☆nextOrSame()
		* next()とprevious()
		* 成人の日を求めたいの例
			* 現在は、月の第二月曜日　←　これもdayOfWeekInMonth()とかで簡単に求められる
* Temporal Adjuster つくるの、結構大変
	* たとえば営業日とか振替休日
	* 出来合いののAdjusterはすべて日付に関するもの、時刻に対するものが必要な場合は自作する必要あり
* 営業日の計算例
	* nextBusinessDay()

* まとめ
	* よくつかうやつはすでにあるよー
	* low-level API で自作できるけど、すでにあるやつは簡単に使える


---

# nabedge 「あなたのプロジェクトが気軽にjavaをバージョンアップするために必要なこと」

<iframe src="//www.slideshare.net/slideshow/embed_code/45542440" width="425" height="355" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//www.slideshare.net/nabedge/201503-java10-java" title="渋谷java−あなたのプロジェクトで気軽にjavaをバージョンアップするために必要なこと" target="_blank">渋谷java−あなたのプロジェクトで気軽にjavaをバージョンアップするために必要なこと</a> </strong> from <strong><a href="//www.slideshare.net/nabedge" target="_blank">nabedge Watanabe</a></strong> </div>

* ビズリーチの人
* Javaのバージョンは何使ってるか？（質問）
* なぜ古いバージョンを使うのか
	* なにかあったらいやだから
		* elasticsearchのコンパイラがバグってた例
		* ↑珍しいからニュースになる
	* インフラ担当者がいやな顔する
	* がんばってjava8に上げても、、、数年後には新しいのがでてる。。。
	* そうじゃなくて、上げやすくしておくことが重要
* JDKとJRE
	* なんか全部にJDKが入っている
		* 本番では要らないよね？
		* ↑これがバージョンアップ難しい原因では？
	* JDKの切り替え簡単
		* 開発環境や、Jenkinsとか、切り替えられるように設計されている
	* JREの切り替えは？
		* マイナーバージョンアップも含めて切り替え、とか、なんかあったときの元に戻し方は？
		* JJUG去年のやつ（[スライド](http://www.slideshare.net/nabedge/java-the-twelve-factor-app)）
			* [TwelveFactorApp](http://12factor.net/)
			* 第2章「依存関係」の最後の段落
				* アプリケーションがシステムツールを必要とするならば、そのツールをアプリケーションに組み込むべきではない
				* アプリにJREを組み込む！
				* JREにアプリに組み込む！
	* $JAVA_HOME/lib/ext の配下は自分で作ったjarをおいてもいい
		* そこは自動的にクラスパスに含まれる！

* Tomcatはインストール型ではなく、ライブラリ型＝組み込み型　を使おう
* ビルド方法！（Tomcatの例）とリリース＆実行方法
	* JREの配布tarを解凍して、その中にアプリを突っ込む
* JREのバージョンup/down方法
	* Jenkinsでのビルド時に、jreのバージョンを切り替えるだけ！
* まとめ
	* JDKとJREはちゃんと区別する
	* APサーバは組み込み
	* JREもろともリリースする！

* 質問
	* ImageMagickとかCurlとかは、Docker、AMIとかでパッケージングにするってのはどうかんがえる？
		* それは考え方一つ。パッケージングをどう考えるか

---

休憩 10分

---

# emaggame 「Droolsについて」

<iframe src="//www.slideshare.net/slideshow/embed_code/45527705" width="425" height="355" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//www.slideshare.net/lanabe/shibuyajava10-drools" title="Introduction to Drools" target="_blank">Introduction to Drools</a> </strong> from <strong><a href="//www.slideshare.net/lanabe" target="_blank">Yoshimasa Tanabe</a></strong> </div>

* BRMS とは
	* ビジネスロジックをルールとしてそとに追い出す
	* DSLで書いたり
	* BRMSは↑のランタイム

* Drools
	* RedHatのOSS　BRMS実装
* デモ
	* 自動車保険の例
	* デシジョンテーブル（ルールを記載してあるスプレッドシート）
* デモ２
	* TwitterCBR
	* Twitterのサンプルストリームから条件に合致するツイートの抽出
	* ☆CEP（Complex Event Processing）
	* （.drlってのがDSL記述ファイルかな）
	* 会話を抽出　1段階目の抽出に対して2段階目の条件を設定
	* （1段階目と2段階目のは効率はどうなんだろう？）

* 所感
	* デシジョンテーブル直感的
	* DSLは覚えること多いけど、まあ表現しやすい

---

# @lino_s 「Jenkins実践入門第二版What's New」

<iframe src="//www.slideshare.net/slideshow/embed_code/45543510" width="425" height="355" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//www.slideshare.net/linoSth/jenkins-whats-new" title="Jenkins実践入門 第二版 What&#x27;s New" target="_blank">Jenkins実践入門 第二版 What&#x27;s New</a> </strong> from <strong><a href="//www.slideshare.net/linoSth" target="_blank">Masanori (聖規) Satoh (佐藤)</a></strong> </div>

* Jenkins 10万インストレーション！
* 2011/11/11 Jenkins実践入門発売
* 4刷　→　改訂の流れ
* ビルドツールの一新　AntからMavenへ。Gradleも
	* パッケージリポジトリも
* ツールの最新化
	* Selenium WebDriverへ
* 高度なビルド
	* ビルドパイプライン
	* ビルドの昇格
* その他いろいろ
* デザインかわったのでスクショ全部とりなおし、、、
* 2015年5月か6月に発売予定

---

# n_agetsu 「Tomcat実装から学ぶClassLoaderLeak」

<iframe src="//www.slideshare.net/slideshow/embed_code/45544134" width="425" height="355" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//www.slideshare.net/agetsuma/tomcat-java" title="Tomcatの実装から学ぶクラスローダリーク #渋谷Java" target="_blank">Tomcatの実装から学ぶクラスローダリーク #渋谷Java</a> </strong> from <strong><a href="//www.slideshare.net/agetsuma" target="_blank">Norito Agetsuma</a></strong> </div>

SIerさん

* ClassLoaderLeak とは
	* ホットデプロイ時に古いクラスローダがGCされない
	* Jenkinsとかでホットデプロイ時に↑に遭遇率が高い
* 主な原因
	* アンデプロイ済みAPへの参照が残ること！
* ThreadLocal.remove実行漏れ
	* アンデプロイ時にスレッドプールのチェック
* JDBCドライバの参照が残る
	* postgresのドライバの例。DriverManagerに登録される
	* JDBCドライバを.warに入れない
	* deregisterDriver実行
	* Tomcat8ではデフォルトで見地・解放が有効
	* 解析方法は？
		* HeapDumpとる
		* Eclipse Memory Analyzer
			* Duplicate classes
		* ☆path to GC Roots で原因を特定（どこに参照が残っているか）
* 伝統的なライブラリでのclassloaderleak
	* 最新版ではだいたい直ってる
* Tomcatだけじゃない
* まとめ
	* CIでの（ホットデプロイ）での顕在化
	* ヒープダンプ見ると原因わかるー

---

休憩10分

---

# masakkuma 「SonarQubeについて」

（スライド見つからなかった。。。）

* 株式会社エムティーアイ
* オフショア管理をしてる

* SonerQubeの紹介
	* OSSのソースコード静的解析ツール
* 自動化をやる
	* コストの面から、静的解析だけ。
* Javaの静的解析。CheckStyle, FindBugs, Jenkins
* オフショアの品質比較したい
* ☆SonerQube Runner
* コメントは抜いた行数をだしてくれる
* コードの重複率
* コードの複雑度
* 技術的負債
	* （どうやって判定している？）
	* 技術的負債の全体に対する割合
* 問題箇所のドリルダウン
* dashboardのカスタマイズ
* Tips
	* メモリ不足（最低1GB、DiskIOも高速である必要あり）
	* 文字コードが指定外のものがあるとこける
	* 指標は組み合わせてみる
* 見える化が大事
* 上司受けがよかった（定量的に見えるから）
* おまけで playbookあるよー

---

# komiya_atsushi「あなたと乱数生成とJava」

<div style="max-width:425px; width:100%">
<script async class="speakerdeck-embed" data-id="39188c674fdd4b6e89bb1471b752bd8e" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>
</div>

* SmartNews の人
* 広告と機械学習　をjavaで（スマートニュース）

* 乱数生成
	* はまりどころ多い
	* ☆ThreadLocalRandum
	* ☆SecureRandum
* commons-math3
	* MersenneTwister（乱数生成器）
	* Well44497a
		* 乱数の周期がものすごいでかい！
	* ISAACRandom
		* 暗号論的にセキュア？ほんとに？
	* ☆RandomDataGenerator
* 比較（性能、速度について）
	* ThreadLocalRandom、Random 並列実行
		* ThreadLocalRandomの方が性能劣化しない
	* commons-math3の比較（2億回生成）
		* ThreadLocalRandomめっちゃ速い
		* （2億回をシリアルで実行したの？平均レイテンシ？）
* どれを使うべきか
	* commons-math3は並列実行は避ける（スレッドセーフじゃないので）

---

# jflute 「Java8 stream() にススメ (わりと入門)」

* とりあえず実装を
* 会員テーブルの全件取得
* filter()の例
* map()の例
	* 今までの書き方と考え方が逆
	* （入れ物を用意してから突っ込む　→　入れ物は最後に用意するようなイメージ？）

* 今までのjavaやってた人はstreamやっぱり壁っぽい
* なにがちがうのか？
	* 「目的レベルがあがった」
		* いままでは目的を達成するための手段レベル
		* 新しいのは目的を達成するための目的レベルで書く
* Streamの考え方が重要。
	* 手続きから目的へ

---

# kawachi「最小Hello World!チャレンジ」

<iframe src="//www.slideshare.net/slideshow/embed_code/45544556" width="425" height="355" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//www.slideshare.net/TakashiKawachi/hello-world-45544556" title="最小 Hello World! チャレンジ" target="_blank">最小 Hello World! チャレンジ</a> </strong> from <strong><a href="//www.slideshare.net/TakashiKawachi" target="_blank">Takashi Kawachi</a></strong> </div>

pLucky 社

* javaは10年前くらい

* .classファイルを小さくするがお題
* まずはJavaで
	* 408 byte
* javap -c で見る
	* コンストラクタは要らないなぁ。。。。
* ☆Jasminでトライ
	* JVM byte code 用アセンブラ
	* 303 byte
* バイナリで見てみる
	* わからん
* .class ファイルの読み方
	* 「[4. The class File Format](https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-4.html)」
	* バイナリが読めるようになってきた
* ☆constant pool が9割
	* SourceFile 属性。なくても動く
	* 自クラス名　
	* 継承元クラスを変更　親クラス
* バイナリエディタで修正
	* 250 byte !
* まとめ
	* バイナリいじると普段見れないエラーが見れる
	* class読めると、javapの出力に親しみがわく

---

宣伝タイム

---

おつかれさまでしたー！