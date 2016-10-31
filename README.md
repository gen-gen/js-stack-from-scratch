# ゼロから始めるJavaScript生活

[![Yarn](/img/yarn.png)](https://yarnpkg.com/)
[![React](/img/react.png)](https://facebook.github.io/react/)
[![Gulp](/img/gulp.png)](http://gulpjs.com/)
[![Redux](/img/redux.png)](http://redux.js.org/)
[![ESLint](/img/eslint.png)](http://eslint.org/)
[![Webpack](/img/webpack.png)](https://webpack.github.io/)
[![Mocha](/img/mocha.png)](https://mochajs.org/)
[![Chai](/img/chai.png)](http://chaijs.com/)
[![Flow](/img/flow.png)](https://flowtype.org/)

[![Build Status](https://travis-ci.org/verekia/js-stack-from-scratch.svg?branch=master)](https://travis-ci.org/verekia/js-stack-from-scratch)

モダンJavaScriptスタックチュートリアル、**ゼロから始めるJavaScript生活** へようこそ。

これはJavaScriptスタックを使い始めるための最短最速のガイドです。**ES6, Babel, Gulp, ESLint, React, Redux, Webpack, Immutable, Mocha, Chai, Sinon, Flow** のセットアップ方法をお伝えします。
このガイドは一般的なプログラミングの知識とJavaScriptの基礎を前提としています。これら全てのツールを一緒につなぎ合わせることにフォーカスしており、各ツールについて**可能な限りシンプルな例**を提供します。このチュートリアルは、独自のボイラープレートをゼロから準備するための方法として見ることもできます。

このチュートリアルの目標はさまざまなツールを組み立てることにあるため、これらのツールの仕組みについて、それぞれの詳細については触れません。
それらのより深い知識を習得したい場合、それぞれのドキュメントを参照するか、他のチュートリアルを探してください。

このチュートリアルで説明しているスタックはその大部分にReactを使用しています。Reactのチュートリアルの多くは、セットアップの部分を完全に省略しており、新規参入者は基礎がしっかりしていない中での学習を強いられています。初心者に「ブラックボックス」の設定を与えるのとは異なり、ここで行っているアプローチでは、徹底理解のために、可能な限り簡単な方法によるスタックのセットアップを行います。

コード例は章ごとに利用できます。それぞれ `yarn && yarn start` または `npm install && npm start` で実行できます。各章の **ステップバイステップの指示** に従って全てゼロから入力することを推奨します。


**すべての章には、その前の章のコードを含んでいます** 。そのため、全てを含んだプロジェクトのボイラープレートを探しているのであれば、最終章をcloneすれば入手できます。

注意: 章の順序は必ずしも最も教育的なものではありません。たとえば、テスティング/型検査はReactを導入する前に行うことできます。しかしながら、章を移動させたり以前の章を修正するのは、以降の章を全て変更する必要があるため、かなり困難です。いったん状況が落ち着いた場合、より良い方法のために配置を変更するかもしれません。

このチュートリアルのコードはLinux、macOS、Windows上で動作します。


## 目次

[1 - Node と NPM, Yarn, そして package.json](/tutorial/1-node-npm-yarn-package-json) （翻訳完了）

[2 - packageのインストールと使用](/tutorial/2-packages)（翻訳完了）

[3 - Babel と Gulp による ES6 のセットアップ](/tutorial/3-es6-babel-gulp)（翻訳完了）

[4 - ES6 構文によるクラスの使い方](/tutorial/4-es6-syntax-class)（翻訳完了）

[5 - ES6モジュールの構文](/tutorial/5-es6-modules-syntax)（翻訳完了）

[6 - ESLint](/tutorial/6-eslint)（翻訳完了）

[7 - Webpackによるクライアントアプリ](/tutorial/7-client-webpack)

[8 - React](/tutorial/8-react)

[9 - Redux](/tutorial/9-redux)

[10 - Immutable JS と Redux の改良](/tutorial/10-immutable-redux-improvements)

[11 - MochaとChai, Sinonによるテスティング](/tutorial/11-testing-mocha-chai-sinon)

[12 - Flowによる型検査](/tutorial/12-flow)

## 今後の予定

本番/開発環境, Express, React Router, サーバサイドレンダリング, Styling, Enzyme, Gitフック。

## 翻訳

- [Chinese](https://github.com/pd4d10/js-stack-from-scratch) by [@pd4d10](http://github.com/pd4d10)
- [Japanese](https://github.com/takahashim/js-stack-from-scratch) by [@takahashim](http://github.com/takahashim)

翻訳を追加したい場合、[推奨翻訳方法](/how-to-translate.md)を読んでから始めてください!

## Credits

Created by [@verekia](https://twitter.com/verekia) – [verekia.com](http://verekia.com/).

License: MIT
