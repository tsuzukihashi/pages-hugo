---
title: "Flutter入門！"
date: 2021-01-30T04:08:10+09:00
draft: false
tags: ["Flutter"]
---

# はじめに
兼ねてより気になっていたFlutterというものを始めようかと思います。

2019年にヤフーに新卒入社した右も左もわからなかった自分が、運良くPayPayフリマのiOSチームに配属されて技術的にも人間的にも優れた先輩方に教わりながらiOSアプリ開発も少しずつできるようになってきました。

Swiftでの個人アプリ開発もつづけ、そろそろAndroidも対応した物を作りたいという気持ちが僅かながら芽生えてきました。

しかし、ネイティブで両方作るのは結構しんどいのでは？という気持ちもありまして、一粒で二度美味しいと評判の`Flutter`に手を出すことにしました。

# Flutter

FlutterはGoogleによって開発されているマルチプラットフォームで動作するモバイルアプリケーションUIフレームワークです。

開発言語はDartと呼ばれるもので、こちらもGoogleによって開発されています。

プラットフォームも言語もGoogleによって統率されており、なかなかGoogleが気合を入れていることが感じざるを得ないものとなっています。

Firebase周りとの連携もしやすいとも聞いているので、やるしかないです。

# 開発環境

開発環境は`Android Studio`と`VSCode`が主流らしいです。

自分は`VSCode`が見た目的に好きなのでそちらでやることにします。

# インストールしてみる
※ Android Studio, Xcode, Homebrew, Cocoapodsがインストールされてる前提

Flutter自体はは公式サイトからzipファイルをダウンロードして任意の場所においてパスを指定する。

[公式サイト](https://flutter.dev/docs/get-started/install/macos)

パスは任意の場所でよく、以下のようにbinまでしてあげれば良いです。

```shell
export PATH="$PATH:$HOME/dev/flutter/bin"
```

ここで
```shell
flutter doctor
```

というコマンドを利用します。これでflutterが使えるのかどうか見てくれます。

さらに、修正方法も示してくれます。

続いて、VSCodeにFlutterのExtensionをインストールする。

コマンドパレットを開いて(⌘ + shift+ P)、`flutter`で検索すると`New Application Project`と出てくるのでそこからプロジェクトを作れます。

![コマンドパレット](https://lh3.googleusercontent.com/pw/ACtC-3cdvIRWcOjQxbBsb9PkDKgpcqvDTPwhYDwk8T5bFW1_HiAAhsFxwmlcJ18mh_QZsbJqlZGvb-S3sfWNjIVtqMhq7DIIBiLcxSr1tzYboHMCB0xH0b8V0ZQJhD55RcvGhfr5SaIMofT2ToP5w63Ep-qwwQ=w1216-h848-no?authuser=0)

プロジェクトの名前を決定してあげると、プロジェクトが表示されます。

次にデバイスの選択方法です。

画面右下のデバイス領域をクリックすると、画面上部に選べるシミュレータが表示されます。

![デバイス選択](https://lh3.googleusercontent.com/pw/ACtC-3c1yUfSHgk1aOtpmZJSbV4BJ1mK98kSCbiToM7n6R3ajx-kLXoyYFWnlEWKFZkFnKUgCR8IbD4tgAY6wwIF1PMXU7D8MJVJGmDKTDFJmTJJ71QQQ4NIrvxRiz1Sh5w9pUzFWedrWeSWB94C9MfRz46NWQ=w3072-h1928-no?authuser=0)

なかったら、`Create Android emulator`したり、iOSの場合、terminalで

```shell
open -a simulator
```

と入力しても起動できるようです。（シラナカッタ）

# アプリを実行してみる

右上の再生マークをクリックして実行できます。

![実行](https://lh3.googleusercontent.com/pw/ACtC-3fcSMPp0AavZZYDMOVMXTo63RFTwUTOA8uFPD3e-_TXspbCvp-disWvOg4IdNHt-EVY_DKwa-jA-5aaojYBwTY7We0eA9QyHYAuWY80XFWo-ieTLebmJNv74krA6YEIpF9N9HmPVTrYBAvy7L7vHXfOKw=w3072-h1928-no?authuser=0)

以下のようにでもアプリが表示されればOK！
![Demo](https://lh3.googleusercontent.com/pw/ACtC-3e9sSHrO_K2FFyS5vzYJM8FY3Nx8cgl4aVS27vnf1ZvN6DhTuEGO2rdu3Jx54gZTQLtO9LchL_gZYdoBrOjWizT3pHqg2U-snLI3MHhgTcaTP79H-fc2RTnElbDa8xIakO379LGyhjESRewuMJu9jk2Ag=w880-h1644-no?authuser=0)

