# 1. Hello World Vue.js + Java Application

<!-- TOC -->

- [1. Hello World Vue.js + Java Application](#1-hello-world-vuejs--java-application)
    - [1.1. 概要](#11-概要)
    - [1.2. 構築手順](#12-構築手順)
        - [1.2.1. gradleとnpm初期設定](#121-gradleとnpm初期設定)
        - [1.2.2. backend - Javaを構築](#122-backend---javaを構築)
            - [1.2.2.1. SampleController.java](#1221-samplecontrollerjava)
            - [1.2.2.2. Main.java](#1222-mainjava)
        - [1.2.3. frontend - vue.jsを構築](#123-frontend---vuejsを構築)
        - [1.2.4. アプリケーションを起動する](#124-アプリケーションを起動する)
    - [1.3. 参考](#13-参考)

<!-- /TOC -->

## 1.1. 概要
Vue.jsをフロントエンドに、Javaで書いたWeb APIをバックエンドにしたアプリケーション。

JavaのHello World REST APIを作って、フロントエンドのvue.jsに表示するだけのシステムを作る。

## 1.2. 構築手順

### 1.2.1. gradleとnpm初期設定

```shell
mkdir helloVueJava
cd helloVueJava
mkdir backend
mkdir frontend
cd backend
gradle init --type java-application
cd ../frontend
npm init
```

### 1.2.2. backend - Javaを構築

Spring BootでHello Worldを構築する。

1. project nameを変更
    - settings.gradle
    - `rootProject.name = 'hellovuejava-backend'`
1. gradleにSpring Boot Gradle pluginを追加する
    - <https://docs.spring.io/spring-boot/docs/current/reference/html/build-tool-plugins-gradle-plugin.html>
1. Spring Boot Webを依存関係に追加
    - `spring-boot-starter-web`
1. main classを変更
    - `mainClassName = 'hello.Main'`
1. mkdir src/main/java/hello
    - SampleController.javaを作成
    - Main.javaを作成
1. gradle buildを実行
1. gradle bootrun
    ```
    :compileJava UP-TO-DATE
    :processResources NO-SOURCE
    :classes UP-TO-DATE
    :findMainClass
    :bootRun

    .   ____          _            __ _ _
    /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
    ( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
    \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
    '  |____| .__|_| |_|_| |_\__, | / / / /
    =========|_|==============|___/=/_/_/_/
    :: Spring Boot ::        (v1.5.3.RELEASE)
    略
    ```
1. ブラウザで`http://localhost:8080/hello`にアクセス
    - **Hello World!** と表示されたら成功！

#### 1.2.2.1. SampleController.java
```java
package hello;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class SampleController {

    @GetMapping("/hello")
    String hello() {
        return "Hello World!";
    }
}
```

#### 1.2.2.2. Main.java
```java
package hello;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Main {

    public static void main(String[] args) {
        SpringApplication.run(Main.class, args);
    }
}
```


### 1.2.3. frontend - vue.jsを構築

Javaと違って実装するファイルが多いのでファイルの内容は割愛。

1. .gitignoreを書く
    - `/node_modules`
1. npmで必要なライブラリを追加する
    - `npm i -S vue materialize-css jquery`
    - `npm i -D babel-core babel-loader babel-preset-es2015 css-loader file-loader style-loader url-loader vue-loader vue-template-compiler webpack webpack-dev-server`
1. .babelrcを書く
1. アプリケーションを実装する
    - src
        - js
            - domain : ドメインクラスを置く場所
                - Hello.js : WebAPI GET処理
            - presentation 
                - vue_components
                    - Application.vue : Vueコンポーネントのルート
                    - Hello.vue
            - index.js : Vueインスタンスを作る
        - index.html : index.jsを読み込むだけHTML
1. webpackの設定を書く
    - `webpacl.config.js`
1. package.jsonにbuildコマンドを追加する
    - `"build": "mkdir -p build && cp src/index.html build/index.html && webpack",`
1. `build`フォルダにindex.htmlとbundle.jsなどが出来てたらOK

### 1.2.4. アプリケーションを起動する

1. Web APIを起動
    - cd backend
    - gradle bootrun
2. フロントエンドのindex.htmlをブラウザで開く
3. `PRINT MESSAGE`ボタンを押して、`Hello World!!`と表示されたら成功!!
    - 動かないときは、ブラウザのWeb開発ツールを開いてコンソールやデバッグしてみる。


## 1.3. 参考
- Spring Boot
    - [Spring Boot](https://projects.spring.io/spring-boot/)
    - [Spring Boot 使い方メモ - Qiita](http://qiita.com/opengl-8080/items/05d9490d6f0544e2351a#web-%E3%82%A2%E3%83%97%E3%83%AA%E3%82%92%E4%BD%9C%E3%82%8B)
    - [Spring Boot - CORS](https://docs.spring.io/spring/docs/4.2.x/spring-framework-reference/html/cors.html)
- Vue.js関連
    - [GUIアプリケーションアーキテクチャ総合！みたいなやつ書いてる](http://nekogata.hatenablog.com/entry/2017/03/26/132150)
        - この記事読んでフロントエンド開発やってみようと思った。
    - <https://github.com/lavenderx/modern-java-web-scaffold>
- [materializecss](http://materializecss.com/)
    - 簡単にキレイになって好き
