# ゼロから始めるJavaScript生活

[![Build Status](https://travis-ci.org/verekia/js-stack-from-scratch.svg?branch=master)](https://travis-ci.org/verekia/js-stack-from-scratch)
[![Release](https://img.shields.io/github/release/verekia/js-stack-from-scratch.svg?style=flat-square)](https://github.com/verekia/js-stack-from-scratch/releases)
[![Dependencies](https://img.shields.io/david/verekia/js-stack-boilerplate.svg?style=flat-square)](https://david-dm.org/verekia/js-stack-boilerplate)
[![Dev Dependencies](https://img.shields.io/david/dev/verekia/js-stack-boilerplate.svg?style=flat-square)](https://david-dm.org/verekia/js-stack-boilerplate?type=dev)
[![Gitter](https://img.shields.io/gitter/room/js-stack-from-scratch/Lobby.svg?style=flat-square)](https://gitter.im/js-stack-from-scratch/)

[![React](/img/react-padded-90.png)](https://facebook.github.io/react/)
[![Redux](/img/redux-padded-90.png)](http://redux.js.org/)
[![React Router](/img/react-router-padded-90.png)](https://github.com/ReactTraining/react-router)
[![Flow](/img/flow-padded-90.png)](https://flowtype.org/)
[![ESLint](/img/eslint-padded-90.png)](http://eslint.org/)
[![Jest](/img/jest-padded-90.png)](https://facebook.github.io/jest/)
[![Yarn](/img/yarn-padded-90.png)](https://yarnpkg.com/)
[![Webpack](/img/webpack-padded-90.png)](https://webpack.github.io/)
[![Bootstrap](/img/bootstrap-padded-90.png)](http://getbootstrap.com/)

モダンJavaScriptスタックチュートリアル、**ゼロから始めるJavaScript生活**へようこそ。

> 🎉**これは2016年に公開した初版を改訂した第2版です。[Change Log](/CHANGELOG.md)もどうぞ!**

これはJavaScriptスタックを使い始めるための最速ガイドです。このガイドは一般的なプログラミングの知識とJavaScriptの基礎を前提としています。**これら全てのツールを一緒につなぎ合わせることにフォーカスしており**、各ツールについて**可能な限りシンプルな例**を提供します。このチュートリアルは、**独自のボイラープレートをゼロから準備するための方法**として見ることもできます。このチュートリアルのゴールはさまざまなツールを組み合わせるところまでで、各ツールの詳細をそれぞれ紹介することはしません。より詳しい知識を得るためにはそれぞれのツールのドキュメントにあたるか、他のチュートリアルを探してください。

もちろん、ちょっとしたJSによるインタラクションを含むだけのシンプルなウェブページを作るのであれば、ここで紹介するスタックを全て使う必要はありません(ES6コードを複数のファイルに書きたいのであれば、Browserify/Webpack + Babel + jQueryの組み合わせで十分です)。しかし、スケールするウェブアプリを構築するにあたってセットアップの手助けが必要であれば、このチュートリアルが重宝するでしょう。

このチュートリアルで説明しているスタックはその大部分にReactを使用しています。もし初心者の方がReactについてのみ学びたいのであれば、[create-react-app](https://github.com/facebookincubator/create-react-app)で設定済みのReact環境をすぐに試すことができます。例えばすでにReactを使っているチームに加わり、学習用playgroundでキャッチアップするということであれば、このようなアプローチをお勧めします。このチュートリアルでは、内部で何が行っているかをすべて理解してもらえるよう、既存の設定を使わないようにしています。

コード例は章ごとに用意しており、それぞれ`yarn && yarn start`で実行できます。とはいえ、各章の**ステップバイステップの指示**に従って全てゼロから入力することを推奨します。

最終的なコードは[JS-Stack-Boilerplate repository](https://github.com/verekia/js-stack-boilerplate)と[releases](https://github.com/verekia/js-stack-from-scratch/releases)から入手可能です。[live demo](https://js-stack.herokuapp.com/)もあります。

これはLinuxとmacOS、そしてWindowsで動作します。

## 目次

[01 - Node, Yarn, `package.json`](/tutorial/01-node-yarn-package-json.md#readme)

[02 - Babel, ES6, ESLint, Flow, Jest, Husky](/tutorial/02-babel-es6-eslint-flow-jest-husky.md#readme)

[03 - Express, Nodemon, PM2](/tutorial/03-express-nodemon-pm2.md#readme)

[04 - Webpack, React, HMR](/tutorial/04-webpack-react-hmr.md#readme)

[05 - Redux, Immutable, Fetch](/tutorial/05-redux-immutable-fetch.md#readme)

[06 - React Router, Server-Side Rendering, Helmet](/tutorial/06-react-router-ssr-helmet.md#readme)

[07 - Socket.IO](/tutorial/07-socket-io.md#readme)

[08 - Bootstrap, JSS](/tutorial/08-bootstrap-jss.md#readme)

[09 - Travis, Coveralls, Heroku](/tutorial/09-travis-coveralls-heroku.md#readme)

## 今後の予定

エディタの設定(まずはAtom)、MongoDB、プログレッシブWebアプリケーション。

## 翻訳

翻訳を追加したい場合、[translation recommendations](/how-to-translate.md)を読んでから始めてください!

### V2

すぐにここに載せます ;)

Check out the [ongoing translations](https://github.com/verekia/js-stack-from-scratch/issues/147).

### V1

- [中文](https://github.com/pd4d10/js-stack-from-scratch) by [@pd4d10](http://github.com/pd4d10)
- [Italiano](https://github.com/fbertone/js-stack-from-scratch) by [Fabrizio Bertone](https://github.com/fbertone)
- [日本語](https://github.com/takahashim/js-stack-from-scratch) by [@takahashim](https://github.com/takahashim)
- [Русский](https://github.com/UsulPro/js-stack-from-scratch) by [React Theming](https://github.com/sm-react/react-theming)
- [ไทย](https://github.com/MicroBenz/js-stack-from-scratch) by [MicroBenz](https://github.com/MicroBenz)

## Credits

Created by [@verekia](https://twitter.com/verekia) – [verekia.com](http://verekia.com/).

License: MIT
