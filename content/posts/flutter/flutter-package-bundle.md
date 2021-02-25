---
title: "Flutterで作ったプロジェクトのBundleID・PckageNameを変更する方法と対策"
date: 2021-02-03T00:17:05+09:00
draft: false
tags: ["Flutter"]
---

普通にVSCodeやコマンドからFlutterプロジェクトを作るとBundleID・PackageNameはcom.example.hogehoge のように指定されてしまいます。

これを修正する方法を示すとともに、そうならないようにするVSCodeで開発している方向けの設定をご紹介します。

#  すでに作ってしまった場合の修正方法
- iOS
    iOSの場合は一つだけ変更すれば良いので楽です。
    FlutterプロジェクトのiOSディレクトリに移動します。

    例
    ```shell
    /my_app/ios
    ```
    ここで

    ```shell
    xed .
    ```
    でXcodeでプロジェクトを開きます。

    以下の画像のように`TARGETS`の`Bundle Identifier`を修正します。
    以上で終わりです。

![xcode22](https://lh3.googleusercontent.com/pw/ACtC-3dawLqkuoQDpNPNTR7nY6P5KrkJAUHZ5Okk5Rl-KWR4cC3wMdyjFwMlQvfzQ34f1vo4PcWWF7lWgbZ_Rh8ScRm887eOoz3kBXKixkws9qujFMX_VBBofIKKDrXtkurZapenpZcsFRWH-xdvpBmpTktrhA=w2436-h1590-no?authuser=0)

- Android
    Androidの場合は名前の修正とディレクトリの修正の二つがあります。

    - ネーム変更
        VSCodeでプロジェクトを開きます。
        `⌘ + shift + f`でプロジェクト全体の検索画面を開きます。
        以下のように検索窓に`com.example.{プロジェクト名前}`を入れて検索します。

        ![searchvscode](https://lh3.googleusercontent.com/pw/ACtC-3eHwFD5jSfF9A17UWc51lyTZqk4jdY3f6eTIcqmrhuJ_wxvOghOPf4Yh-c-xiBiNSulFeeexo6lDiyrfLEjd1-2cjDqLtJ9loUsuMVlcXZJiQX3TJkg6WUAEvC_Bqm4iIAJuVTjp4c80XUGaHBS2ajDbA=w2054-h1590-no?authuser=0)

        検索窓の左側に下向きの矢印ボタンがあるのでそれをクリックします。
        その後置換するための窓が出てきますので、そこに変更したいPackage Nameを入れて、右側にある一斉置換のボタンをクリックすると名前を変更することができます。

        ![vscodereplace](https://lh3.googleusercontent.com/pw/ACtC-3dxDVS9Lf4ZiZqRM67psCsqhnOPRGyaAH79P7qzS8dLpcGfQYFgyj9ZZD18_6yWkC1tDAQG2bCbYQv1MsJfylAzSylUAr6B98oUv7CFf3lVUTzUxgv1q3yFsxKQf2aSJX28eVvDbSFFS8l1_d--Eu2oKg=w2054-h1590-no?authuser=0)

    - ディレクトリ変更
        Androidのデフォルトの言語が`java`か`kotlin`によって変わりますが、大体`kotlin`ですので、以下のようなディレクトリ構成になっていると思います。
        ```
        my_app/android/app/src/main/kotlin/com/example/my_app
        ```

        ですので、この/com/exampleの名前を設定したいPackage Nameに変更しておきます。
    
    以上で終わりです。


# VSCodeの場合のより良い方法
上記のようなことを毎回やるのはよろしくないので、次からは自動的にやってもらいたいと思います。

 - Settings.jsonを開く
 `ショートカットキーcommand + ,`

![settings](https://lh3.googleusercontent.com/pw/ACtC-3caFZQeI3IAhLqg6Hub10l12yqqdZ67G3_caKh12xz_uF4MTnAMus_4WW_PBIxNTVcozXG2OhXWRJ78DXKDM2eWq9QVEttypkn2PHEIF_v7bTE_9-sNr9tfiCOmhUa8NeVwI6p-aCp7wO0QktgzUMA7ow=w2272-h1760-no?authuser=0)

`organization`で検索し、DartのExtensionsの`Flutter Create Organization`から`Edit in settings.json`をクリックします。

![edit](https://lh3.googleusercontent.com/pw/ACtC-3cdGzDCr8HQTy46L38pV1GwesHMywM3iSCsaeH9KlPl7IgBAL6Q4yctJYxMFntH0rbnEqFLSiCO7YwZGPeoZAGuVnTNhVY6IYcf_7QiiZp-v5twF_qOQ99xZmn1ZwZrH8GD0B4KbZlP_xLu3zj7B4aUCg=w2272-h1760-no?authuser=0)

開くと以下のようなjsonがあるはずです。

![jsonfile](https://lh3.googleusercontent.com/pw/ACtC-3chYVZmiwvAjvuM2c9eovnOL15hYCfk3FWgkNv7GmAQ325bXwVAgph0GV74tF2uHyAWkim25JvTMonpTWQeHAKws16AFK3IZ0dPdoMj4LPEmwUwdz3ornuRgU4yCoUko5SBMxB3uUHTd5ISHySdecObsA=w2272-h1760-no?authuser=0)

これの`art.flutterCreateOrganization`が`null`になっているため、ここを自身の`organization`を設定してあげます。

```settings.json
{
    "[dart]": {
        "editor.formatOnSave": true,
        "editor.formatOnType": true,
        "editor.rulers": [
            80
        ],
        "editor.selectionHighlight": false,
        "editor.suggest.snippetsPreventQuickSuggestions": false,
        "editor.suggestSelection": "first",
        "editor.tabCompletion": "onlySnippets",
        "editor.wordBasedSuggestions": false
    },
    "dart.flutterCreateOrganization": "com.tsuzuki817"
}
```

そうすると次回からVSCodeでflutterプロジェクトを作成した時には自動的に設定した文字列からorganizationが設定されるようになります。

![result](https://lh3.googleusercontent.com/pw/ACtC-3eOCpXDYCfTmUpLG-H_oXC7aPa5vt8mCmsSVi_Vjh1u_CtB03DBYhGowdU_uD8X7QeXZEQRJoZqQFcbpvSsFEiaW_avZlm26Z5cj4BBIwXgqhb696Tp40HikRf-1Y7NVf36h7rQwi6KFWAu-B6LCFhvmQ=w2054-h1590-no?authuser=0)

# コマンドでやりたい人向け
プロジェクトを作るときに以下のコマンドを打つと、指定されたorganizationが反映されるそうです。

```shell
flutter create --org com.your.orgname my_app_name
```

# 参考URL
- [VS Codeのsettings.jsonの開き方](https://qiita.com/y-w/items/614843b259c04bb91495)
- [Visual Studio Codeの一括置換](https://qiita.com/hayatokunn/items/76b41e00f114cf9a9f140)
- [Flutterでの開発をスムーズに行うためのTips集](https://medium.com/flutter-jp/tips-b2487a63a8)
- [Flutterでパッケージ名がcom.exampleになっているのをiOS/Androidで修正する箇所まとめ](https://qiita.com/osamu1203/items/6adfab47e562f5e7d03b#1-androidappsrcmainandroidmanifestxml)