# 3 - BabelとGulpによるES6のセットアップ

ES6構文を使ってみましょう。ES6構文は「古い」ES5構文を大幅に改善したものです。全てのブラウザとJS環境はES5を理解しますが、ES6は理解するとは限りません。そのため、Babelと呼ばれる、ES6ファイルをES5ファイルに変換するツールを使います。Babelを実行するには、タスクランナーのGulpを使います。`package.json`ファイルの`scripts`以下に書くタスクに似ていますが、タスクをJSファイルに書く方がJSONファイルに書くよりもシンプルで汚くなりにくいです。そのため、Gulpと、Gulp用のBabelプラグインもインストールしておきましょう:

- `yarn add --dev gulp`と実行します
- `yarn add --dev gulp-babel`と実行します
- `yarn add --dev babel-preset-latest`と実行します
- `yarn add --dev del`と実行します (これは後述の通り、`clean`タスクのためです)
- `package.json`ファイルに、Babelの設定のための`babel`フィールドを追加します。最新のBabelプリセットを使うため、以下のようにします:

```json
"babel": {
  "presets": [
    "latest"
  ]
},
```

**注意**: `.babelrc`ファイルがプロジェクトのルートにあると、`package.json`の`babel`よりもそちらが優先されます。ルートフォルダはそのうちどんどん膨れ上がっていくため、`package.json`が巨大にならないうちは、Babelの設定は`package.json`内に記述するようにします。

- `index.js`ファイルを新たな`src`フォルダに移動します。これはES6のコードを記述するためのフォルダです。`lib`フォルダはコンパイル後のES5コードが置かれます。GulpとBabelはこのコード生成の面倒を見ます。`index.js`にある前章の`color`関連のコードを削除し、以下の簡単な内容に書き換えます:

```javascript
const str = 'ES6';
console.log(`Hello ${str}`);
```

ここでは*テンプレート文字列リテラル*を使っています。これはES6の新しい構文で、`${}`を使った文字列連結なしで変数を直接文字列内に埋め込むためのものです。テンプレート文字列リテラルは**バッククォート**を使っていることに注意してください。

- 以下の内容で`gulpfile.js`を作ります:

```javascript
const gulp = require('gulp');
const babel = require('gulp-babel');
const del = require('del');
const exec = require('child_process').exec;

const paths = {
  allSrcJs: 'src/**/*.js',
  libDir: 'lib',
};

gulp.task('clean', () => {
  return del(paths.libDir);
});

gulp.task('build', ['clean'], () => {
  return gulp.src(paths.allSrcJs)
    .pipe(babel())
    .pipe(gulp.dest(paths.libDir));
});

gulp.task('main', ['build'], (callback) => {
  exec(`node ${paths.libDir}`, (error, stdout) => {
    console.log(stdout);
    return callback(error);
  });
});

gulp.task('watch', () => {
  gulp.watch(paths.allSrcJs, ['main']);
});

gulp.task('default', ['watch', 'main']);

```

全体を理解するために少し時間をとりましょう。

GulpのAPI自体は非常に率直なものです。`gulp.task`を定義し、その中では`gulp.src`でファイルを参照し、そのファイルに対し一連の操作を`.pipe()`を使って適用し(この例の`babel()`のように)、新しいファイルを`gulp.dest`に出力します。Gulpのタスクは、`gulp.task`の第2引数として(`['build']`のような)配列の形で渡された必要なタスクを先に実行することができます。より完全な紹介は、[ドキュメント](https://github.com/gulpjs/gulp)を参照してください。

まず最初に`paths`オブジェクトを定義します。これは異なるファイルパスを格納し、DRYに保つためです。

続いて5つのタスク: `build`、`clean`、`main`、`watch`、そして`default`を定義します。

- `build`は`src`以下のソースファイルを変換し`lib`に保存するため、Babelが呼ばれるところです。
- `clean` は`build`を行う前に、`lib`フォルダに自動生成されるファイル全てを削除するタスクです。このタスクは、`src`内のファイルをリネームや削除したり、ビルドに失敗したことに気づかない場合でも`lib`フォルダを`src`フォルダに確実に同期させるため、生成されたファイルを削除するのに便利です。Gulpストリームと連携してファイルを削除するために、ここでは`del`パッケージを使っています(これはGulpでのファイルの削除方法として[推奨](https://github.com/gulpjs/gulp/blob/master/docs/recipes/delete-files-folder.md)されている方法です)。
- `main`は前章で`node .`と入力し実行させているのとほぼ同じもので、`lib/index.js`を実行することだけが異なっています。`index.js`はNodeが探しにいく標準のファイル名なので、単に`node lib`とだけ書いています(DRYを守るために`libDir`変数を使っています)。タスク内の`require('child_process').exec`と`exec`の部分はシェルコマンドを実行するNodeの標準関数です。`stdout`は`console.log()`に渡し、`gulp.task`のコールバック関数を使って潜在的なerrorを返します。この部分がはっきりと理解できなくても気にする必要はありません。このタスクは基本的に`node lib`を実行するためのものであるとだけ覚えてください。
- `watch` は特定のファイルに更新があった場合に`main`タスクを実行します。
- `default` は、コマンドラインで単に`gulp`と実行した場合に呼び出される特殊なタスクです。

**注意**: どうしてGulpファイル内でES6コードが使えるのか、気になっているかもしれません。なぜならこのコードはBabelでES5コードにトランスパイルされないからです。その理由は、ここではES6を標準でサポートしているバージョンのNodeを使っているためです(`node -v`を実行し、バージョン6.5.0以上のNodeを使っていることを確認してください)。

さあ、動かしてみましょう!

- `package.json`内の`start`スクリプトを`"start": "gulp"`に変更します。
- `yarn start`と実行します。"Hello ES6"と表示され、変更の監視が始まるはずです。 `src/index.js`に間違ったコードを書いてみて、saveしたときにGulpが自動的にエラーを表示するか試してみましょう。
- `/lib/`を`.gitignore`に追加します。

(原文: [3 - Setting up ES6 with Babel and Gulp](https://github.com/verekia/js-stack-from-scratch/tree/master/tutorial/3-es6-babel-gulp))

次章: [4 - ES6構文によるクラスの使い方](/tutorial/4-es6-syntax-class)

[前章](/tutorial/2-packages)または[目次](https://github.com/verekia/js-stack-from-scratch#table-of-contents)に戻る。
