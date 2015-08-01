---
layout: post
title: "勉強会メモ：第十二回 #渋谷java @渋谷クロスタワー12F ビズリーチ"
date: 2015-08-01 21:00:00 +0900
comments: false
tags: 勉強会 Java 渋谷Java
description: 勉強会 java 渋谷Java スライドまとめとちょっと感想です。

---

http://shibuya-java.connpass.com/event/16867/

ほぼスライドまとめです。

<!--more-->


---

14:00 ~ 

* 司会：芹沢さん　8回まで運営してた（元ビズリーチ）
	* 今rails書いてる
	* おしながきのせつめい
	
---

# @_ayato_p 「ネタじゃない ClojureScript」

<div style="max-width:500px; width:100%">
<script async class="speakerdeck-embed" data-id="22fe5d1b87134cb0868952946d3d4cbb" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>
</div>


* 今年の4月からClojure サイボウズスタートアップでClojure書いている。
* 日本だと情報少なめ？
* Clojureいろんなプラットフォーム上でうごくよー（JVM, .NET, JavaScript）（ClojureScriptはJavaScript上で動かすClojure）
* ☆CLJSJS（JSライブラリとかをClojureで呼びやすくするラッパーみたいな）
* REPLでプラウザーのDOM操作できる <（へー）
* 画面の再描画遅いけど、Live code reloading での再読み込みは画面の再描画ではない
* ClojureScript 界隈ではReactのWrapperに対する思い入れが強い（らしい）

* JSXの気持ち悪さ < JSの中にHTML出てキター！
* ☆IntelliJ + Cursive 
* ☆Boot ← ClojureScript 用のタスクランナーてきな？
* 微妙なところ
	* コンパイル時Javaがたちあがる

---

# @khasunuma 「GlassFish Internals #2 : Modular Architecture」

<iframe src="//www.slideshare.net/slideshow/embed_code/key/5pI8ickMJOU39" width="425" height="355" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//www.slideshare.net/btnrouge/glass-fish-internals-2" title="Glass fish internals #2" target="_blank">Glass fish internals #2</a> </strong> from <strong><a href="//www.slideshare.net/btnrouge" target="_blank">Kenji Hasunuma</a></strong> </div>

そーとー難しい話。

* GlassFish どういうjarで構成されているの　という話
* ☆[OSGi](http://www.atmarkit.co.jp/fjava/special/osgi/osgi_1.html)
* GlassFishの大雑把に言った場合の構成の図（スライド）
* 依存関係解決 > ☆[HK2](https://hk2.java.net/2.4.0-b29/)
	* A light-weight and dynamic dependency injection framework

---

# @making 「どこよりも早い Spring Boot 1.3 解説」

<iframe src="//www.slideshare.net/slideshow/embed_code/key/iMd8pa4vXaO8jc" width="425" height="355" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//www.slideshare.net/makingx/spring-boot-13-news-java" title="Spring Boot 1.3 News #渋谷Java" target="_blank">Spring Boot 1.3 News #渋谷Java</a> </strong> from <strong><a href="//www.slideshare.net/makingx" target="_blank">Toshiaki Maki</a></strong> </div>

* リリースされていないけど、どこよりも早いよ
* Spring 芸人、コントリビューター
* すごく人気出てきてるよ！
* 1.3の面白い機能の紹介
	* 4つの目玉機能ある
* キャッシュ機能
	* アノテーションで戻り値をキャッシュ > ?キャッシュクリアしたいときはどうするのかなぁ
	* キャッシュ実装簡単に選べる
* ☆[JOOQ](http://www.jooq.org/)
	* SQLをタイプセーフに

* Auto reload Class loader < これはよさそう！

---

休憩タイム

以下LT

---

# @hajimeni 「AWS Lamda with Java/Scala」 ビズリーチ西山さん

<iframe src="//www.slideshare.net/slideshow/embed_code/key/4a2B0MwwISj0P0" width="425" height="355" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//www.slideshare.net/HajimeNi/aws-lambda-51164125" title="AWS Lambda with Java/Scala #渋谷Java 第十二回" target="_blank">AWS Lambda with Java/Scala #渋谷Java 第十二回</a> </strong> from <strong><a href="//www.slideshare.net/HajimeNi" target="_blank">hajime ni</a></strong> </div>

* javaでlambda ＞ 起動時のオーバーヘッドやっぱ大きめ

---

# @bati11_ 「Spring Boot と Swagger」

<div style="max-width:500px; width:100%">
<script async class="speakerdeck-embed" data-id="56097426b20947768405d87e691c3837" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>
</div>

* 刈谷さん、[アソビュー](https://www.asoview.co.jp/) という会社
* Swagger = JSONでAPIを記述する > ドキュメントツール？
* ☆SwaggerEditor, SpringFox

---

# @nikuyoshi 「Javaの資格試験(OCJ-P)を取って何を学んだか｣

<iframe src="//www.slideshare.net/slideshow/embed_code/key/GLOpWAliu8Zc3F" width="425" height="355" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//www.slideshare.net/nikuyoshi/javaocj-p" title="Javaの資格試験(OCJ-P)を取って何を学んだか" target="_blank">Javaの資格試験(OCJ-P)を取って何を学んだか</a> </strong> from <strong><a href="//www.slideshare.net/nikuyoshi" target="_blank">Hiroki Uchida</a></strong> </div>

* Softbankのひと。運用基板開発
* オラクルの資格〜
* ！受験料たけー！
* 勉強の感想　引っ掛けが多い。Javaを実務でやってた人が、総復習がてらに受験するのはいいのでは？　業務に活かしにくい。資格のための勉強。

---

# 芹沢さん代打LT

@unknown_199309　＜　いないのでせりざわさんが

<iframe src="//www.slideshare.net/slideshow/embed_code/key/qggTcddFjPJKPV" width="425" height="355" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//www.slideshare.net/kazuhiroserizawa988/introduction-of-retrofit" title="Introduction to Retrofit" target="_blank">Introduction to Retrofit</a> </strong> from <strong><a href="//www.slideshare.net/kazuhiroserizawa988" target="_blank">Kazuhiro Serizawa</a></strong> </div>


* 経験上、APIドキュメントって間違ってることがままある（ドキュメント整備が追いついていない）
* タイプセーフにエンドポイントつけられる！気に入ってる。使ったこと無いけど

---

ここで休憩。
私用にてここでおいとま

---

# zer0_u 「古いJavaから新しいJavaへ！2年目の挑戦」

* ※スライド未発見。発見後更新します

---

# y_taka_23 「「Java から眺める型システムの世界」

[著者ブログ](http://ccvanishing.hateblo.jp/entry/2015/08/01/203751)

<iframe src="//www.slideshare.net/slideshow/embed_code/key/hDYv5hq6MxzJad" width="425" height="355" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//www.slideshare.net/y_taka_23/intro-featherweightjava" title="Hello, Type Systems! - Introduction to Featherweight Java" target="_blank">Hello, Type Systems! - Introduction to Featherweight Java</a> </strong> from <strong><a href="//www.slideshare.net/y_taka_23" target="_blank">y_taka_23</a></strong> </div>

---

# fmnb0516 「Nashornを使ったSinatra風アプリケーションの実装」

* ※スライド未発見。発見後更新します