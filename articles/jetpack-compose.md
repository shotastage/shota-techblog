---
title: "SwiftエンジニアのためのJetpack Compose"
emoji: "💭"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Swift", "Android", "Kotlin"]
published: false
---

## 環境構築

Androidアプリケーションを開発するのに必要なのはAndroid Studioです。ちょうど、我々iOSエンジニアがXcodeを使うのと同じで、簡単に言うとXcodeのAndroid版です。
以下のリンクから、Android Studioをダウンロードして初期セットアップを完了させてください。Java系統のランタイムやエミュレーターなどはこのIDEがよしなにセットアップしてくれます。

[Android Studio](https://developer.android.com/studio?gclid=CjwKCAjwjYKjBhB5EiwAiFdSfr0umtwdrWZey-1bYqfkLGwJJSlgxpjnYN2G1QQfwBkYESTpgHND3hoCskIQAvD_BwE&gclsrc=aw.ds)


## プロジェクトの作成

さて、早速Android Studioを使って初めてのAndroidプロジェクトを作成します。基本的にはXcodeと大きくは変わりませんが、いくつかAndroid OSならではのポイントがあります。

### OSサポート下限の設定

![Android Studioバージョン選択画面](/images/and11-vselect.png)

iOSにおいても、どのバージョンのOSまでサポートするか下限を決定します。iOSの場合は２個前のメジャーバージョンをサポートするのが一般的ですがAndroidの場合は事情が異なります。
試しに、2個まえのAndroid 11を指定するとたったの40%ほどのデバイスしかサポートされないと出てきました。
実は, Android端末はiOSと比較してバージョンのフラグメント化が激しく中々最新のバージョンにアップグレードされないという事情があります。これは、iOS
