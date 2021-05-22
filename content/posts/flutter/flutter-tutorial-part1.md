---
title: "Flutterチュートリアルやってみた part1"
date: 2021-03-04T04:08:10+09:00
draft: false
tags: ["Flutter"]
---

- [参考](https://flutter.dev/docs/get-started/codelab)
- 作業リポジトリ => https://github.com/tsuzukihashi/flutter-tutorial-part01

# 環境
- 作業日: 2020/03/07
- OS: macOS Big Sur 11.2.1
- PC: MacBook Pro (13-inch, M1, 2020)

# 初めに
Flutterの勉強をするには公式のチュートリアルをやるのが一番だという情報を得たのでやっていき！

このチュートリアルでは、リストの無限スクロールをやりました。

# Step1 アプリを作る
Flutterのプロジェクトを新規に作ります。

`lib/main.dart`の中身を一度全て削除し、以下のように画面中央に"Hello World"と表示するように書き換えます。

!['helloworld'](https://lh3.googleusercontent.com/pw/ACtC-3fDMeNtFSaVU3mOiyu-Mmqi_QriPegmSPFx_tuhiAiMLYx4lrZxDLmkp-IgIj1reQ-IOUB8CwTviYVIRxbAtDfj8bxh2FB3TgeNSh91tb3nggQa1Y_KT7azj5VlNyN1AtCssQvBY8ZbYhMzSJzmSkmp-w=w174-h372-no?authuser=0)

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Welcom to Flutter',
      home: Scaffold(
        appBar: AppBar(
          title: Text('Welcome to Flutter'),
        ),
        body: Center(
          child: Text('Hello World'),
        ),
      ),
    );
  }
}

```

コードをコピペした際に、インデントがズレた時の対処法がありましたが、
VSCodeのプラグインが入っていればコードを保存する(⌘ + S)をするたびにフォーマットが走っていました。

| android studio | VS Code | Terminal |
| -- | -- | -- |
| コードを選択し右クリックして\n`Reformat Code with dartfmt` | コードを選択し右クリックして `Format Document` | flutter format `<filename>` | 

Step1まとめ

- マテリアルデザインという標準的なデザインを採用
- main() メソッドは矢印 (=>) 記法を使用し簡潔に描けるようになっている
- アプリは StatelessWidget を拡張し、アプリ自体がウィジェット
- Flutter では、整列、パディング、レイアウトなど、ほとんどすべてがウィジェットになっている

# Step2　外部パッケージを利用する

このステップでは外部パッケージを利用してアプリを作っていくっぽいです。

今回利用するのは、`english_words`という最も多く使われている5000個の英単語といくつかのユーティリティ関数を含むパッケージ
- https://pub.dev/packages/english_words

他の多くのオープンソースパッケージも同様に[pub.dev](https://pub.dev)にあるので、何かパッケージを探したい時はここをチェキすれば良さそう！

パッケージを利用するのに`pabspec.yaml`というファイルをいじる必要があり、

`pubspec.yaml`はFlutterアプリのアセットと依存関係を管理してくれます。

以下のように`dependencies`に使いたいパッケージを記入します。

```pubspec.yaml
name: flutter_tutorial_part01
description: A new Flutter project.
publish_to: 'none' 
version: 1.0.0+1

environment:
  sdk: ">=2.7.0 <3.0.0"

dependencies:
  flutter:
    sdk: flutter
  cupertino_icons: ^1.0.2
  english_words: ^3.1.5

dev_dependencies:
  flutter_test:
    sdk: flutter
  integration_test:
    sdk: flutter

flutter:
  uses-material-design: true
```

`english_words: ^3.1.5`これはenglish_wordsのバージョン3.1.5以上を利用すること示しています。

記入したら、以下のコマンドを打つか、VSCodeならプラグインが入っていればyamlファイルを保存した時点でpub getが走るのでパッケージがプロジェクトに取り込まれます。

```
flutter pub get
```

`pub get` を実行すると、プロジェクトに取り込まれたすべてのパッケージとそのバージョン番号のリストを含む `pubspec.lock`ファイルも自動生成されます。

パッケージを利用していくには、`main.dart`の先頭に以下を追記します。

```
import 'package:english_words/english_words.dart';
```

そうすると、今はまだ使っていないので当たり前なのですが、利用されていませんという表示になっています。

先ほど記述した`Hello World`の代わりに、`english_words`を使ってランダムな英単語を表示させていきます。

![english_words](https://lh3.googleusercontent.com/pw/ACtC-3e70ptDSMg1H5TTvyV-vvPQcrwXHQeA7X93_B58xl-VG2g1Lf2N6pHNQ66HxQrpBIv_-QgBbmAC93YiHuGpwFyGUdojfkIztI8Snp_s7zVVQpkPr5rDTnEMFJ1acbK54yEaiM8dilXfdZ7ijcARKD8NUQ=w174-h372-no?authuser=0)

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final wordPair = WordPair.random();

    return MaterialApp(
      title: 'Welcom to Flutter',
      home: Scaffold(
        appBar: AppBar(
          title: Text('Welcome to Flutter'),
        ),
        body: Center(
          child: Text(wordPair.asPascalCase),
        ),
      ),
    );
  }
}
```

`final wordPair = WordPair.random();`でランダムな単語のペアを生成し、その単語のペアを`wordPair.asPascalCase`でパスカルケース（キャメルケース）で表示しています。

ホットリロードが走るたびに、ランダムな文字列が表示されることが確認できると思います。

# Step3 Stateful widgetの追加

今まで使っていたのはステートレスウィジェットと呼ばれるもので、プロパティは変更できず、不変のものとのこと！

ステートフルウィジェットはウィジェットが生きている間に変更される可能性がある状態を維持し続ける（？）

## Stateful Widgetの実装に必要な2つのもの
1. Stateful Widgetクラス
  - ステートフルウィジェットクラスはそれ自体が不変
  - 捨てたり再生成したりすることができる
2. Stateful Widgetクラスを生成するStateクラス
  - ステートクラスはウィジェットの有効期間中は持続

このステップでは、`RandomWords`を作り、そのステートクラスである`_RandomWordsState`を作成、それを既存の`MyApp`（ステートレスウィジェット）の子として`WordsRandom`を利用します！

main.dartの最後尾にカーソルを移動し、

```
stful
```

と入力すると、以下のようにStateful Widgetを作るために必要なボイラープレートコードを自動生成してくれます！（便利！）

![stful](https://lh3.googleusercontent.com/pw/ACtC-3cN-d4Rc7TX0mV6EWDdB3RQp004f-PMp3QyGl9o0vJ9X13EFBPg8DslFBWMnlZ8jVgmOKUPR5i7fi5rvZl5Y6AmIA7GSIkX7Op-VVN15SIKrjsaxwaTeJdLp_TcQPh2039ErkYLS_jSiw4q1dqfUHIkFQ=w704-h540-no?authuser=0)

このカーソルの状態のまま、ウィジェット名`RandomWords`を入力します。

すると以下のような形になると思います。

```dart
class RandomWords extends StatefulWidget {
  @override
  _RandomWordsState createState() => _RandomWordsState();
}

class _RandomWordsState extends State<RandomWords> {
  @override
  Widget build(BuildContext context) {
    return Container(
      
    );
  }
}
```

`State`クラスの前に`_(アンダースコア)`がついていますが、これはDart言語でプライバシー強化するためのもので、

これはStateオブジェクトのベストプラクティスとして推奨されているようです。

アプリのロジックのほとんどは`_RandomWordsState`にあり、`RandomWords`の状態を保持し続け、生成された単語ペアのリストを保存し、ユーザーがスクロールすると無限に増えていきます。

`_RandomWordsState`の`build`メソッドを以下のように更新します。

```dart
class _RandomWordsState extends State<RandomWords> {
  @override
  Widget build(BuildContext context) {
    final wordPair = WordPair.random();
    return Text(wordPair.asPascalCase);
  }
}
```

そして、今回作った`RandomWords`を`MyApp`で使うように修正します。

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Welcom to Flutter',
      home: Scaffold(
        appBar: AppBar(
          title: Text('Welcome to Flutter'),
        ),
        body: Center(
          child: RandomWords(),
        ),
      ),
    );
  }
}
```

# Step4 無限スクロールするListViewを作る

`_RandomWordsState`を変更し、ユーザーがスクロールするたびに無限に単語のペアを生成し表示させていきます。

`ListView`の`builder factory constror`を利用すると、必要に応じてリストのビューを構築することができます。

`english_words`のランダムで作られた単語のペアを保存するために、`_RandomWordsState`クラスに`_suggestions`というリストと、フォントサイズを大きくするために`_biggerFont`を用意します。

```dart
class _RandomWordsState extends State<RandomWords> {
  final _suggestions = <WordPair>[];
  final _biggerFont = TextStyle(fontSize: 18);
...
```

次に、`_suggestions`をListViewとして表示するために`_buildSuggestions()`というメソッドを追加していきます。

ListViewにはitemBuilderというものがあり、これを実装するとリストを作ることができるそう！

itemBuilderには2つのパラメータ(BuildContextと行イテレータ)が渡されます。

イテレータは0から始まり、関数が呼び出されるたびにインクリメントします。

`_RandomWordsState`クラスに`_buildSuggestions() `を実装していきましょう！

※ここではまだ`_buildRow`を実装していないのでエラーが表示されますが気にせず進みます。

```dart
  Widget _buildSuggestions() {
    return ListView.builder(
      padding: EdgeInsets.all(16.0),
      itemBuilder: /*1*/　(BuildContext context, int i) {
        if (i.isOdd) return Divider();　/*2*/

        final index = i ~/ 2;　/*3*/

        if (index >= _suggestions.length) {
          _suggestions.addAll(generateWordPairs().take(10));　/*4*/
        }
        return _buildRow(_suggestions[index]);
      },
    );
  }
```

/*1*/　itemBuilderのコールバックは生成された単語のペアごとに1回呼び出される。
偶数行の時は、単語のペアを表示する`ListTitle`行を追加
機数行の時は、仕切りの線である`Divider`Widgetを追加

/*2*/ ListViewの各行の前に高さ1pxの仕切り線を追加

/*3*/　`i ~/ 2`は式であり、iを2で割った整数になります。
(ex: i -> 0のとき 0,  i -> 1のとき 0, i -> 2のとき 1, i -> 3のとき 1, i -> 4のとき 2, i -> 5のとき 2)

これにより、`_suggestions`で指定する`index`が`Divider`を除いた数になります。

/*4*/　使用可能な単語のペアの最後に達したら、さらに10個追加して`_suggestions`に追加する

次に、`_buildRow` 関数を作成していきます。

`_RandomWordsState`に追加します。

```dart
  Widget _buildRow(WordPair pair) {
    return ListTile(
      title: Text(
        pair.asPascalCase,
        style: _biggerFont,
      ),
    );
  }
```

これは単語のペアから`ListTitle`を作り、先ほど作った`_biggerFont`を`style`として当てています。

次に、`_RandomWordsState`クラスで単語生成クラスを直接呼び出すのではなく、`_buildSuggestions()`を使用するように`build()`を更新します。

```dart
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Startup Name Generator')),
      body: _buildSuggestions(),
    );
  }
```

最後に`MyApp`クラスを修正します。

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Startup Name Generator',
      home: RandomWords(),
    );
  }
}
```

タイトルを変更し、homeを`RandomWords()`に変更しました。

アプリを再起動してみてください！

動作しているツイートをぶら下げておきます！

https://twitter.com/tsuzuki817/status/1370050258391105538?ref_src=twsrc%5Etfw

以上Flutter Tutorial part1でした！

