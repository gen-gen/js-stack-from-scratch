# 11 - Testing with Mocha, Chai, and Sinon

## MochaとChai

- `src/test`フォルダを作ります。このフォルダはアプリケーションのフォルダ構造を反映していて、`src/test/client`フォルダも同様に作ります(`server`フォルダと`shared`フォルダも必要なら作っても構わないのですが、ここではテストを書きません)。

- `src/test/client`フォルダ内に`state-test.js`ファイルを作ります。これはReduxアプリケーションのライフサイクルをテストするのに使われます。

メインのテスティングフレームワークには、[Mocha](http://mochajs.org/)を使うことにします。Mochaは使いやすく、機能が豊富で、今のところ[最もよく使われるJavaScriptのテスティングフレームワーク](http://stateofjs.com/2016/testing/)です。また、フレキシブルでモジュラー化されています。特に、好きなアサーションライブラリを使うことができます。[Chai](http://chaijs.com/)は、たくさんの[plugin](http://chaijs.com/plugins/)を持つ素晴らしいアサーションライブラリで、異なるアサーションスタイルを選ぶことができます。

- `yarn add --dev mocha chai`を実行して、MochaとChaiをインストールします。

`state-test.js`を以下のように書きます:

```javascript
/* eslint-disable import/no-extraneous-dependencies, no-unused-expressions */

import { createStore } from 'redux';
import { combineReducers } from 'redux-immutable';
import { should } from 'chai';
import { describe, it, beforeEach } from 'mocha';
import dogReducer from '../../client/reducers/dog-reducer';
import { makeBark } from '../../client/actions/dog-actions';

should();
let store;

describe('App State', () => {
  describe('Dog', () => {
    beforeEach(() => {
      store = createStore(combineReducers({
        dog: dogReducer,
      }));
    });
    describe('makeBark', () => {
      it('should make hasBarked go from false to true', () => {
        store.getState().getIn(['dog', 'hasBarked']).should.be.false;
        store.dispatch(makeBark());
        store.getState().getIn(['dog', 'hasBarked']).should.be.true;
      });
    });
  });
});
```

さて、この全体を解析してみましょう。

まず、`chai`の`should`アサーションスタイルをどのようにimportしているのかを注意してみてください。これを使うと`mynumber.should.equal(3)`といった、ちょっと巧妙な構文を使ってアサートできるようになります。どんなオブジェクトに対しても`should`を呼び出せるように、何よりも前に`should()`を実行しなければなりません。これらのアサーションには、 `mybook.should.be.true`のように*式(expressions)*であるものがあり、ESLintはこのような書き方に警告を出します。そのため、ファイルの先頭に`no-unused-expressions`ルールを無効にするESLintのコメントを追加しています。

Mochaのテストは木構造のような階層構造で動作します。ここでは、アプリケーションの状態の`dog`属性に影響する`makeBark`関数をテストしたいので、テストの階層は`describe()`で表現した通り、`App State > Dog > makeBark`という形になります。`it()`が実際のテスト関数で、`beforeEach()`は`it()`の各テストの前に実行される関数になります。
この例では、各テストが走る前に新しい状態が必要です。
そこで、ファイルの先頭で変数`store`を宣言し、このファイル内での全てのテストで使えるようにしています。

Mocha tests work like a tree. In our case, we want to test the `makeBark` function which should affect the `dog` attribute of the application state, so it makes sense to use the following hierarchy of tests: `App State > Dog > makeBark`, that we declare using `describe()`. `it()` is the actual test function and `beforeEach()` is a function that is executed before each `it()` test. In our case, we want a fresh new store before running each test. We declare a `store` variable at the top of the file because it should be useful in every test of this file.

`makeBark`テストは明示的に書かれていますが、`it()`内に文字列で与えられている説明によってさらにわかりやすくなっています: ここでは`makeBark`の呼び出しによって`hasBarked`が`false`から`true`に変わるのをテストしています。

さあ、テストを実行してみましょう！

- `gulp-mocha`プラグインを使って、以下の`test`タスクを作ります:

```javascript
import mocha from 'gulp-mocha';

const paths = {
  // [...]
  allLibTests: 'lib/test/**/*.js',
};

// [...]

gulp.task('test', ['build'], () =>
  gulp.src(paths.allLibTests)
    .pipe(mocha())
);
```

- もちろん、`yarn add --dev gulp-mocha`で実行します。

見ての通り、テストは`lib`にトランスパイルされたコードを実行します。これが`test`が`build`の前提条件になっている理由です。
`build`も`lint`という前提条件を持っており、そして最後に、`test`が`main`の前提条件になります。これにより、`default`は次のようなタスクの連鎖ができます: `lint` > `build` > `test` > `main`。

- `main`の前提条件を`test`に変えます:

```javascript
gulp.task('main', ['test'], () => /* ... */ );
```

- `package.json`の`"test"`スクリプトを`"test": "gulp test"`に変更します。こうすると`yarn test`でテストを実行できるようになります。
`test`は例えばCIサービスのようなツールで自動的に呼ばれる標準のスクリプトでもあるため、必ずテストタスクはここに書くべきです。
`yarn start`はWebpackのクライアントバンドルを作る前にテストを実行するため、全てのテストにパスすればビルドするだけになります。
This way you can use `yarn test` to just run your tests. `test` is also the standard script that will be automatically called by tools like continuous integration services for instance, so you should always bind your test task to it. `yarn start` will run the tests before building the Webpack client bundle as well, so it will only build it if all tests pass.

- `yarn test`または`yarn start`を実行すると、テスト結果が出力されます。おそらくグリーンになっているはずです。

## Sinon

ユニットテストで*fake*を使いたくなることがあります。たとえば、`deleteDatabases()`という関数の呼び出しを含む`deleteEverything`という関数があったとします。`deleteDatabases()`の実行には様々な副作用があるため、テストスイートを走らせる際には実行したくありません。

[Sinon](http://sinonjs.org/) is a testing library that offers **Stubs** (and a lot of other things), which allow us to neutralize `deleteDatabases` and simply monitor it without actually calling it. This way we can test if it got called, or which parameters it got called with for instance. This is typically very useful to fake or avoid AJAX calls - which can cause side-effects on the back-end.

In the context of our app, we are going to add a `barkInConsole` method to our `Dog` class in `src/shared/dog.js`:

```javascript
class Dog {
  constructor(name) {
    this.name = name;
  }

  bark() {
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

If we run `barkInConsole` in a unit test, `console.log()` will print things in the terminal. We are going to consider this to be an undesired side-effect in the context of our unit tests. We are interested in knowing if `console.log()` *would have normally been called* though, and we want to test what parameters it *would have been called with*.

- Create a new `src/test/shared/dog-test.js` file, and add write the following:

```javascript
/* eslint-disable import/no-extraneous-dependencies, no-console */

import chai from 'chai';
import { stub } from 'sinon';
import sinonChai from 'sinon-chai';
import { describe, it } from 'mocha';
import Dog from '../../shared/dog';

chai.should();
chai.use(sinonChai);

describe('Shared', () => {
  describe('Dog', () => {
    describe('barkInConsole', () => {
      it('should print a bark string with its name', () => {
        stub(console, 'log');
        new Dog('Test Toby').barkInConsole();
        console.log.should.have.been.calledWith('Wah wah, I am Test Toby');
        console.log.restore();
      });
    });
  });
});
```

Here, we are using *stubs* from Sinon, and a Chai plugin to be able to use Chai assertions on Sinon stubs and such.

- Run `yarn add --dev sinon sinon-chai` to install these libraries.

So what is new here? Well first of all, we call `chai.use(sinonChai)` to activate the Chai plugin. Then, all the magic happens in the `it()` statement: `stub(console, 'log')` is going to neutralize `console.log` and monitor it. When `new Dog('Test Toby').barkInConsole()` is executed, a `console.log` is normally supposed to happen. We test this call to `console.log` with `console.log.should.have.been.calledWith()`, and finally, we `restore` the neutralized `console.log` to make it work normally again.

**Important note**: Stubbing `console.log` is not recommended, because if the test fails, `console.log.restore()` is never called, and therefore `console.log` will remain broken for the rest of the command you executed in your terminal! It won't even print the error message that caused the test to fail, so it leaves you with very little information about what happened. That can be quite confusing. It is a good example to illustrate stubs in this simple app though.

If everything went well in this chapter, you should have 2 passing tests.

Next section: [12 - Type Checking with Flow](/tutorial/12-flow)

Back to the [previous section](/tutorial/10-immutable-redux-improvements) or the [table of contents](https://github.com/verekia/js-stack-from-scratch).
