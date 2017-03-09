# 01 - Node, Yarn, and `package.json`

この章ではNode、Yarn、そして基本的な`package.json`ファイルとパッケージのセットアップを行います.

## Node

> 💡 **[Node.js](https://nodejs.org/)** はJavaScriptの実行環境です。Nodeはバックエンドの開発だけではなく、一般的なスクリプティングにも使われています。フロントエンド開発においては、整形(linting)、テスティング、ファイルのアセンブリングといった様々なタスクに使われます。

このチュートリアルではあらゆるところにNodeを使っているため、Nodeをインストールする必要があります。**macOS** と **Windows** 用バイナリの[ダウンロードページ](https://nodejs.org/en/download/current/)に行くか、Linuxディストリビューション用の[package manager installations page](https://nodejs.org/en/download/package-manager/)に行きます。

たとえば、**UbuntuかDebian** であれば、Nodeをインストールするために以下のコマンドを実行します。

```sh
curl -sL https://deb.nodesource.com/setup_7.x | sudo -E bash -
sudo apt-get install -y nodejs
```

Node のバージョンは6.5.0以降であればどれでも構いません。

## NVM

Nodeがすでにインストールされている場合、あるいはより高い柔軟性を求める場合、NVM ([Node Version Manager](https://github.com/creationix/nvm)) をインストールし、NVM を使って最新のNodeをインストールしてください。

## NPM

NPMはNodeのデフォルトのパッケージマネージャーで、Nodeと合わせて自動的にインストールされます。パッケージマネージャーはパッケージ(あなたや第三者の書いたコードのモジュール)をインストールして管理するために使われます。このチュートリアルではたくさんのパッケージを使いますが、ここではもう一つのパッケージマネージャー、Yarnを使います。

## Yarn

> 💡 **[Yarn](https://yarnpkg.com/)** はNode.jsのパッケージマネージャーで、NPMよりも高速であり、オフライン環境にも対応し、依存しているものを[より期待通りに](https://yarnpkg.com/en/docs/yarn-lock)取得します。


Yarnは2016年10月に[リリース](https://code.facebook.com/posts/1840075619545360)されて以来、パッケージマネージャの新たな選択肢としてJavaScriptコミュニティに急速に受け入れられつつあります。NPMを使いたい場合は、`yarn add`や`yarn add --dev`などのコマンドを、`npm install --save`や`npm install --save-dev`などと読み替えてください。


- お使いのOSの[インストール手順](https://yarnpkg.com/en/docs/install)に従ってYarnをインストールします。

## `package.json`

> 💡 **[package.json](https://yarnpkg.com/en/docs/package-json)** はJavaScriptプロジェクトを記述・設定するのに使われるファイルです。このファイルは一般的な情報(プロジェクト名、バージョン、コントリビューター、ライセンスなど)や、使用するツールの設定オプション、さらに *tasks* を実行するためのセクションも含んでいます。

- 新しい作業用フォルダを作り、`cd`で移動します。
- `yarn init`を実行し、質問に答えると(`yarn init -y`で全ての質問をスキップできます)、`package.json`ファイルが自動生成されます。

以下は基本的な`package.json`で、このチュートリアルで使っていきます:

```json
{
  "name": "your-project",
  "version": "1.0.0",
  "license": "MIT"
}
```

## Hello World

- `console.log('Hello world')`と書かれた`index.js`ファイルを作ります。

🏁 そのフォルダ内で`node .`と実行します(`index.js`はNodeがカレントフォルダ内で探すデフォルトのファイル名です)。"Hello world"と表示されるはずです。

**Note**: レーシングフラグの絵文字 🏁 が見えますか? これは **チェックポイント** に来た時に使います. 今後たくさんの変更を行いますが、次のチェックポイントまでコードが動かなくなるかもしれません.

## `start` スクリプト

`node .`でプログラムを実行させるのはやや低レベル寄りです。代わりにNPM/Yarnスクリプトを使ってコードを実行します。より複雑なプログラムを実行する場合でも、必ず`yarn start`で実行できるようにするのは、適切な抽象化になっています。

- `package.json`内に、次のような`scripts`オブジェクトを追加します:

```json
{
  "name": "your-project",
  "version": "1.0.0",
  "license": "MIT",
  "scripts": {
    "start": "node ."
  }

}
```

`start`はこのプログラムを動かす *task* につけられた名前です。このチュートリアルを通じて、この`scripts`オブジェクト内にはたくさんの異なるタスクが作られます。`start`はアプリケーションのデフォルトのタスクにつけられる典型的な名前です。他の標準的なタスク名としては`stop`や`test`があります。

`package.json`は構文的に正しいJSONファイルでなければならないので、末尾のカンマを追加してはいけません。`package.json`ファイルを手で編集する時はよく注意してください。

🏁 `yarn start`と実行します。`Hello world`と表示されるはずです。

## Git と `.gitignore`

- `git init`と入力してGitレポジトリを初期化します。

- `.gitignore`ファイルを作り、以下の内容を追加します。

```gitignore
.DS_Store
/*.log
```

`.DS_Store` ファイルはmacOSが自動生成するファイルで、リポジトリには決して入れないようにします。

`npm-debug.log` と `yarn-error.log` はパッケージマネージャーがエラーに遭遇した際に作られるファイルで、こちらもリポジトリには入れたくないファイルです。

## インストールとパッケージの使用

この章では、パッケージをインストールして使ってみます。「パッケージ」とは他の誰かが書いたコードの集まりのことで、これから書くコード内で利用することができます。どんなコードでもです。ここでは、例として色を扱うためのパッケージを使ってみます。

- コミュニティ製のパッケージ`color`をインストールするため、`yarn add color`と実行します。

`package.json`を開くと、Yarnが`dependencies`に`color`を自動的に追加したのがわかります。

そして`node_modules`フォルダが作られ、そこにパッケージが保存されます。

- `.gitignore`ファイルに`node_modules/`を追加します。

`yarn.lock`というファイルをYarnが生成したことにも気づくでしょう。このファイルはリポジトリにコミットしておくべきです。こうしておけば、チーム内のメンバー全員が同じバージョンのパッケージを利用するようになります。もしYarnではなくNPMを使いたい場合は、*shrinkwrap*がこのファイルの代わりとなります。

- `index.js` フィルに以下のように書きます:

```js
const color = require('color')

const redHexa = color({ r: 255, g: 0, b: 0 }).hex()

console.log(redHexa)
```

🏁 `yarn start`と実行すると、`#FF0000`と表示されるはずです。

おめでとうございます、パッケージをインストールして使うことができました!

`color`はこの章の中でしか使わない、パッケージの使い方を説明するためだけのシンプルなパッケージです。もう不要になったので、アンインストールして構いません。

- `yarn remove color`を実行します

## 2種類の依存関係

パッケージの依存関係には、`"dependencies"`と`"devDependencies"`の2種類があります。

**Dependencies** はアプリケーションを実行するために必要なライブラリです (React, Redux, Lodash, jQueryなど)。 `yarn add [package]`でインストールします。

**Dev Dependencies** は開発やビルドしている間だけ使うパッケージです(Webpack, SASS, linters, テスティングフレームワークなど)。`yarn add --dev [package]`でインストールします。

(原文: [01 - Node, Yarn, and package.json](https://github.com/verekia/js-stack-from-scratch/blob/master/tutorial/01-node-yarn-package-json.md))

次章: [02 - Babel, ES6, ESLint, Flow, Jest, Husky](/tutorial/02-babel-es6-eslint-flow-jest-husky)

[目次](https://github.com/takahashim/js-stack-from-scratch#table-of-contents)に戻る

