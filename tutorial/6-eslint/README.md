# 6 - ESLint

潜在的な問題を見つけるため、コードをlintしましょう。ESLintはES6コード用のlinterです。ルールを自身で細かく設定する代わりに、Airbnbが作った設定を利用します。この設定はプラグインをいくつか使うため、プラグインをインストールする必要があります。

- `yarn add --dev eslint eslint-config-airbnb eslint-plugin-import eslint-plugin-jsx-a11y eslint-plugin-react`を実行します

このように、複数のパッケージを一つのコマンドでインストールできます。いつものように、これらは全て`package.jsonに追加されます。

`package.json`に以下のような`eslintConfig`フィールドを追加します:

```json
"eslintConfig": {
  "extends": "airbnb",
  "plugins": [
    "import"
  ]
},
```

`plugins`の部分はES6のimport構文を使うことをESLintに教えています。

**注意**: プロジェクトのルートにある`.eslintrc.js`ファイルの代わりに、`package.json`ファイルの`eslintConfig`フィールドを使うこともできます。Babelの設定と同様、ルートフォルダが多くのファイルで溢れるのを避けるためこのようにしていますが、ESLintの設定が複雑化してきたら、この代替策を検討してください。

ESLintを実行するためのGulpタスクが必要です。そのため、ESLint Gulpプラグインをインストールします。

- `yarn add --dev gulp-eslint`を実行します

次のようなタスクを`gulpfile.babel.js`に追加します:

```javascript
import eslint from 'gulp-eslint';

const paths = {
  allSrcJs: 'src/**/*.js',
  gulpFile: 'gulpfile.babel.js',
  libDir: 'lib',
};

// [...]

gulp.task('lint', () => {
  return gulp.src([
    paths.allSrcJs,
    paths.gulpFile,
  ])
    .pipe(eslint())
    .pipe(eslint.format())
    .pipe(eslint.failAfterError());
});
```

Gulpにこのタスクを伝えるため、`gulpfile.babel.js`と、`src`以下にあるJSファイルをインクルードする必要があります。

以下のように、Gulpタスク`build`の前提条件に`lint`タスクを追加します:

```javascript
gulp.task('build', ['lint', 'clean'], () => {
  // ...
});
```

- `yarn start`を実行すると、Gulpfile内のたくさんのlintのエラーと、`index.js`内で`console.log()`を使っている警告が表示されるはずです。

警告の一種として、`'gulp' should be listed in the project's dependencies, not devDependencies (import/no-extraneous-dependencies)`といった出力があるとか思います。これは実際には誤検知に当たります。ESLintはビルド時のみに使われるJSファイルがどれで、そうではないJSファイルがどれかを知ることができません。そのため、コード内のコメントを使ってESLintに教えてあげる必要があります。`gulpfile.babel.js`ファイルの先頭に、以下を追加します:

```javascript
/* eslint-disable import/no-extraneous-dependencies */
```

こうすることで、ESLintはこのファイルに`import/no-extraneous-dependencies`ルールを適用しなくなります。

残る問題は`Unexpected block statement surrounding arrow body (arrow-body-style)`です. これは貴重な指摘です。ESLintは以下のようなコードのもっと良い書き方を教えてくれているのです。

```javascript
() => {
  return 1;
}
```

これは次のように修正するべきです:

```javascript
() => 1
```

関数がreturn文しか含まない場合、ブレースとreturn文とセミコロンを省略できるためです。

それではGulpファイルを正しく修正してみましょう:

```javascript
gulp.task('lint', () =>
  gulp.src([
    paths.allSrcJs,
    paths.gulpFile,
  ])
    .pipe(eslint())
    .pipe(eslint.format())
    .pipe(eslint.failAfterError())
);

gulp.task('clean', () => del(paths.libDir));

gulp.task('build', ['lint', 'clean'], () =>
  gulp.src(paths.allSrcJs)
    .pipe(babel())
    .pipe(gulp.dest(paths.libDir))
);
```

最後の問題は`console.log()`についてのものです。この例で問題の元となっている`index.js`内での`console.log()`は正しいものであることを伝えましょう。ご想像の通り、`/* eslint-disable no-console */`を`index.js`の先頭に置きます。

- `yarn start`を実行すると、すべて解決されている状態に戻ります。

**注意**: この章ではコンソール用にESLintを設定しています。ビルド時/pushする前にエラーを見つけられるのは素晴らしいことですが、もしかするとIDEにも統合したくなるかもしれません。しかし、IDEに付属しているES6用のlintツールを使わないでください。そのツールがlinting用に使っているバイナリは`node_modules`フォルダにあるように設定してください。そううするとそのツールはプロジェクトの全ての設定、Airbnbのプリセットなど全てが利用できるようになります。そうでなければ、一般的なES6のlintingしか使えるようになりません。

(原文: [6 - ESLint](https://github.com/verekia/js-stack-from-scratch/tree/master/tutorial/6-eslint))

次章: [7 - Webpackによるクライアントアプリ](/tutorial/7-client-webpack)

[前章](/tutorial/5-es6-modules-syntax)または[目次](https://github.com/verekia/js-stack-from-scratch)に戻る。
