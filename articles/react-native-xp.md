---
title: "React Native for Windows + macOSを使ってみる"
emoji: "🌟"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["react", "reactnative"]
published: false
---

## 概要

React Native for Windows + macOSはXXX年にMicrosoftからリリースされたReact Nativeの拡張モジュールです。本流のReact Nativeが対応するiOSとAndroidのモバイルプラットフォームに加え、macOSとWindowsのデスクトッププラットフォームもサポートしています。
最大の成果は、クロスプラットフォーム開発をモバイル領域からデスクトップOSにまで拡大し、モバイルとデスクトップの両プラットフォーム間の垣根をなくしたことです。この成果は、モバイルアプリとデスクトップアプリの開発をより少ない予算と人員で行うことを可能にしました。

React Nativeと同じく、ネイティブのUI描画機構とReact NativeのJavaScript APIをマッピングし、JSXによるUI構造定義とCSSによるスタイリングを可能にします。また、JavaScriptの動作に関しては、React Nativeと同様にJavaScriptCoreが採用されており、技術的特性はほとんど同じです。

*React Native XPはReact Nativeと同様に同一のコードベースから同一外観＆同一動作をほぼ保証し*


## React Native XPがもたらすビジネスインパクト

React Native XPはReact Nativeがビジネスにもたらすメリットをより強化します。React Nativeがモバイルアプリケーションに開発においてAndroid、iOS両方のアプリケーションを同一コードでの開発を可能にし低予算かつマイクロな開発チームで開発可能にしました。

これにより、限られた予算内でモバイルアプリ開発という手段を取るのを可能にしました。

React Native XPは更にデスクトップアプリにもこのメリットを拡張することで、モバイルアプリを開発するのとほとんど変わらない人員と予算でデスクトップアプリを視野に入れることができるようになります。

このメリットは主にエンタープライズ向けのソリューション開発において大きなメリットとなります。エンタープライズ領域でのDX化は、企業内での作業の多くがPCで行われることから PC向けのソフトウェア開発が


## React Native XPの技術的特性

### for macOSの実装

React Native for macOSは基本的にiOS向けのReact Nativeに一部macOS向けの対応を追加したものです。
iOSとmacOSは同じ系統のOSであり、類似したUIのAPIを持ちます。しかし、完全に同一なわけではなくiOSであればUIKit, macOSではCocoaと呼ばれるUIフレームワークを用います。
当然、従来のiOSではReact NativeのViewをUIKitにマッピングしています。では、React Native XPのmacOSにおいてはどうでしょうか。
これは、

### JavaScriptランタイムの違い

![Comparison of JavaScript VM amoung each OS](/images/rn-xp/figure_001.png)

React Native XPの特筆すべき特徴の一つは、WindowsでのJavaScriptランタイムの選択です。

React Native自体はJavaScriptの仮想マシン（VM）としてJavaScriptCoreを採用しています。これはWebKit系のブラウザ、例えばSafariなどに使用されており、iOSをはじめとするMac系のOSではOSのフレームワークとしても内臓されています。また、AndroidにおいてはOS標準のフレームワークではありませんが、挙動の一貫性を保つために標準のV8ではなくAndroid向けにビルドしたJavaScriptCoreをバンドルします。
また、React Native XPにおいてもmacOSはiOSと同じJavaScriptCoreを用います。

このようなことから、iOS, Android共にJavaScriptCoreを用いるのであれば、WindowsにおいてもJavaScriptCoreが採用されるのが自然に思えます。

しかし、React Native XPではWindowsに限りChakraというVMが採用されています。

一体、なぜなのでしょうか？

これは、MS公式の見解が示されていないためある程度推測になりますが、このような手段を採用した理由として以下のようなものが考えられます。

- Chakraを採用することでWindows UIのAPIとJavaScript APIをマッピングする実装コストを最小限に抑えた
- ChakraがWindows OSのエコシステムとして既に安定しており、JavaScriptCoreによる挙動の安定性よりメリットがあるから

Chakraは、もともとInternet ExplorerでJavaScriptの処理を担当していたJScript9というDLLでした。その後、MicrosoftがInternet Explorerの後継としてMicrosoft Edge（Project Spartan）の開発に際し、このJScript9をフォークしました。このフォークにより生まれたのがChakraで、JScript9によるVBのサポートなどで肥大化していたInternet Explorerの機能が削除され、スリム化が進められました。さらに、ECMAScript2015などの最新のJavaScript記法への対応を備えたJavaScript VMとして機能しています。

現在、Windowsの標準ブラウザであるEdgeはChromiumベースに変更されChakraはV8に置き換えられましたがWindowsのアプリケーションフレームワークとしてChakraは残されました。多数のWindows UIがChakraからアクセス可能になっています。

このような事から、ChakraがWindowsにおいてのJS VMとして最適解だと判断されたのだと考えられます。



## プラットフォーム固有のユーザー体験最適化

理論上では、同一のコードベースから同一の外観と動作を保証することでiOSとAndroid, macOS, Windowsといった

## Windowsでの環境構築

今回、検証を実施した環境は以下のような環境。

**実験環境**

| 項目  | 環境  |
|:----------|:----------|
| OS | Windows 11 Pro (ARM) |
| CPU | M2 Max (Parallels Virtual CPU Quad-Core) |
| RAM | 8GB (Parallels Virtual RAM) |



