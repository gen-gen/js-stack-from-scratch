# ゼロから始めるJavaScript生活

[![Build Status](https://travis-ci.org/verekia/js-stack-from-scratch.svg?branch=master)](https://travis-ci.org/verekia/js-stack-from-scratch) [![Join the chat at https://gitter.im/js-stack-from-scratch/Lobby](https://badges.gitter.im/js-stack-from-scratch/Lobby.svg)](https://gitter.im/js-stack-from-scratch/Lobby)

[![Yarn](/img/yarn.png)](https://yarnpkg.com/)
[![React](/img/react.png)](https://facebook.github.io/react/)
[![Gulp](/img/gulp.png)](http://gulpjs.com/)
[![Redux](/img/redux.png)](http://redux.js.org/)
[![ESLint](/img/eslint.png)](http://eslint.org/)
[![Webpack](/img/webpack.png)](https://webpack.github.io/)
[![Mocha](/img/mocha.png)](https://mochajs.org/)
[![Chai](/img/chai.png)](http://chaijs.com/)
[![Flow](/img/flow.png)](https://flowtype.org/)

モダンJavaScriptスタックチュートリアル、**ゼロから始めるJavaScript生活**へようこそ。

> ⚠️️ このチュートリアルのメジャーアップデート版を2月末までに公開する予定です。ご期待下さい! [より詳しく(英語)](https://github.com/verekia/js-stack-from-scratch/issues/133).

これはJavaScriptスタックを使い始めるための最短最速のガイドです。このガイドは一般的なプログラミングの知識とJavaScriptの基礎を前提としています。**これら全てのツールを一緒につなぎ合わせることにフォーカスしており**、各ツールについて**可能な限りシンプルな例**を提供します。このチュートリアルは、**独自のボイラープレートをゼロから準備するための方法**として見ることもできます。

もちろん、ちょっとしたJSによるインタラクションを含むだけのシンプルなウェブページを作るのであれば、ここで紹介するスタックを全て使う必要はありません(ES6コードを複数のファイルに書いてCLIでコンパイルしたいのであれば、Browserify/Webpack + Babel + jQueryの組み合わせで十分です)。しかし、スケールするウェブアプリを構築するにあたってセットアップの手助けが必要であれば、このチュートリアルが重宝するでしょう。

このチュートリアルの目標はさまざまなツールを組み立てることにあるため、これらのツールの仕組みについて、それぞれの詳細については触れません。それらのより深い知識を習得したい場合、それぞれのドキュメントを参照するか、他のチュートリアルを探してください。

このチュートリアルで説明しているスタックはその大部分にReactを使用しています。もし初心者の方がReactについてのみ学びたいのであれば、[create-react-app](https://github.com/facebookincubator/create-react-app)で設定済みのReact環境をすぐに試すことができます。例えばすでにReactを使っているチームに加わり、学習用playgroundでキャッチアップするということであれば、このようなアプローチをお勧めします。このチュートリアルでは、内部で何が行っているかをすべて理解してもらえるよう、既存の設定を使わないようにしています。

コード例は章ごとに利用できます。それぞれ`yarn && yarn start`または`npm install && npm start`で実行できます。各章の**ステップバイステップの指示**に従って全てゼロから入力することを推奨します。

**すべての章には、それまでの章のコードが含まれています** 。そのため、全てを含んだプロジェクトのボイラープレートを探しているのであれば、最終章をcloneすればそれで使えます。

注意: 章の順序は必ずしも最も教育的なものではありません。たとえば、テスティング/型検査はReactを導入する前に行うこともできます。しかしながら、章を移動させたり以前の章を修正するのは、以降の章を全て変更する必要があるため、かなり困難です。いったん状況が落ち着いてから、より良い方法のために配置を変更するかもしれません。

このチュートリアルのコードはLinux、macOS、Windows上で動作します。

## 目次

[1 - NodeとNPM、Yarn、そしてpackage.json](/tutorial/1-node-npm-yarn-package-json)

[2 - パッケージのインストールと使用](/tutorial/2-packages)

[3 - BabelとGulpによるES6のセットアップ](/tutorial/3-es6-babel-gulp)

[4 - ES6のクラス構文を使う](/tutorial/4-es6-syntax-class)

[5 - ES6モジュール構文](/tutorial/5-es6-modules-syntax)

[6 - ESLint](/tutorial/6-eslint)

[7 - Webpackによるクライアントアプリ](/tutorial/7-client-webpack)

[8 - React](/tutorial/8-react)

[9 - Redux](/tutorial/9-redux)

[10 - Immutable JSとReduxの改良](/tutorial/10-immutable-redux-improvements)

[11 - MochaとChai、Sinonによるテスティング](/tutorial/11-testing-mocha-chai-sinon)

[12 - Flowによる型検査](/tutorial/12-flow)

## 今後の予定

本番/開発環境, Express, React Router, サーバサイドレンダリング, Styling, Enzyme, Gitフック。

## 翻訳

- [中文](https://github.com/pd4d10/js-stack-from-scratch) by [@pd4d10](http://github.com/pd4d10)
- [Italiano](https://github.com/fbertone/js-stack-from-scratch) by [Fabrizio Bertone](https://github.com/fbertone)
- [日本語](https://github.com/takahashim/js-stack-from-scratch) by [@takahashim](https://github.com/takahashim)
- [Русский](https://github.com/UsulPro/js-stack-from-scratch) by [React Theming](https://github.com/sm-react/react-theming)
- [ไทย](https://github.com/MicroBenz/js-stack-from-scratch) by [MicroBenz](https://github.com/MicroBenz)

翻訳を追加したい場合、[translation recommendations](/how-to-translate.md)を読んでから始めてください!

### 日本語訳の方針 (by takahashim)

- 英文の更新速度が早いので、英文に追従しやすくする
  - 文の数と順番は維持する
  - Markdownの記法は変更しない（一部改行を追加する）
- 日本語として読みやすくする
  - 語順や節の順番は維持しない
  - `you`は（「あなた」と）訳さない
  - 代名詞は適宜展開する
  - 英単語と日本語の間の空白はトル
  - 英単語の区切りにも`,`は使わず`、`を使う
  - `:`はそのままにする
- テクニカルな用語については原文が透けて見える訳語にする
  - 困ったらカタカナを使う
  - もっと困ったらカッコ書きで原語も書く

## Credits

Created by [@verekia](https://twitter.com/verekia) – [verekia.com](http://verekia.com/).

License: MIT
