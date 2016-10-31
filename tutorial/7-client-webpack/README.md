# 7 - Client App with Webpack

## アプリの構造

- プロジェクトのルートに`dist`フォルダを作り、以下の`index.html`ファイルを追加します:

```html
<!doctype html>
<html>
  <head>
  </head>
  <body>
    <div class="app"></div>
    <script src="client-bundle.js"></script>
  </body>
</html>
```

`src`フォルダには、次のサブフォルダを作ります: `server`, `shared`, `client`。そしてカレントの`index.js`を`server`フォルダに、`dog.js`を`shared`フォルダに移動しいます。`client`フォルダには`app.js`ファイルを作ります。

Nodeバックエンドは使わないのですが、この分割はそれぞれの所属をはっきりさせるのに役立ちます。`server/index.js`内の`import Dog from './dog';`を`import Dog from '../shared/dog';`に変更する必要がありますが、ESLintは解決できないモジュールのエラーを検出してくれます。

`client/app.js`ファイルに以下を書きます:

```javascript
import Dog from '../shared/dog';

const browserToby = new Dog('Browser Toby');

document.querySelector('.app').innerText = browserToby.bark();
```

`package.json`の`eslintConfig`に以下を追加します:

```json
"env": {
  "browser": true
}
```
こうすると、`window`や`document`といったブラウザでは必ずアクセスできる変数を使っても、ESLintは未定義変数の警告を出さないようになります。

`Promise`など、最新のESの仕様を使いたい場合には、[Babel Polyfill](https://babeljs.io/docs/usage/polyfill/)をクライアントコードに含める必要があります。

- `yarn add babel-polyfill`を実行する

そして`app.js`の先頭に、importを追加します:

```javascript
import 'babel-polyfill';
```

このpolyfillを使うと、バンドルのサイズが増えるため、これらの機能を使いたい場合のみ追加するようにします。このチュートリアルでは無駄のないボイラープレートのコードを提供するために、このpolyfillを導入し、次章以降のコードサンプルで使っていきます。

## Webpack

Node環境では、いろんなファイルを自由に`import`しても、Nodeはファイルシステムを使って適切に解決してくれました。ブラウザにはファイルシステムが持たないため、`import`もファイルを参照することができません。エントリポイントのファイルである`app.js`が必要なインポートの連関を探してこれるようになるため、
「バンドル」

In a Node environment, you can freely `import` different files and Node will resolve these files using your filesystem. In a browser, there is no filesystem, and therefore your `import`s point to nowhere. In order for our entry point file `app.js` to retrieve the tree of imports it needs, we are going to "bundle" that entire tree of dependencies into one file. Webpack is a tool that does this.


WebpackはGulpのように、`webpack.config.js`という設定ファイルを使用します。GulpがBabelを利用していたのと全く同じように、ES6のimportとexportが使えるようにできます。そのためには、`webpack.config.babel.js`という名前のファイルを使います。

- 空のファイル`webpack.config.babel.js`を作ります

- Gulpの`link`タスクに`webpack.config.babel.js`を、`paths`定数にいくつかパスを追加します。

```javascript
const paths = {
  allSrcJs: 'src/**/*.js',
  gulpFile: 'gulpfile.babel.js',
  webpackFile: 'webpack.config.babel.js',
  libDir: 'lib',
  distDir: 'dist',
};

// [...]

gulp.task('lint', () =>
  gulp.src([
    paths.allSrcJs,
    paths.gulpFile,
    paths.webpackFile,
  ])
    .pipe(eslint())
    .pipe(eslint.format())
    .pipe(eslint.failAfterError())
);
```

WebpackにBabelを使ってES6ファイルを扱う方法を教える必要があります(GulpにES6ファイルの扱いを教えるために`gulp-babel`を使ったのと同様です)。Webpackでは、素の古いJavaScriptではないものを扱う場合、*loaders*を使います。Webpack用のBabel loaderをインストールしてみましょう。

- `yarn add --dev babel-loader`を実行します

- `webpack.config.babel.js`ファイルに以下を書きます:

```javascript
export default {
  output: {
    filename: 'client-bundle.js',
  },
  devtool: 'source-map',
  module: {
    loaders: [
      {
        test: /\.jsx?$/,
        loader: 'babel-loader',
        exclude: [/node_modules/],
      },
    ],
  },
  resolve: {
    extensions: ['', '.js', '.jsx'],
  },
};
```

これを解読してみます:

このファイルはWebpackが読めるように`export`する必要があります。`output.filename`が生成したいバンドルのファイル名です。`devtool: 'source-map'`はブラウザでのデバッグが捗るようにするためのソースマップを有効にします。`module.loaders`には`test`があります。これは`babel-loader`がどんなファイルを扱うかテストするかを正規表現で指定しています。次章では`.js`ファイルと(React用の)`.jsx`ファイルの両方を扱うため、`/\.jsx?$/`という正規表現を使っています。`node_modules`フォルダはトランスパイルを行わないため、excludeで排除しています。`resolve`の部分では、拡張子なしで`import`した場合、どのような種類のファイルを`import`するべきかをWebpackに指定しています。これは例えば`import Foo from './foo'`と書くと、`foo`のところは`foo.js`や`foo.jsx`と解釈されるようにするためのものです。

さて、Webpackの設定は終わりましたが、*実行*するにはもうちょっとかかります。

## WebpackをGulpに統合する

Webpackはさまざまなことを行います。プロジェクトがクライアントサイドのものであれば、Gulpをすっかり置き換えてしまうこともできます。
一方で、Gulpはもっと汎用的なツールで、linting、テスト、バックエンドタスクなどに向いています。
初学者の人にとっては、Webpackの複雑な設定よりもシンプルで理解しやすいでしょう。
ここではすでにGulpのセットアップとワークフローを構築済みなので、WebpackをGulpのビルドに統合させるのが理想的でしょう。

それではWebpackを実行するGulpタスクを作りましょう。`gulpfile.babel.js`を開きます。

`node lib/`を実行する`main`タスクはもう必要ありません。`index.html`を開けばアプリが実行されるためす。

- `import { exec } from 'child_process'`を削除します

Gulpプラグイン同様、`webpack-stream`パッケージを使えば、GulpにWebpackに統合するのも簡単です。

-  `yarn add --dev webpack-stream`を実行し、パッケージをインストールします

- 以下の`import`を追加します:

```javascript
import webpack from 'webpack-stream';
import webpackConfig from './webpack.config.babel';
```

2行目で設定ファイルを取り込んでいます。

先ほど触れたとおり、次章では`.jsx`ファイルを使います(まず使うのはクライアント側でですが、サーバ側でも後ほど使用します)。ちょっと早いですがさっそく設定しておきましょう。

- constの定数宣言を次のようにします:

```javascript
const paths = {
  allSrcJs: 'src/**/*.js?(x)',
  serverSrcJs: 'src/server/**/*.js?(x)',
  sharedSrcJs: 'src/shared/**/*.js?(x)',
  clientEntryPoint: 'src/client/app.js',
  gulpFile: 'gulpfile.babel.js',
  webpackFile: 'webpack.config.babel.js',
  libDir: 'lib',
  distDir: 'dist',
};
```

`.js?(x)`は`.js`ファイルと`.jsx`ファイルにマッチするパターンです。

アプリケーションの別の部分についてと、エントリポイントのファイルが定数に追加されました。

- `main`を次のように変更します:

```javascript
gulp.task('main', ['lint', 'clean'], () =>
  gulp.src(paths.clientEntryPoint)
    .pipe(webpack(webpackConfig))
    .pipe(gulp.dest(paths.distDir))
);
```

**注意**: `build`タスクは現状、`src`以下にある全ての`.js`ファイルをES6からES5にトランスパイルします。ここではコードを`server`と`shared`、`client`に分割しています。これは`server`と`shared`のみこのタスクでコンパイルするためのものです(`client`はWebpackが担当するたｍです)。しかしながら、テスティングの章では、Webpackの外でテストするため`client`のコードもGulpでコンパイルする必要があります。そのため、その章にたどり着くまで、不要な重複ビルドが行われることになります。今のところはこれでも良いと同意してもらえると思います。実際には、今ここで扱うのはクライアントバンドルのみなので、`build`タスクも`lib`フォルダもその章までは特に使う必要がないものです。

- `yarn start`を実行すると、Webpackが`client-bundle.js`ファイルを作ります。`index.html`をブラウザで開くと"Wah wah, I am Browser Toby"と表示されるはずです。

最後に: `lib`フォルダとは異なり、`dist/client-bundle.js`ファイルと`dist/client-bundle.js.map`ファイルはビルドの度に`clean`タスクで消されません。

- `clientBundle: 'dist/client-bundle.js?(.map)'`を`paths`の設定に追加し、`clean`タスクを以下のように変更します:

```javascript
gulp.task('clean', () => del([
  paths.libDir,
  paths.clientBundle,
]));
```

- `/dist/client-bundle.js*`を`.gitignore`ファイルに追加します。

次章: [8 - React](/tutorial/8-react)

[前章](/tutorial/6-eslint)または[目次](https://github.com/verekia/js-stack-from-scratch)に戻る。
