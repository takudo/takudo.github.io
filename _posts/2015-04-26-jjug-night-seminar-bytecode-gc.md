---
layout: post
title: "2015-04-24 JJUG ナイトセミナ 「Javaのプログラムはどうやって動いているの?」"
date: 2015-04-26 21:00:00 +0900
comments: false
tags: 勉強会 JJUG Java 
description: 勉強会 JJUG Java javap と仲良くなろうと思った 基本のGCのアルゴリズムが理解できた。すごくスッキリ分かってしまった 「パージェクトJava」読み終えたぐらいのレベル感（つまり僕）の人にちょうどいい感じだった USTに甘えず来てよかった マカロンは積むもの
header_img: https://qiita-image-store.s3.amazonaws.com/0/34165/cacd94df-429c-cfec-99d7-8c969f21f8dc.jpeg


---

https://jjug.doorkeeper.jp/events/23636

---

#感想

* javap と仲良くなろうと思った
* 基本のGCのアルゴリズムが理解できた。すごくスッキリ分かってしまった
* 「パージェクトJava」読み終えたぐらいのレベル感（つまり僕）の人にちょうどいい感じだった
* USTに甘えず来てよかった
* マカロンは積むもの

---

桜庭さん@skrb

* Javaのプログラムが実際どうやって動くのかというお話
* アジェンダ
	* JVMとは
	* mainを実行するまで
	* どうやって実行するか（バイトコード）
* Javaチャンピオン、日本で一人
* サイト「[Java in the Box](http://www.javainthebox.com/)」
	* Java SE
	* このプレゼンもJavaFXでつくっています！
* サイト「ごちそうフォト」
	* 最近１位になった（称号：スイーツ王）

<!--more-->

---

# 一部

<iframe src="//www.slideshare.net/slideshow/embed_code/key/use99fBkdru5bH" width="425" height="355" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//www.slideshare.net/skrb/java-jvm" title="Javaのプログラムはどうやって動いているの? JVM編" target="_blank">Javaのプログラムはどうやって動いているの? JVM編</a> </strong> from <strong><a href="//www.slideshare.net/skrb" target="_blank">Yuichi Sakuraba</a></strong> </div>

## JVM
* VirtualMachine
	* ソフトウェア仮想マシン
	* VM（Write Once Run Anywhere）
		* OSによらない。OSの差異を吸収
* Java Virtual Machine
	* javac → クラスファイル。クラスファイルの中にバイトコード
	* バイトコードを解釈して実行
	* 基本、インタープリタ（１行ずつ実行）　⇔　Cなどのコンパイル言語
	* 実行時コンパイルで速くする（JIT）
	* 今日はJITはやらない、インタープリタだけ
* 今日のサンプルコード
	* 今日のJavaのコードはこれだけ
	* main()と、add()のメソッドがあるだけ
* コンパイル：　.java → javac → .class
	* .java
		* クラス、フィールド、メソッド　定義が入ってる
	* .class
		* クラス定義、フィールド定義、バイトコード（メソッド）、定数プール（文字列、リテラル）、属性（その他雑多。Genericsの型パラメータなど。Javaのバージョンによって解釈可能・不可能がある。解釈できないと無視する）
* javap
	* バイトコードの逆アセンブル
	* オプション 
		* -p プライベートメソッドも含める
		* -v バイトコード、定数プールも含める
	* 書籍「Java仮想マシン仕様」　バイトコードの読み方
	* Wikipediaにもバイトコードのよみかた載ってる
	
## mainを実行するまで
* java コマンドを叩くと...
	* JVM起動→クラスロード→リンク（.classの準備）→初期化→main実行
	* JVM起動
		* JNI_CreateJavaVM() ←JVMを起動できる
	* クラスロード
		* Class ファイルの読みこみ、使用するものを芋づるでロード（親クラスとか、利用しているものとか）
		* さっきのサンプルコードでも、200個位クラス読み込む
		* なにが読み込まれているか確認
			* java -verbose:class Test
			* rt.jar（超巨大）←java9から分割する？
	* リンク
		* classファイルが正しいかどうかチェック（改ざん、悪意のあるコード）
		* static フィールドの初期化（※コードの実行はしない。オブジェクトはnullが入る、基本型は基本型のデフォルトが入る？）
		* クラス間の関係を解決（resolution）
		* ここで実行できる準備がととのった！
	* 初期化
		* Staticフィールドの初期化（今度はコードの実行をする。final変数とかでもココで初めて値が入る）
		* static イニシャライザの実行
	* main()
		* 実行

```
public class Hoge
    static {
        //ここ
    } 
}
```

* Java Puzzle: Anniversary
	* static final でもおかしな値が入る事がある

## バイトコードの実行

* コレクションのいろいろ
	* 1:Queue
	* 2:Deque （両側から出せる）
	* 3:Stack　（片側からしかだせない）
	* 4:Ring Buffer（終わりがない）
		* Javaにはない
	* 5:Map
	* 6:Linked List
	* Javaの実装いろいろ
* 今日のお話のメインは Stack
	* Last In Fisrt Out（LIFO）
	* 途中要素へのアクセスNG

* 実行形式
	* ☆ StackMachine
		* Java VM
		* PostScript
		* .NET Framework CLR
		* ☆ VMは StachMachine が書きやすい
	* ☆ RegisterMachine
		* メモリを事前に準備
		* ☆ IntelCore, ARM Cortex

* 逆ポーランド記法
	* カッコがなくてイイ！
	* StackMachineと相性がいい
		* 中間の状態をStackにとっておく！

* ワーキングメモリ
	* スレッドごとにスタックがある（Java Stack）
	* スタック
		* PC（プログラム・カウンタ）
		* フレームを積み重ねる
		* 再帰で見たことのある、スタックオーバーフロー
		* フレームの中に更にスタックがある
			* Operand Stack（計算領域？）
			* Local Variable（ローカル変数）
			* Heapへの参照をもってる
* バイトコード（フレーム内の話）
	* load : ローカル変数からの読み出し
	* store : ローカル変数に入れる
	* フレームスタックの0番目にはthisが入る（インスタンスメソッドの実行）

* JVM編まとめ＆質問
	* OracleとEclipseのjavacの出力物はちょっとちがう
	* VMの動作はベンダーによってちょっと違う、例えばIBM（一応仕様通りには作られているはず）
	* OracleJDKとOpenJDKの関係、コードベースは同じ。Oracleの方がツールが多い、内部的にいろいろやってたり

---

# 二部 GC

<iframe src="//www.slideshare.net/slideshow/embed_code/key/Fwsvn7aTCdauQb" width="425" height="355" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//www.slideshare.net/skrb/java-gc-47402594" title="Javaのプログラムはどうやって動いているの? GC編" target="_blank">Javaのプログラムはどうやって動いているの? GC編</a> </strong> from <strong><a href="//www.slideshare.net/skrb" target="_blank">Yuichi Sakuraba</a></strong> </div>

* なぜGC？
* GC アルゴリズム
	* Mark&Sweep
	* Copy
	* 世代別

---

* GC
	* メモリを自動的に管理
	* 不要になったオブジェクトを開放
* GCがなかった頃
	* 自前でメモリ管理
		* Cとか...
	* 問題点
		* メモリ開放わすれ
		* 二重開放
		* 無効な参照
	* メモリ周りのバグ修正難しい（静的改正とか、デバッガはあるけれども...）
* 1959年に最初に作られる
	* Mark&Sweep(Lisp向け）
* 1990年代に、やっと使われてきた感じ
	* JavaのGCも遅いって言われてた
	* CPUやメモリの性能向上

* GCの分類
	* アルゴリズム
		* 基本
			* ◯ Mark&Sweep
			* 参照カウント（※Javaにはない）
			* ◯ Copy
		* 複合
			* ◯ 世代別
			* G1
	* 運用
		* Serial
		* Incremental
		* Concurrent
		* Parallel


* ↑の◯のSerialを紹介
* GCはGC本があるのでそれ読むといいよ	

## Mark & Sweep

* 使用中のオブジェクトにマーク。マークがされていないオブジェクトを掃除
	* Mark フェーズ
	* Sweep フェーズで取り除く

* どうやってMarkする？　※要図参照
	* Rootから、参照をたどっていく。芋づる式
		* ☆深さ優先探索
* Mark後、先頭からFreeListに入れる
* Mark&Sweapのあと、間に空いた部分を詰める（Compaction　コンパクション）
* いいところ
	* 実装がシンプル、参照書き換えがない（※コンパクションしない場合）、安全
* わるいところ
	* 遅い
	* プログラムを止める必要があるが、その時間が長い
	* ヒープの断片化が起きやすい（※コンパクションしない場合）
	* ↑コンパクションも重たいので、やったらやったで余計重たい

## Copy GC

* ヒープを二分割
	* 使ってる方と使ってない方　に二分
	* GC時に、まずマークして、使ってる方から使ってない方に、使ってるオブジェクトだけコピーする（コンパクションもできてる）
	* ↑をやると使ってる方・使ってない方が入れ替わる
* いいところ
	* 速い
	* ヒープの断片化がおきない
	* 高速なアロケーション（先頭から詰めておくから）
* わるいところ
	* ヒープの使用効率が悪い（2倍必要）
	* 参照の書き換えがある

## 世代別GC

* 仮説：若いObjほど早く死ぬ
* 世代別にヒープ管理を行う
	* 若い世代：高速なGC → Copy
	* 古い世代：安定したGC → Mark&Sweep
* Objの年齢：GCを生き延びた回数
* HotSpotVM
	* CopyGC用→Survivor 1 & Survivor 2
	* Mark&SweepGC用→Tenured（テニュアード、かな）
	* 新しいObjはEdenに。
	* GC時、Suvivor1に入れる（CopyGC）
	* 次のGC時、EdenとSuvivor1の生き残りを、Survivor2に一緒に入れる
	* 何度かGCをやって、まだ残っていたら、Tenuredにいれる
		* TenuredのGCでコンパクションが走るのがいわゆるフルGC。めっちゃ遅いので、なるべく避ける
* Mark&Sweepと、CopyGCのいいとこどり
* 領域サイズのチューニングが必要。古い世代のObjのGCをなるべく減らす
* ☆CMS,☆G1GCなどの派生GCもあり 

## まとめ

* アルゴリズムは3つ！60年代から変わらない。それらの組み合わせで新しいものが出来てきている
* 原理を知っておけば、チューニングに活かせる
* GCは友達！

---

* GCを動かさないようにすることは？
	* やり方はある。RealTimeJava（スループット高い）。
	* ☆Jロキット、G1GCでGCの上限時間を設定できる

* Groovy 、jruby
	* 実行時に型を決める

---

* Java女子部宣伝
	* よこたさん

---

おつかれーした！