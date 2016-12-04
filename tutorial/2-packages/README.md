# 2 - パッケージのインストールと利用

この章では、パッケージをインストールして使ってみます。「パッケージ」とは他の誰かが書いたコードの集まりのことで、これから書くコード内で利用することができます。どんなコードでもです。ここでは、例として色を扱うためのパッケージを使ってみます。

- コミュニティ製のパッケージ`color`をインストールするため、`yarn add color`と実行します。

`package.json`を開くと、Yarnが`dependencies`に`color`を自動的に追加したのがわかります。

そして`node_modules`フォルダが作られ、そこにパッケージが保存されます。

- `.gitignore`ファイルに`node_modules/`を追加します(まだ`git init`を実行していなければここで実行します)。

`yarn.lock`というファイルをYarnが生成したことにも気づくでしょう。このファイルはリポジトリにコミットしておくべきです。こうしておけば、チーム内のメンバー全員が同じバージョンのパッケージを利用するようになります。もしYarnではなくNPMを使いたい場合は、*shrinkwrap*がこのファイルの代わりとなります。

- `index.js`ファイルに`const Color = require('color');`と追加します。
- 例として以下のような形でパッケージを使ってみます: `const redHexa = Color({r: 255, g: 0, b: 0}).hex();`
- `console.log(redHexa)`を追加します。
- `yarn start`と実行すると、`#FF0000`と表示されるはずです。

おめでとうございます、パッケージをインストールして使うことができました!

`color`はこの章の中でしか使わない、パッケージの使い方を説明するためだけのシンプルなパッケージです。もう不要になったので、アンインストールして構いません。

- `yarn remove color`を実行します

**注意**: パッケージの依存関係には、`"dependencies"`と`"devDependencies"`の2種類あります。`"dependencies"`は`"devDependencies"`より一般的なもので、後者は開発している間だけ使うパッケージであり、本番環境では使いません（典型的には、ビルド関係のパッケージや、linterなどがあります）。`"devDependencies"`に追加するには、`yarn add --dev [package]`と実行します。

(原文: [2 - Installing and using a package](https://github.com/verekia/js-stack-from-scratch/tree/master/tutorial/2-packages))

次章: [3 - ES6をBabelとGulpを使ってセットアップ](/tutorial/3-es6-babel-gulp)

[前章](/tutorial/1-node-npm-yarn-package-json)または[目次](https://github.com/verekia/js-stack-from-scratch#table-of-contents)に戻る
