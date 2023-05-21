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

React Native for macOSは基本的に、iOS向けのReact Nativeに一部macOS対応を追加したものです。React NativeはOS標準のUIとReact NativeのViewのAPIをマッピングしています。iOSでは、このマッピングにはUIKitが使用されています。
しかし、iOSとmacOSでは、使用するUIフレームワークが異なります。具体的には、iOSではUIKitが、macOSではCocoaが使用されます。
一見、React Native for macOSがCocoaと連携しているように見えますが、実際にはMac Catalystというフレームワークを利用しています。Mac Catalystは、macOS標準のCocoaではなく、iOSのUIKitを用いてmacOSアプリケーションを開発するためのフレームワークです。
しかし、このMac Catalystは、完全にiOSで動作するUIKitとは同一ではありません。一部にはmacOS向けの最適化が施されています。そのため、React Native for macOSでは、一部の最適化を無効化し、iOSとの外観を統一するなどの追加処理が行われています。

### for Windowsの実装

React Native for WindowsはmacOSとは対照的に非常に多くのマッピングが実装されています。macOSがiOSのモバイル向けの従来から存在するReact Nativeの資産を応用できたのに対して、WindowsはiOS, Androidいずれにおいても共通性がありません。

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

![](/images/rn-xp/pic1.png)

理論上では、同一のコードベースから同一の外観と動作を保証することでiOSとAndroid, macOS, Windowsといった幅広いプラットフォーム向けのアプリケーションを最小コストで作成が可能なように思えます。
しかし、この場合に注意しなければならないのがプラットフォーム特有のユーザー体験です。上の図のように、OS毎に独自のUIデザイン思想があり同様の機能を提供していてもそのデザインやエクスペリエンスは大きく異なります。
従って、macOSを想定して作成したUIや挙動がWindowsユーザーにとって自然ではないといったことがよく発生し

## 実装と実験・検証

この章では、React Native macOS+Windowsを実際に導入したプロジェクトにおいて動作させ、以下のような評価項目で検証を行います。

**検証する観点**

- 外観の同一性が担保できるか
- ボタンなどのイベントなどの取り扱いや挙動が同一であるか
- React Native向けの主要なUIライブラリ•フレームワークが導入可能であるか
- ネイティブモジュールを含むライブラリを導入可能か

これらを検証するための、サンプルプロジェクトを以下のリポジトリに作成しています。



### macOSでの実行環境

macOSにおいての開発環境はiOSとほとんど変わりありません。必要なものはmacOSとXcodeです。また、React NativeのiOSパッケージは内部的に[Cocoapods](https://cocoapods.org/)を依存関係解決に用いているため[Homebrew](https://brew.sh/index_ja)などを利用して予めインストールする必要があります。
既存のReact Nativeプロジェクに対して以下のコマンドを実行することでmacOS用の拡張をプロジェクトに組み込みます。

**実装環境**

![](/images/rn-xp/macos-exp-specification.png)


## Windowsでの環境構築

Windowsで必要な環境はWindows 10もしくはWindows 11とVisual Studio環境です。
Visual StudioはCommunity版でも問題ありません。Visual Studio Codeとは異なりますので

**実験環境**

![](/images/rn-xp/win-spec.png)

| 項目  | 環境  |
|:----------|:----------|
| OS | System type	64-bit operating system, ARM-based processor |
| CPU | M2 Max / Apple Silicon 3.20 GHz  (4 processors) |
| RAM | 8GB (Parallels Virtual RAM) |


## References

- React Native for Windows + macOS · Build native Windows &amp; macOS apps with Javascript and React. https://microsoft.github.io/react-native-windows. Accessed: 2023-5-20.

- Github - microsoft/react-native-macos. https://github.com/microsoft/react-native-macos/tree/main/React. Accessed: 2023-5-20.

- Github - microsoft/react-native-windows. https://github.com/microsoft/react-native-windows. Accesed: 2023-5-20.

