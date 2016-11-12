# 1 - Node、NPM、Yarn、そしてpackage.json

この章ではNode、NPM、Yarn、そして基本的な`package.json`ファイルのセットアップを行います.

まず、バックエンドのJavaScriptとして使われるだけではなく、モダンなフロントエンドスタックを構築するためのツールとしても使われる、Nodeをインストールする必要があります。

macOSとWindowsバイナリ用[ダウンロードページ](https://nodejs.org/en/download/current/)に行くか、Linuxディストリビューション用の[package manager installations page](https://nodejs.org/en/download/package-manager/)に行きます。

たとえば、**UbuntuかDebian** であれば、Nodeをインストールするために以下のコマンドを実行します。

```bash
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install -y nodejs
```

Node のバージョンは6.5.0以降であればどれでも構いません。

Nodeの標準パッケージマネージャー、`npm`はNodeに付属しているため、別途インストールする必要はありません。

**注意**: Nodeがすでにインストールされている場合、`nvm`([Node Version Manager](https://github.com/creationix/nvm)) をインストールし、`nvm`を使って最新のNodeをインストールしてください。

[Yarn](https://yarnpkg.com/)はNPMよりも高速なもう一つのパッケージマネージャで、オフライン環境にも対応し、依存しているものを[より期待通りに](https://yarnpkg.com/en/docs/yarn-lock)取得します。Yarnは2016年10月に[リリース](https://code.facebook.com/posts/1840075619545360)されて以来、パッケージマネージャの新たな選択肢としてJavaScriptコミュニティに急速に受け入れられつつあります。このチュートリアルではYarnを使っています。NPMを使いたい場合は、`yarn add`や`yarn add --dev`などのコマンドを、`npm install --save`や`npm install --dev`などと読み替えてください。

- [インストール手順](https://yarnpkg.com/en/docs/install)に従ってYarnをインストールします。`npm install -g yarn`や`sudo npm install -g yarn`などでインストールすることもできます（そうです、YarnをインストールするのにNPMを使うのです。Internet ExplorerやSafariを使ってChromeをインストールするようなものですね!）

- 新しい作業用フォルダを作り、`cd`で移動します。
- `yarn init`を実行し、質問に答えると(`yarn init -y`で全ての質問をスキップできます)、`package.json`ファイルが自動生成されます。
- `console.log('Hello world')`と書かれた`index.js`ファイルを作ります。
- そのフォルダ内で`node .`と実行します(`index.js`はNodeがカレントフォルダ内で探すデフォルトのファイル名です)。"Hello world"と表示されるはずです。

`node .`でプログラムを実行させるのはやや低レベル寄りです。代わりにNPM/Yarnスクリプトを使ってコードを実行します。より複雑なプログラムを実行する場合でも、必ず`yarn start`で実行できるようにするのは、適切な抽象化になっています。

- `package.json`内の`scripts`オブジェクトに`"start": "node ."`と追加します。

```json
"scripts": {
  "start": "node ."
}
```

`package.json`は構文的に正しいJSONファイルでなければならないので、末尾のカンマを追加してはいけません。`package.json`ファイルを手で編集する時はよく注意してください。

- `yarn start`と実行します。`Hello world`と表示されるはずです。

- `.gitignore`を作り、以下の内容を追加します。

```gitignore
npm-debug.log
yarn-error.log
```

**注意**: 私が作った`package.json`ファイル内には、各章に対して`tutorial-test`スクリプトがあります。このスクリプトは`yarn && yarn start`で各章が正しく実行されることを私自身がテストするためのものです。他のプロジェクトに使う場合は、ここは削除しても構いません。

(原文: [1 - Node, NPM, Yarn, and package.json](https://github.com/verekia/js-stack-from-scratch/tree/master/tutorial/1-node-npm-yarn-package-json))

次章: [2 - パッケージのインストールと使用](/tutorial/2-packages)

[目次](https://github.com/verekia/js-stack-from-scratch#table-of-contents)に戻る
