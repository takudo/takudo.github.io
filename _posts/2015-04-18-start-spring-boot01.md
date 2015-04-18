---
layout: post
title: "最初の Spring-boot x Gradle x IntelliJ（スケルトンアプリの作成とデバッグ実行まで）"
date: 2015-04-19 01:00:00 +0900
comments: false
tags: Java spring-boot gradle intellij 
description: Java spring-boot gradle intellij 
header_img: https://qiita-image-store.s3.amazonaws.com/0/34165/e8e89c9f-fa76-3e99-b279-cbf4b3d37c55.jpeg

---

__※注意※　2015-04-19時点で、自動コンパイルが出来ていません__

---

# はじまりはじまり

[公式](http://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/#getting-started-installing-spring-boot)をみる

# スケルトンアプリの立ち上げ

mac 環境なので、homebrewでgradle入れます。

<!--more-->


```
$ brew install gradle 
```

ファイル作ります（[コピペ](http://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/#getting-started-gradle-installation)）

```
buildscript {
    repositories {
        jcenter()
        maven { url "http://repo.spring.io/snapshot" }
        maven { url "http://repo.spring.io/milestone" }
    }
    dependencies {
        classpath("org.springframework.boot:spring-boot-gradle-plugin:1.3.0.BUILD-SNAPSHOT")
    }
}

apply plugin: 'java'
apply plugin: 'org.springframework.boot.spring-boot'

jar {
    baseName = 'myproject'
    version =  '0.0.1-SNAPSHOT'
}

repositories {
    jcenter()
    maven { url "http://repo.spring.io/snapshot" }
    maven { url "http://repo.spring.io/milestone" }
}

dependencies {
    compile("org.springframework.boot:spring-boot-starter-web")
    testCompile("org.springframework.boot:spring-boot-starter-test")
}
```

エントリポイントのファイル作ります（[コピペ](http://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/#getting-started-first-application-code)）

src/main/java/Example.java

```
import org.springframework.boot.*;
import org.springframework.boot.autoconfigure.*;
import org.springframework.stereotype.*;
import org.springframework.web.bind.annotation.*;

@RestController
@EnableAutoConfiguration
public class Example {

    @RequestMapping("/")
    String home() {
        return "Hello World!";
    }

    public static void main(String[] args) throws Exception {
        SpringApplication.run(Example.class, args);
    }

}
```


アプリを立ち上げる

```
cd path/to/build.gradle.dir
$ gradle bootRun
```

ブラウザを確認して（ http://127.0.0.1:8080 ）、 Hello World! が表示されてたらOK！

---

# IntelliJでデバッグする

まずこちら読んで下さい

<blockquote class="twitter-tweet" data-cards="hidden" lang="ja"><p>Intellij × Spring-boot × Gradleの開発時のHotSwap問題（と勝手に名付ける）、&#10;<a href="http://t.co/998KtzAaWK">http://t.co/998KtzAaWK</a>&#10;このページの手動リロードで一応できることはわかったけど、&#10;手動面倒なのですがどうにかならないですか。お助け。</p>&mdash; takudo (@takudo_dev) <a href="https://twitter.com/takudo_dev/status/585821838090969088">2015, 4月 8</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

<blockquote class="twitter-tweet" data-conversation="none" data-cards="hidden" lang="ja"><p><a href="https://twitter.com/takudo_dev">@takudo_dev</a> JRebel使うとか？&#10;|qω・`)ﾁﾗｯ&#10;<a href="http://t.co/S6eVANNulq">http://t.co/S6eVANNulq</a></p>&mdash; いまいまさのぶ (@masanobuimai) <a href="https://twitter.com/masanobuimai/status/585830946147971072">2015, 4月 8</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

<blockquote class="twitter-tweet" data-conversation="none" lang="ja"><p><a href="https://twitter.com/masanobuimai">@masanobuimai</a> <a href="https://twitter.com/takudo_dev">@takudo_dev</a> IDEAの自動コンパイルは（デバッグ)実行時は働かないという謎仕様なのでcmd(ctrl)+sをmakeに割り当てるか、ターミナルからアプリケーションを起動しておくとかいう回避策があるにはあります。(JRebel使ってても同じ</p>&mdash; 山本裕介 (@yusuke) <a href="https://twitter.com/yusuke/status/585832372718829569">2015, 4月 8</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

ここできちんと理解しておくべきは、

* __自動コンパイル__
* __自動リロード（クラスリロード）__

の2つが無いと、Play2みたいなイメージのデバッグはできないということです。

## IntelliJでプロジェクトを読み込む

割愛

## NG: 自動コンパイル

* [このへん](http://qiita.com/Sa2/items/c3150e3d43698cd67ff1) を参考にしましたが、うまく行きません。
* あとfswatchを使ってgradle build を走らせてみましたが、遅すぎでした(>_<)
    * fswatch使って個別のファイルコンパイルさせればうまくいくのかなぁ...
    
## 代替: 手動コンパイル＆クラスリロード（2015-04-18時点でのよさげな手段）

http://www.jetbrains.com/idea/help/reloading-classes.html

* IntelliJからこのアプリを起動します
    * Example.javaを開いて、右クリック→ Debug を選べば起動するはずです。
* Example.javaの内容を変更します。
* メインメニューの Run＞Reload Changed Classes を実行するとアプリが再起動することなく、再コンパイルされたクラスがリロードされます
* デバッグ起動しているので、ブレークポイントとかも貼れます。

---

# 感想とかこのあと

* 手動トリガーのコンパイルでも結構早いところが救いかな...
* fswatch（というかファイル変更検知）をうまく使ってなんとかしたい



