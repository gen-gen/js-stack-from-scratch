# 12 - Flow

[Flow](https://flowtype.org/)は静的型チェッカーです。コード内の一貫性のない型を検出し、アノテーションを使って明示的な型宣言を追加することができます。

- トランスパイルの過程でBabelにFlowアノテーションを理解し削除させるため、`yarn add --dev babel-preset-flow`を実行してBabelのFlowプリセットをインストールします。そして、`package.json`の`babel.presets`に`"flow"`を追加します。

- プロジェクトのルートに空の`.flowconfig`ファイルを作ります。

- `yarn add --dev gulp-flowtype`を実行してFlowのGulpプラグインをインストールし、`lint`タスクに`flow()`を追加します:

```javascript
import flow from 'gulp-flowtype';

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
    .pipe(flow({ abort: true })) // Add Flow here
);
```

`abort`オプションはFlowが問題を検出した場合にGulpタスクを中断させるものです。

これでFlowが実行できるようになりました。

- `src/shared/dog.js`にFlowアノテーションを以下のように追加します:

```javascript
// @flow

class Dog {
  name: string;

  constructor(name: string) {
    this.name = name;
  }

  bark(): string {
    return `Wah wah, I am ${this.name}`;
  }

  barkInConsole() {
    /* eslint-disable no-console */
    console.log(this.bark());
    /* eslint-enable no-console */
  }

}

export default Dog;
```

`// @flow`コメントはFlowにこのファイルの型検査をしてほしいことを伝えるものです。それ以外の部分について、Flowアノテーションは、たいてい関数の引数か関数名のあとにコロンがついています。詳細はドキュメントを見てください。

この状態で`yarn start`を実行すると、Flowは問題なく動きますが、ESLintはここで使われている標準的ではない文法について注意してくれることでしょう。Babelのパーサは先ほどインストールした`babel-preset-flow`プラグインのおかげでFlowコンテンツをパースできるようになるため、ESLintがFlowアノテーションを独自に解釈しようとするのではなく、Babelのパーサを使うようになってくれると理想的です。`babel-eslint`パッケージを使えばこれが実現できます。やってみましょう。

- `yarn add --dev babel-eslint`を実行します。

- `package.json`の`eslintConfig`に次のプロパティを追加します: `"parser": "babel-eslint"`

`yarn start`を実行するとlintと型検査が正しく行われるようになったはずです。

これでESLintとBabelで共通のパーサを実行できるようになったので、`eslint-plugin-flowtype`を使ってESLintにFlowのアノテーションをlintさせられるようになりました。

- `yarn add --dev eslint-plugin-flowtype`を実行し、`package.json`の`eslintConfig.plugins`に`"flowtype"`を追加します。また、`eslintConfig.extends`の配列の`"airbnb"`の次に`"plugin:flowtype/recommended"`を追加します。

これで例えば`name:string`というアノテーションを書くと、ESLintはコロンの後ろにスペースを入れるのを忘れていると注意してくれるようになります。

**注意**: `package.json`に書いた`"parser": "babel-eslint"`プロパティは実際には`"plugin:flowtype/recommended"`に含まれるものなので`package.json`から削除することができますが、好みで明示的に残しておいても構いません。このチュートリアルは最小限のセットアップを行うものなので、削除しました(訳注:削除してない??)。

- `src`以下のすべての`.js`と`.jsx`ファイルに対して、`// @flow`を追加し、`yarn test`または`yarn start`を実行します。そしてFlowが提案した部分全部に対して、型アノテーションを追加します。

直感的ではない例としては、`src/client/component/message.jsx`にある以下のものがあります:

```javascript
const Message = ({ message }: { message: string }) => <div>{message}</div>;
```

このように、分割代入している場合、オブジェクトリテラル記法のようなものを使って展開されたプロパティにアノテーションをつけます。

他に見かける例としては、`src/client/reducers/dog-reducer.js`にあるように、FlowはImmutableがデフォルトのexportをつけないと注意してくるというものがあります。この問題は[Immutableの#863](https://github.com/facebook/immutable-js/issues/863)で議論されていますが、2つの回避策があります:

```javascript
import { Map as ImmutableMap } from 'immutable';
// or
import * as Immutable from 'immutable';
```

Immutableがこの件を公式に解決するまで、Immutableコンポーネントをimportする時はどちらか好きな方を使いましょう。個人的には`import * as Immutable from 'immutable'`の方が、短い上に修正された際のリファクタリングも不要なので気に入っています。

**注意**: Flowが型エラーを`node_modules`フォルダで見つけた場合、問題のあるパッケージを無視するため`[ignore]`セクションを`.flowconfig`に追加します(`node_modules`ディレクトリ全体を無視しないようにします)。以下のようになります:

```flowconfig
[ignore]

.*/node_modules/gulp-flowtype/.*
```

この場合、`node_modules/gulp-flowtype`ディレクトリにあるAtomの`linter-flow`プラグインに型エラーが検出されました。これは`// @flow`がついたファイルを同梱していました。

これでlintされて、型検査もテストも通った万全のコードができました!

原文: [12 - Flow](https://github.com/verekia/js-stack-from-scratch/tree/master/tutorial/12-flow)

[前章](/tutorial/11-testing-mocha-chai-sinon)または[目次](https://github.com/verekia/js-stack-from-scratch)に戻る。

