---
title: "SwiftのWebAssembly環境を構築してみた"
emoji: "😎"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Swift", "WebAssembly"]
published: false
---

WebAssemblyは多くのWebブラウザで実行することができるアセンブラっぽい中間言語です。これを使うことで、

- Rust, C++といったネイティブコードをWebブラウザで動作させることができる
- スクリプトよりもネイティブのバイナリに近い高速で実行ができる

という利点があります。
たとえば、Web向けのUnity（ゲームエンジン)やGoogle Meetの背景ぼかし処理などはWebAssemblyを用いることでWebベースのアプリケーションでありながらネイティブアプリと同様の機能やスピードをユーザーに提供することが可能となっています。
また、P2Pでの分散処理基盤などでも安全にコードを実行する仮想マシンとしてWebAssemblyを採用する事例が出てきています。

今回は、そんなWebAssemblyをSwiftで記述しコンパイルするための環境構築を行います。




## Swift向けWasmビルド基盤carton

https://github.com/swiftwasm/carton

Swiftのコンパイラは標準ではWebAssembly向けのバイトコードにコンパイル・ビルドする機能を持っていません。しかしSwiftがバックエンドとして用いているコンパイラ基盤であるLLVMはWASM向けコンパイルが可能です。しかし、通常のSwfitプロジェクトをビルドするように`swift build`を実行すればいいと言うわけではありません。
そこで、Cartonはこの*Swift Build*と同じ役割をWebAssembly向けのプロジェクトで果たしてくれています。

インストールは下記のコマンドでHomebrew経由で行うことができます。

```shell
$ brew install swiftwasm/tap/carton
```


## Cartonを使ってSwiftのWasmプロジェクトをビルドする

次に、プロジェクトの作成を行います。