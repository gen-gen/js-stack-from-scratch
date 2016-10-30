# 2 - packageのインストールと使用

この章では、packageのインストールと利用について説明します。"package"とは他の誰かが書いたコードの集まりのことで、あなたが書いたコード内で利用することができます。どんなコードでもです。ここでは、例として色を扱うためのパッケージを使ってみます。

- コミュニティ製のパッケージの `color` をインストールするため、 `yarn add color`と実行します。

`package.json`を開くと、Yarnが`dependencies`に`color`を自動的に追加したのがわかります。

そして`node_modules`フォルダが作られ、そこのpackageが保存されます。

- `.gitignore`ファイルに`node_modules/`を追加します(そして `git init` を、まだ実谷していなければここで実行します)。

`yarn.lock`というファイルをYarnが生成したことにも気づくでしょう。このファイルはリポジトリにコミットしておくべきです。そうしておけば、あなたのチームのメンバー全員が同じバージョンのパッケージを利用できるようになります。もしYarnではなくNPMを使いたい場合は、*shrinkwrap*がこのファイルの代わりとなります。

- `index.js`ファイルに`const Color = require('color');`と追加します。

- 例として以下のような形でパッケージを使ってみます: `const redHexa = Color({r: 255, g: 0, b: 0}).hexString();`
- `console.log(redHexa)`を追加します。
- `yarn start`と実行すると、`#FF0000`と表示されるはずです。

おめでとうございます、packageをインストールして使うことができました!

`color`はこの章の中で、packageの簡単な使い方を説明するためにしか使いません。もう不要になったので、アンインストールして構いません。

- `yarn remove color`を実行します

**注意**: パッケージの依存関係は、`"dependencies"` と `"devDependencies"`の2種類あります。`"dependencies"` は `"devDependencies"`より一般的なもので、後者は開発中のみに使うパッケージで、本番環境では使われないものです（典型的には、ビルド関係のpackageや、linterなどがあります）。`"devDependencies"`に追加するには、`yarn add --dev [package]`と実行します。


原文: [2 - Installing and using a package](https://github.com/verekia/js-stack-from-scratch/tree/master/tutorial/2-packages)

次章: [3 - Setting up ES6 with Babel and Gulp](/tutorial/3-es6-babel-gulp)

[前章](/tutorial/1-node-npm-yarn-package-json) または [目次](https://github.com/verekia/js-stack-from-scratch)に戻る
