---
title: "Flutterチュートリアルやってみた part2"
date: 2021-03-12T04:08:10+09:00
draft: false
tags: ["Flutter"]
---

- [参考](https://codelabs.developers.google.com/codelabs/first-flutter-app-pt2#0)
- 作業リポジトリ => https://github.com/tsuzukihashi/flutter-tutorial-part01

# 環境
- 作業日: 2020/03/12
- OS: macOS Big Sur 11.2.1
- PC: MacBook Pro (13-inch, M1, 2020)

# はじめに
前回に引き続きpart2をやっていきたいと思います。

part2では画面の遷移とテーマカラーの変更がテーマのようです！

前回までのコードは、こちらで！

https://github.com/tsuzukihashi/flutter-tutorial-part01

今回の作業ディレクトリは以下です。

https://github.com/tsuzukihashi/fulutter-tutorial-part02


# step 0 migrate
最近Flutter2が発表されたので、チュートリアルでもmigrateについて記述がありました。

part01で行った`pubspec.yaml`を修正します。

`null safety`に対応するため`english_words`のバージョンをあげます。

```yaml
  english_words: ^4.0.0-0 
```
`pub get`した後に以下のコマンドを入力し、マイグレートします。

```
dart migrate --apply-changes
```

ここまでで一旦アプリを起動してListViewが表示されていることを確認します。

![star](https://lh3.googleusercontent.com/pw/ACtC-3dcDVq94drOSZq8H_ErU6Ij2chHcgwgaPCULOzdAcm8YH6xCjCKNdkBl7010pgWkpcsNcNoks_Ls6_7TMiDLFpOAvxBhMOWg3wpyIzvFJ1lzJORYJTj4OxBpSezunxk2bYG9TK7MAyJY4B-kbLjoRgYbw=w736-h1590-no?authuser=0)

# step 1　リストにアイコンを追加する

`_RandomWordsState`に`_saved`という`Set`を追加します。

このSetにはユーザーがお気に入りした単語のペアが入ります。

SetはListと異なり、重複を許さず順番を保持し続けるという特性があります。

次に、_buildRowメソッドないに`alreadySaved`チェックを追加して単語のペアがお気に入りに追加されているかしていないかを確認します。

次に`_buildRow`にハートアイコンを追加します。

`ListTile`の`trailing`にアイコンを追加します。

```dart
  Widget _buildRow(WordPair pair) {
    final alreadySaved = _saved.contains(pair);
    return ListTile(
      title: Text(
        pair.asPascalCase,
        style: _biggerFont,
      ),
      trailing: Icon(
        alreadySaved ? Icons.favorite : Icons.favorite_border,
        color: alreadySaved ? Colors.red : null,
      ),
    );
  }
```

![heart](https://lh3.googleusercontent.com/pw/ACtC-3cVDpwz4s-sOUm9C8jTlmjwt1rKecKrQ_NVs8nJHe2xXIdWscicO1wXus2Zz1YeHHQSwT91D39O3HCbQTn81uuIhEoh8mF56dh-jK6Q-VPEC-sV-iTZzoZECOs8b1v6bg0yUIEBGx535iIh1b96U5qNWA=w736-h1590-no?authuser=0)

# step 2 ハートアイコンをタップ可能にする

これを行うためには`_buildRow`メソッドを更新します！

`ListItem`の`onTap`を実装します。

タイルがタップされたら`setState()`を呼び出し状態が通知されたことをフレームワークに伝える働きがあるそうです。

```dart
    Widget _buildRow(WordPair pair) {
    final alreadySaved = _saved.contains(pair);
    return ListTile(
      title: Text(
        pair.asPascalCase,
        style: _biggerFont,
      ),
      trailing: Icon(
        alreadySaved ? Icons.favorite : Icons.favorite_border,
        color: alreadySaved ? Colors.red : null,
      ),
      onTap: () {
        setState(() {
          if (alreadySaved) {
            _saved.remove(pair);
          } else {
            _saved.add(pair);
          }
        });
      },
    );
```

![image]](https://lh3.googleusercontent.com/pw/ACtC-3fEe__50G17SdWjHd3UYIMj4YXfr3jnJUEYBf6HE2f-a5CGXh6FFHA4vgeTCUd-29_9HlTcXimG8lyj7lzODg7PkxtM8bLZZ1dlpvV6ABPiFmzfBP1pxXAp8PHiO0uNLZqhIQp4DcQcHQsVLDFfqqA5zw=w736-h1590-no?authuser=0)

# step 3 新しい画面に遷移する

このステップではお気に入りを表示するためのページ（Flutterではページのことをルートと呼びます）を作ります。

ホームルートと新しいルートの間を移動する方法を学びます。

FlutterのNavigatorはアプリのルートを含むスタック管理します。

ルートをNavigatorのスタックにプッシュすると、そのルートが表示され、ルートをポップすると表示が前のルートの戻ります。

まず、遷移させるための要素として、AppBarにリストボタンを配置します。

`_RandomWordsState`の`build`メソッドの`AppBar`に`actions`を追加します。

```dart
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Startup Name Generator'),
        actions: [IconButton(icon: Icon(Icons.list), onPressed: _onPressed)],
      ),
      body: _buildSuggestions(),
    );
  }
}
```

また、`IconButton`にはタップした際のメソッドが必要なため`_onPressed`メソッドをState内に作っておきます。

```dart
  void _onPressed() {}
```

この時点でボタンをタップしても何も起こりません。

ルートを作成し、Navigatorのスタックにプッシュする処理をかきます。

```dart
  void _onPressed() {
    Navigator.of(context)
        .push(MaterialPageRoute<void>(builder: (BuildContext context) {
      final tiles = _saved.map((WordPair pair) {
        return ListTile(
          title: Text(
            pair.asPascalCase,
            style: _biggerFont,
          ),
        );
      });
      final divied =
          ListTile.divideTiles(tiles: tiles, context: context).toList();
      return Scaffold(
        appBar: AppBar(
          title: Text('Saved Suggestions'),
        ),
        body: ListView(
          children: divied,
        ),
      );
    }));
  }
```


`Navigator.of(context).push`でルートがスタックにプッシュされます。

次に、`MaterialPageRoute`とそのビルダーを追加します。

ListViewを作るためにListTileを作成し、さらに仕切り線を追加し`toList()`でリストに変換しそれを`ListView`に食わせます。

アプリを再起動し、セルをタップしAppBarのリストアイコンをタップするとタップした単語のペアのリストを確認することができます。

![navigator](https://lh3.googleusercontent.com/pw/ACtC-3f4ekzMyijRTvwPG5Cc9pEqjUWQHYf8DtsInjKJ2f-2uaFzm4mD0_5a9I0NPRIWSDdv1o0XY6ZJN8lZk3vE9WBBp3A5iDlv7XfIL0noGSyPOFOQbDusx9-7Z9-ulGYAVi223zFgwOZaURW1luiE2RCDpA=w736-h1590-no?authuser=0)

また、何もしなくても戻るボタンができていることが確認することができます。

# step 4 テーマを変更する

このステップではアプリのテーマを変更します。テーマを変更すると、外観と雰囲気を帰ることができます。

`ThemeData`クラスを変更することで簡単にテーマを変更することができます。

以下のように`MyApp`クラスのthemeを変更してprimaryColorを白色に変更します。

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Startup Name Generator',
      theme: ThemeData(primaryColor: Colors.white),
      home: RandomWords(),
    );
  }
}
```

![white](https://lh3.googleusercontent.com/pw/ACtC-3ea0-RD3wWE43CDqTWVbeDDhjWYp_vQTd-pYBqksf3GPaBQBqxt3Yl4YCzfaKNT_Fk1_OD9qJtdCtsHCsMKAOLv7dKLkqPtakCYSQul4JqI4wiMBlwaVVLzEdHisRsw1-PLNn9Zy3B2t_Dys_NMCW69xQ=w736-h1590-no?authuser=0)

いろんな色を試してみたいときに、いちいちビルドし直さず、ホットリロードで反映されるためUIの変更を迅速に確認することができます。

以上で終わりです！

