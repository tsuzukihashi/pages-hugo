---
title: "Flutterで"
date: 2021-02-03T00:17:05+09:00
draft: false
tags: ["Flutter"]
---

# VSCodeの場合
 - Settings.jsonを開く
 `ショートカットキーcommand + ,`

![settings](https://lh3.googleusercontent.com/pw/ACtC-3caFZQeI3IAhLqg6Hub10l12yqqdZ67G3_caKh12xz_uF4MTnAMus_4WW_PBIxNTVcozXG2OhXWRJ78DXKDM2eWq9QVEttypkn2PHEIF_v7bTE_9-sNr9tfiCOmhUa8NeVwI6p-aCp7wO0QktgzUMA7ow=w2272-h1760-no?authuser=0)

`organization`で検索し、DartのExtensionsの`Flutter Create Organization`から`Edit in settings.json`をクリックします。

![edit](https://lh3.googleusercontent.com/pw/ACtC-3cdGzDCr8HQTy46L38pV1GwesHMywM3iSCsaeH9KlPl7IgBAL6Q4yctJYxMFntH0rbnEqFLSiCO7YwZGPeoZAGuVnTNhVY6IYcf_7QiiZp-v5twF_qOQ99xZmn1ZwZrH8GD0B4KbZlP_xLu3zj7B4aUCg=w2272-h1760-no?authuser=0)


開くと以下のようなjsonがあるはずです。

![json](https://lh3.googleusercontent.com/pw/ACtC-3chYVZmiwvAjvuM2c9eovnOL15hYCfk3FWgkNv7GmAQ325bXwVAgph0GV74tF2uHyAWkim25JvTMonpTWQeHAKws16AFK3IZ0dPdoMj4LPEmwUwdz3ornuRgU4yCoUko5SBMxB3uUHTd5ISHySdecObsA=w2272-h1760-no?authuser=0)

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
    "dart.flutterCreateOrganization": "com.hogehoge"
}
```



# 参考URL
- [VS Codeのsettings.jsonの開き方](https://qiita.com/y-w/items/614843b259c04bb91495)
- [Flutterの効率良い学び方](https://medium.com/flutter-jp/flutter-learning-c5640c5f05b9)
- [Flutterでの開発をスムーズに行うためのTips集](https://medium.com/flutter-jp/tips-b2487a63a8)