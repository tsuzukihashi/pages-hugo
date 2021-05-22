---
title: "Flutter Widget of the Week全部見てみる"
date: 2021-03-14T04:08:10+09:00
draft: true
tags: ["Flutter", "Widget", "WidgetoftheWeek"]
---

# はじめに
こちらのmonoさんのブログで`Widget of the Week`を全話見ることを推奨されていたので従順に全部見て手元でも実際に動かして見つつブログも書いていきます！

[Flutter はじめの一歩](https://medium.com/flutter-jp/first-step-9b7f2c74fb08)

作業リポジトリ: https://github.com/tsuzukihashi/Flutter_Widget_of_the_Week

# 001 SafeArea
https://www.youtube.com/watch?v=lkF0TQJO0bA&list=PLjxrf2q8roU23XGwz3Km7sQZFTdB996iG&index=2

ノッチがあったり、通知が来たりしてレイアウトが複雑になるのをシンプルに解決してくれるのが`SafeArea`ウィジェット

`MediaQuery`を使って画面の範囲を調べ、childウィジェットを適切なサイズに調整してくれる便利なウィジェットです。

| SafeArea無し | SafeArea有 |
| -- | -- | 
| ![safeare-nothing](https://lh3.googleusercontent.com/pw/ACtC-3fNmgqAECI7fcyieYOB_cJKtrRHztUrviu1MJTQzIBoXjcfYlc6Q_uzYZEQurnqvNRZRsNjwLNjQzeL3UBR-k96VGeqgo2mGFHfCBFVC_RuPYQyzq9PDYsAWGp0HhAlVrklgcz9fRdwgTEgi1-nvWVJmQ=w736-h1590-no?authuser=0) | ![safearea](https://lh3.googleusercontent.com/pw/ACtC-3fncOVmhrcK4qjy8dT_HtXR-HlFpksOaw2s5q2dW2GCQylkZUsmBGVbBvOzLsOvgcDJ5Ggi3fjpy5Si4SiJ1dLX-SpaH29hzbb_oakF7uDYFojTO6NoYXvGh7OVh7RavJo8t3xT1eb-oVVa3zBmCKZlgQ=w736-h1590-no?authuser=0) |

引数に`bottom`, `left`, `top`などがあり、SafeAreaを有効化するのを指定することができます。

https://api.flutter.dev/flutter/widgets/SafeArea-class.html

# 002 Expanded
https://www.youtube.com/watch?v=_rnZaagadyo&list=PLjxrf2q8roU23XGwz3Km7sQZFTdB996iG&index=3

