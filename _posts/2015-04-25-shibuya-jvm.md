---
layout: post
title: "2015-04-18 #渋谷JVM"
date: 2015-04-25 19:00:00 +0900
comments: false
tags: 勉強会 JVM Scala Java Groovy Clojure 
description: 勉強会 Scala Java Groovy Clojure まだ読んでないので「ハッカーと画家」読もう  Scalaは自虐っぽい感じがウケます  ただやっぱりコンパイル速度のところは他の言語の話を聞くと尚目立つ感じが  Clojureは先日のJJUG CCC で初めて話を聞いた  要はLispなんすね  すべてが immutable !  「Excel界隈」という単語の闇  Groovy ちゃんと話を聞いたのは初めて  結構使いやすそう  Spockすごい見た目綺麗  日本語DSLの例、おもしろい  Java   生きしださん初めて見てありがたや  言語のアップデートと、その歴史背景（会社とかお金とか）を含めての解説（大人の事情ｗ）は結構面白かった
header_img: https://connpass-tokyo.s3.amazonaws.com/thumbs/95/90/95903cdc9fbd8eb333de45747b052257.png

---

<div class="img">
    <img src="https://qiita-image-store.s3.amazonaws.com/0/34165/29f7f0c5-fc66-0809-f2cc-12138f35c509.jpeg">
</div>

http://d-cube.connpass.com/event/13257/

---

13:00〜

# タイムテーブル

* 12:30-13:00 受付
* 13:00-13:15 前説
* 13:15-14:00 Scalaセッション
* 14:00-14:45 Clojureセッション
* 14:45-15:00 休憩
* 15:00-15:45 Groovyセッション
* 15:45-16:30 Javaセッション
* 16:30-16:45 休憩
* 16:45-17:15 パネルディスカッション
* 17:15-20:00 懇親会

---

# 先に感想

* まだ読んでないので「ハッカーと画家」読もう
* Scalaは自虐っぽい感じがウケます
    * ただやっぱりコンパイル速度のところは他の言語の話を聞くと尚目立つ感じが
* Clojureは先日のJJUG CCC で初めて話を聞いた
    * 要はLispなんすね
    * すべてが immutable !
    * 「Excel界隈」という単語の闇
* Groovy ちゃんと話を聞いたのは初めて
    * 結構使いやすそう
    * Spockすごい見た目綺麗
    * 日本語DSLの例、おもしろい
* Java 
    * 生きしださん初めて見てありがたや
    * 言語のアップデートと、その歴史背景（会社とかお金とか）を含めての解説（大人の事情ｗ）は結構面白かった

<!--more-->

---

# 竹添さん　Scala 

[こちら](http://shigemk2.hatenablog.com/entry/shibuyajvm.scala)の @shigemk2 の記事もどうぞ

* 自己紹介
    * ビズリーチ １年経った
    * Perl → Java → Scala

* 「なぜScalaを使っているか」

* 10年以上前
* 2002年、関数型言語初めて触った「common lisp」
* Paul Graham「ハッカーと画家」
    * 「普通のやつらの上を行け」
    * Lispを使って素早く開発できた。（競合にたいしての）素早さが最大の武器だった
    * Lispをやればお金持ちになれる！
    * 「Ansi Common Lisp」を読んだ！
        * 得たものは、関数型言語、マクロ
    * １年やってみたけど、Lispではお金持ちになれなそうなことに気づくｗ
* 2006年、Haskellちょっとはやった
* Haskell
    * 純粋関数型、モナド、コンパイル通ればバグない...？
    * 仕事では使えない（人類がみなニュータイプにならなければ。。。）
* 2010年、Clojure
    * マークしてなかったけど、この頃知る
    * JVM上で動くLisp系言語　ついにLispの時代きた？
    * Lispなのに左から右へ評価される！（書き方をする事ができる）（Lispはカッコの一番内側から評価される）　
    * カッコも少なめ
    * Java資産もつかえる
* 2011年、Scala
    * オブジェクト指向＋関数型
    * LL系の簡潔な記述、静的型付の安全性
    * 手続き型でも書ける
    * JVM。Java資産使える（当時はJava系のSIer。Java資産が使えることは嬉しい）
    * 小田好先生
        * 「 __Scalaは学術的じゃなく実用的にしたい__ 」
* Haskell VS Scala
    * 同じことをするコード比較
        * Scalaのがなんとなく読みやすい...？
    * 関数型にこだわりすぎない！
        * 「必要なら状態や副作用を」
        * 「メソッドの外から参照透過であれば、メソッド内では状態を持って良い」
        * Scala界隈のいろんな人
            * 超関数型
            * BetterJavaな人
            * ↑聖戦ｗ、論争が起きがちｗ
    * １つの式にこだわりすぎない（やりすぎメソッドチェーン）
        * わかりやすい名前をつける
        * 言語の作者が明言している価値観で↑が後押しされているように感じられる
    * Scala Days 2015 「Hascalator」ｗ
        * IO、例外、副作用のより良い扱い方は？
        * モナドは素晴らしいけど、よりScalaらしい方法は？
    * (Scalaの) __バランス感覚__ が個人的にはいい
    * Java資産も生かせるし〜
* 紆余曲折あったけど、これなら仕事でつかえるかも！！！
* 当時...
    * 少人数でやりたかった。生産性を高め利益率を向上（大規模とは距離を撮っていた）。アジャイル。Javaの生産性に限界を感じていた。
    * ↑にScalaがハマった！
* 実戦投入
    * テストツールづくり
    * 実践的な書籍がなかったので本を書いた「Scala逆引きレシピ」
        * 改訂したいけど、まだちょっと出せないｗ
* つらいことも多かった
    * コンパイル、IDE猛烈に遅い、Play2（1→2の際、Scalaで書き直されたときのデグレ）が猛烈にバグるｗ
    * ビルドツール SBT
        * 全然シンプルじゃねえしｗ
        * ScalaBuildToolに改名ｗ
* でも、当時のJavaに失われていた熱気があった！
* マクロもあるよ
* GitBucketつくってる
    * 当時SIerにいた時に、javaコマンドで簡単に起動できるオールインワンのやつが欲しかった
    * Scalaのメリットを活用
        * ScalaベースのFW、既存のJava技術も
        * 少人数で
* 2014年〜　Scalaで大規模（数十人くらい）で開発
* Scalaの状況
    * Play2も安定
    * Akka,ApacheSpark
* Scalaのこれから
    * コンパイル速度、バイナリ互換、実行環境（Java）
    * バイナリ互換
        * いや、Javaのバイナリ互換のがおかしい（やり過ぎ）ｗ
    * 小田好先生、新コンパイラを作ってる。
        * TASTYという中間ファイルを生成→バイトコード・JSを生成する
        * [ScalaJS](http://tototoshi.github.io/slides/tenka1altjs-scalajs/)　わりと本気ｗ 
        * ほんとにコンパイル速くなんのか...？（余計遅くなるんじゃｗ）
    * 言語仕様の整理
        * 過去のいけてない機能、シンタックスを廃止、別の手段を提供
        * よりシンプルなシンタックスシュガー
    * OOPと関数型、静的型付け→よりシンプルで直感的な記述に
* 「Scala逆引きレシピ」あと一歩なので買ってね！ｗ

---

# 川島さん　Clojure 

<iframe src="//www.slideshare.net/slideshow/embed_code/key/eTBx47wrwjkW3" width="425" height="355" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//www.slideshare.net/kawasima/shibuya-jvm-1clojure" title="渋谷JVM#1 Immutable時代のプログラミング言語 Clojure" target="_blank">渋谷JVM#1 Immutable時代のプログラミング言語 Clojure</a> </strong> from <strong><a href="//www.slideshare.net/kawasima" target="_blank">Yoshitka Kawashima</a></strong> </div>

* Clojure歴２年（子どもが生まれたのと同じくらいから始めたｗ）

* 特徴
    * Lisp
    * 関数
    * プラットフォームと共生（JVM, .NET, Javascript）
        * プラットフォームのGC、例外機構の利用
    * Concurrencyのためにデザイン
* Lispがカッコがおおい？
    * Javaのほうがおおいやんけｗ
* コンセプチュアルなところがおもしろい
* ２大カリスマがいる
    * Rich Hickey
        * この人が言ってることがかっこいい
        * 「Simple made easy」
            *  Simple⇔対義語はComplex
                * 「ひとつの〜〜」
            * Easy⇔対義語はHard
                * すぐに実現できる、馴染みがあること
* ComplexとSimpleの比較
    * 複雑さのもとは組み合わせ
    * Lispは文法が変わらない
    * シンプルさのツールボックス
* Abstraction
    * 「１０のデータ構造に１０の関数よりは、１のデータ構造に１００の関数」という思想
* Seq
* Concurrency
    * 「７つの言語〜」本の並列モデルの本（現在は洋書のみ）
        * ７つのうちの５つがClojure
* すべてがImuutable
    * コレクションの破壊的操作は不可
    * コレクションに対しての変更操作、変更が無ければ共有される（コレクション全体がコピーされているわけではない）
        * 余談：Scalaもいっしょだよー
* 実用的さを考えrefsの仕組みが
* Epochal time model
    * 過去の履歴（オブジェクト）を積み重ねていく
* ☆Identity, State, Value
* ☆STM
    * 複数のrefsを一貫性をもって更新したい
    * 共有オブジェクトのロックをしない、先勝ち
    * 仕組み的に性能が出にくい
* core.async
    * ☆CSP
    * ☆Channel　？キューみたいなもん？
    * thread-based
        * １つの処理は１つのスレッドで
        * ブロックがある。
    * ☆coroutine-based
        * ブロック時点でpark状態に、スレッドをプールに戻す
        * 少ないスレッドで並列実行ができる
        * goマクロ。go-langからとってきたからgoって書くｗ
    * ClojureScriptでも、Clojureと同じように動く
    * JSはシングルスレッドなので、goのみ対応
* マクロすげえｗ
* Clojureのはじめかた
    * leiningen(Mavenみたいな)
    * IDEはLighttable(Clojureコードが動的に評価される), Emacs + cider（こっち派のがおおい）
    * self-studyは「4Clojure」で。問題の穴埋めみたいな
    * Web開発
        * Rubyとおなじようなアーキテクチャ
        * WEBサーバ、規格、フレームワーク、テンプレート
    * ClojureScript
        * Goolgle Clojure Compiler(GCC w)をつかってJS変換
        * 事例：CircleCI、MailOnline、eBay
        * Grunt/Gulp（ミニファイ、ライブリロード） leiningenに統合されている
        * Reactから利用（JSX不要）
            * ☆om（ライブラリ）
    * Excel開発
        * Excel方眼紙ｗ
        * SIerでもつかえる！
        * 「日本向けのグリッドスタイル」ｗ
        * エビデンスソリューションｗ
    * ジョブ開発
    * Lisp面が取り上げられがちだけど
* 唱和
    * 「あなたと　Clojure　今すぐ　イミュータブル」

## 質問
* core.async coroutine先のブロック処理
    * そこが気になるなら非同期処理をするべき
* python、Luaのジェネレータも書ける？
    * clojureは言語機能としてマクロある。ジェネレータとして作る必要はないかと
* clojureのまえは？きっかけとか
    * perl → javaみたいな
    * ポール・グレアムに騙されたクチｗ
    * Lispって観点で惹かれたわけではなく、コンセプトに惹かれてる。
    * 作りたいものがあって、そこにどの言語を使うかッて感じ

---

休憩

---

# 今さら始めようGroovy 上原さん @uehaj

<iframe src="//www.slideshare.net/slideshow/embed_code/key/yelRI7BwuMhEQw" width="425" height="355" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//www.slideshare.net/uehaj/shibuya-jvm-groovy-20150418" title="Shibuya JVM Groovy 20150418" target="_blank">Shibuya JVM Groovy 20150418</a> </strong> from <strong><a href="//www.slideshare.net/uehaj" target="_blank">Junji Uehara</a></strong> </div>

* 自己紹介
    * JGGUG 
    * Grails推進室
    * 「Grails徹底入門」
    * ブログ「Grな日々」
    * Elm, Rust 最近は推してる
    * TAPL本　のRust実装とか
* アジェンダ
    * どんな言語
    * 思想
    * 役立つところ
    * いまどきのGroovy

## どんな言語

* Javaの上位互換
    * 真のクロージャを持つ。簡潔な記述！（ボイラープレートを避ける）
* 2.0〜静的型チェック、静的コンパイルも選べるように
* JVMバイトコードに常にコンパイルされる
* Java標準APIが基本、プラスGroovy独自のクラス、メソッドを追加

* 2003年生まれ
* 開発元会社の変遷
* 2008ごろから安定しだした
* Pivotalやめたけど、どうなる？
    * ASF傘下のインキュベータープロジェクト
* Java7との比較、
    * ワードカウント
        * 関数型な感じで書く
    * Immutable
        * EffectiveJavaから
        * @Immutableアノテーション
        * マクロみたい
* Groovyコードの実行
* GDK
    * Groovy用の拡張されたJavaAPI
    * コレクション、File、Stringなどを拡張
    * 1300メソッド拡張されてる
    * Javaをより良く
## 思想
* Javaを置き換え「ない」
    * 併用、共存、補完が存在意義
    * Java標準はGroovyにとって必要悪ではない（Javaの置き換えじゃない）
        * ラッピング
* JRuby想定ユーザタイプ別Groovy乗り換えガイド
    * rubyだとパフォーマンスたりない
        * Groovy使う　☓
    * JavaAPIもJVMも好き。Javaの文法が嫌い
        * Groovy使う　◯
    * RubyのライブラリをJVM上で使いたい
        * Groovy使う　☓
    * ↑あまり衝突しない

## 役立つところ
* DSLとJavaスクリプティング
* DSL
    * ツールから入る人が多いと思われる
    * ☆Spock（テスト）、☆Geb（Selenium2）, ☆MarkupTemplateEnbine
    * 動的型とDSL
        * 例：JSON、HTMLマークアップ
    * なんでDSL？（そもそも）
        * ドメインモデルの記述に、既存言語に縛られる
        * それもそうだけど、あるべき記述を「追求」する。よりよいドメインモデルを探求する
    * 日本語DSLの例（ブログ記事）
    * ☆リスト（モナド）内包表記の例
    * 型クラスもどき
    * LispBuilder
    * GroovyとGrailsの使われどころ
        * Gradle、Android標準のビルドシステム
        * Netflix
        * 富士山マガジンサービス、Grails
        * NewYorkTmes
        * NTTソフトウェア、GrailsをWebアプリの標準フレームワークに
* スクリプティング
    * 開発のお供に
        * groovysh（REPL）
        * スクレイピングの例

## いまどきのGroovy
* 2.3~2.4 
    * trait入った、型推論改善、Android対応、イミュータブルソート・ユニーク
* Trait
    * Scalaに近い
    * 状態が持てる
    * 動的トレイト注入（Scalaのように静的ではない）
    * Grails3（spring bootベース）で積極採用
* マークアップテンプレートエンジン
    * hamlっぽいテンプレート

## まとめ
* Javaの「ツール」としてみるのがいいかも
    * スクリプティング
    * DSL（ドメインモデル←ユビキタス言語）

## 質問

* better Java の側面、Javaの進化とGroovyとの文法の互換性は？
    * 後から出てきた機能には追随できていないところが出てきた。ラムダは無い。
    * （ラムダの例を取り上げて）上位互換にしていこうという積極性はおそらくない。（Groovyのクロージャがある）

---

# 盛り返すJava きしださん@kis

__※↓のスライドはJJUG CCCでのスライドです。ただ内容的に重複していた部分もあったので載せています__

<iframe src="//www.slideshare.net/slideshow/embed_code/key/7ZbmCYJM3qBIyu" width="425" height="355" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//www.slideshare.net/nowokay/jjug2015spr" title="だれも教えてくれないJavaの世界。 あと、ぼくが会社員になったわけ。" target="_blank">だれも教えてくれないJavaの世界。 あと、ぼくが会社員になったわけ。</a> </strong> from <strong><a href="//www.slideshare.net/nowokay" target="_blank">なおき きしだ</a></strong> </div>

* いままで寝てたｗ
* LINE Fukuoka


* LINEでやってること
    * Spring Framework JSF JPA（きしださんしか使ってないｗ）
    
* 「まだJavaで消耗してるの？」
    * ださい：記述面倒、ださい
    * Java嫌いなオレかっこいい
        * プログラミングにくわしい（気がする）
        * まあ実際Javaいけてないｗ
* なぜJavaがださくなったか
    * CCCでもはなしたやつ
    * バージョンの変遷
    * 6,7は細かい変更なので、5-8で大きな変更（この間10年間）
    * トレンドとの対応
    * 黎明→黄金→停滞→再生期（イマココ）
    * ださい理由：機能更新がされなかったから！　が一番大きい
        * （適切に）予算が投入されなかった、マネジメントされなかった

* 反撃！
    * Java8
    * オラクル（金持ち、淡々とマネジメント）
        * Sunの時は株価の影響をうけてた
        * オラクルにとってJavaはどうでも良い技術だからｗ
    * JJUG CCC参加者の伸び
        * 2013→2015で、300→650名
    * ☆TIOBE（言語ランキング）でも返り咲き
* ラムダ
    * 関数型インターフェース
    * ラムダの構文、匿名クラスのシンタックスシュガー？
        * →じゃないよ。this違う、コンパイル結果も違う
        * クラスはどこにいったのか？
    * ラムダのバイトコード
        * InvokeDynamic
    * 裏側、InvokeDynamicと、クラスが無い
    * なぜ匿名クラスじゃなかったのか
        * クラスが出来まくる
        * いちいちコンストラクタ＆オブジェクト生成
    * ☆InvokeDynamic（JavaSE7より）
        * 動的型言語のため
        * 実行時にメソッドの型を決めつつJVMの最適化もかけやすくなる
        * 仕組みとどのように使われるか（宮川さんの資料に詳しいことが載ってるｗ）
* Javaの欠点
    * 基本型と参照型の非互換性
        * StreamとIntStream、OptionalとOptionalInt
        * 基本型のために用意された関数型インターフェース
    * 記述ださい
        * Optionalとかながくね？　シンタックスシュガーを導入してしまうには、Optionalというオブジェクトを毎回使うのは実行時コストが高い（サーバサイドならまだしも、IoTとかのリソースが少ない環境のことも考える）
* そこで　☆Project Valhallla　！
    * [ブログ記事](http://d.hatena.ne.jp/nowokay/20140811)にも
    * Specialization
        * 基本型を型パラメータとして指定可能なGenerics　！
        * キャストの制約　Hoge<int> ⇔ Hoge<Integer>の相互変換不可 
        * 問題点
            * null、オーバーロードで定義済みのものとかぶった場合
    * Value Type
        * 参照ではない（基本型みたい）
        * メモリに直接配置
        * 効率がいい
        * 現在のクラスと互換性が保てれば、Optionalのシンタックスシュガーになりうる？※妄想
    * いつ？
        * java10かなぁ...（3年後）
    * ほかの言語は？
        * Scala （Don Giovanni）
        
* まとめ
    * そろそろJava見なおしていい
    * 新しいものが入っていけば、Javaも注目されてくのでは
    

##質問タイム

* valhalla対応のJDKで any T 試せるよー

---

# パネルディスカッション

ファシリテーター：久保さん（jflute）

S：Scala 竹添さん

C：Clojure 川島さん

G：Groovy 上原さん

J：Java きしださん

## JVMでよかったところ、JVMじゃなかったらつらかったところ

* G：最近Rust（C）をさわっている。Javaの良さはGC。参照。Cと比べるとすごくシンプル
* J：LINE Fukuoka に就職できたことｗ
    * Java停滞期にはいろいろやったけど、Javaにもどってきた。まだ遅れがあるのでどうにかしてほしい
* S：既存のライブラリ、ミドルがつかえてよかった（gitbucketつくってるときJGit, sshライブラリ）
* C：Rubyの時期あった。Redmineのプラグイン。Rubyはマイナーバージョンアップでも動かなくなる。gemでハマる若者（同様にnodeのnpmでハマる若者）。Mavenの安定感が良い

## JVMでつらいなと思ったこと
* G:安定性と裏腹な部分として、進化が保守的
* J:ネイティブとの接続が弱い、Javaで作り直しがどうしてもある（Javaの方が速かったり）　
* S：LongTermSupport を求められる文化。Scalaのimplicit conversionが色んな所に波及する。JVMバイトコードのことを知ってないとっていう変な状況
* C：起動の遅さ


## 他の言語について、いつどこで知ったか

* C：ScalaとGroovyは触ったこと無い。待ち時間がある言語はちょっとｗ
* S：待ち時間のいい側面。年をとるとコーディング速度が落ちてくるけど、Scalaの待ち時間がいい感じ。Groovyはちゃんと触ったこと無い。Clojureはやったことが。
* J：Clojureは触ったこと無い、存在となんとなく。Scalaは2007くらい〜
* G:Javaは1.0〜。Clojureはカワイさんの本で。Scalaは記憶があいまいだけど、当時Groovyをやってたので食わず嫌い感が。Haskellやってた

## 他の言語に負けない部分

* C:イミュータブル（選択の余地がない）
* S：コンパイルの長さｗ　ちゃんと考えて書くくせがつくので教育上いいのではｗ　無理に関数型を押し付けない
* J：継続性。３０年後に仕事がありそうなものは、JavaとCくらい？
* G：拡張子の長さｗ　最高だぞってノリじゃない。Javaが刀だとすると、Groovyは脇差し。小回りが利く、サブディスプレイのような

## 質問タイム

* 定年超えてから経ってるけどいつまでプログラマやる？
    * C：死ぬ直前まで！夢は息子とペアプロしたい
    * S：ずっとやりたい。他の仕事できないｗ
    * J：会社にいれるかぎりは
    * G：子育てとかであまり時間がとれなくなった。定年退職した後にやりたいことがたまってる。言語の新理論には追いついてなきゃなあと思っている。自分の言語作りたいのは夢。Rustも処理系と勉強。子ども5歳にScratchJrあげたら、「もう大っ嫌い」と言われた。
    * J:こどもは10歳くらいからがいいらしい。抽象度の高いことを考えられる転換期がそのへん。それまでは単純作業が好きらしい
* コンパイラがいけてないから、コンパイラを書きなおしたりする？
    * J：持ち駒が足りない感。処理系をいじらないといけない必要に駆られない。自分の処理系はでもつくってみたい
    * S：自分で頑張る気はない。マクロが使える言語は言語を拡張できる感じ
    * C：あまり興味はない。ClojureScriptは謎のコンパイラ。モードが複数あって、動かないやつとかある。

* java9, java10 新しい機能の取り込みはJVM系言語にとっていいこと？
    * C：JVMに密接に結びつくわけではないので。JVMなくなるんじゃないかと思ってる。
    * S：Javaがよくなるのは基本的にはいいことで、将来的なところ、ScalaはJVMだけじゃなくJavaScriptもあるので、離れていくんじゃないかなあ
    * J：Javaがよくなると、ベターJavaがつらくなる、Javaでいいじゃんとなってくるのでは。シェアの喰い合い的には他の言語つらい。パイ全体自体が増えてる感じはする。割合減っても、ニーズは増えるかも
    * G：ベターJavaの側面があるので、喰われてく感じはある。でも別にそれは悪いことではない。GroovyのすべてがJavaを上書きするわけではないと思う。GroovyのインタプリタがJDKに含まれるとか、ありじゃないかなぁ

* 4人で一緒のチームでやるんだったら、なんの言語つかう？
    * J：新しく言語つくる
    * S：そもそも不可能だと。Clojureやってた。仕事で使うところまでもっていけなかった。
    * C：人生の残りClojureしか書かない。
    
---

おつかれーした！
