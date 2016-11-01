# 11 - Mocha、Chai、Sinonによるテスティング

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

Mochaのテストは木構造のような階層構造で動作します。ここでは、アプリケーションの状態の`dog`属性に影響する`makeBark`関数をテストしたいので、テストの階層は`describe()`で表現した通り、`App State > Dog > makeBark`という形になります。`it()`が実際のテスト関数で、`beforeEach()`は`it()`の各テストの前に実行される関数になります。この例では、各テストが走る前に新しい状態が必要です。そこで、ファイルの先頭で変数`store`を宣言し、このファイル内での全てのテストで使えるようにしています。

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

このように、テストは`lib`にトランスパイルされたコードを実行します。これが`test`が`build`の前提条件になっている理由です。`build`も`lint`という前提条件を持っており、そして最後に、`test`が`main`の前提条件になります。これにより、`default`は次のようなタスクの連鎖ができます: `lint` > `build` > `test` > `main`。

- `main`の前提条件を`test`に変えます:

```javascript
gulp.task('main', ['test'], () => /* ... */ );
```

- `package.json`の`"test"`スクリプトを`"test": "gulp test"`に変更します。こうすると`yarn test`でテストを実行できるようになります。`test`は例えばCIサービスのようなツールで自動的に呼ばれる標準のスクリプトでもあるため、必ずテストタスクはここに書くべきです。`yarn start`はWebpackのクライアントバンドルを作る前にテストを実行するため、全てのテストにパスすればビルドするだけになります。

- `yarn test`または`yarn start`を実行すると、テスト結果が出力されます。おそらくグリーンになっているはずです。

## Sinon

ユニットテストで*fake*を使いたくなることがあります。たとえば、`deleteDatabases()`という関数の呼び出しを含む`deleteEverything`という関数があったとします。`deleteDatabases()`の実行には様々な副作用があるため、テストスイートを走らせる際には実行したくありません。

[Sinon](http://sinonjs.org/)は **スタブ** (とその他もろもろ)を提供するテスティングライブラリで、`deleteDatabases`を無効化し実際に呼び出すことなく監視のみ行うようになります。これにより、呼ばれたかどうか、どのような引数で呼ばれたかどうかといったことをテストできるようになります。これはAJAXの呼び出しをフェイクしたり避けたりする場合によく使われます - バックエンド上の副作用を起こしうるような場合です。

ここでは、アプリに対して、`src/shared/dog.js`にある`Dog`クラスのメソッド`barkInConsole`を追加してみます。

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

ユニットテストで`barkInConsole`を実行すると、 `console.log()` はターミナルへの出力を行います。このようなことは、ユニットテストの文脈における望ましくない副作用であると考えられます。興味があるのは`console.log()`が *正常に呼び出されるかどうか* であり、テストしたいのはどのようなパラメーターが*呼び出されるのか*といったことです。

- 新しいファイル`src/test/shared/dog-test.js`を作り、次のように書き加えます:

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

ここでは、Sinonの*stubs*と、その上でChaiのアサーションを使うためのChaiプラグインをimportしています。

- `yarn add --dev sinon sinon-chai`を実行してライブラリをインストールします。

ここでの新しいことは何でしょうか？ 何よりもまず、Chaiプラグインを使うために`chai.use(sinonChai)`を呼び出しています。そして、`it()`文で全ての魔法が発動しています: `stub(console, 'log')`が`console.log`を無効化し監視します。`new Dog('Test Toby').barkInConsole()`が実行されると、新たな`console.log`が用意されます。そして`console.log`を`console.log.should.have.been.calledWith()`で呼び出してテストし、最後に`restore`で`console.log`を元に戻しています。

**重要な注意**: `console.log`をスタブするのはおすすめできません。もしテストが失敗すると、`console.log.restore()`が呼び出されず、そのため`console.log`はターミナルで残りのコマンドをテストしている間中ずっと壊れてしまったままだからです! テストが失敗した時のエラーメッセージも出力できないため、何が起こっているかの情報をほとんど残すことができなくなります。これは大変厄介です。この例は単純なアプリでスタブを説明するためだけのものです。

もしこの章の内容がうまくいっていれば、パスするテストが2つ得らるはずです。

次章: [12 - Flowによる型検査](/tutorial/12-flow)

[前章](/tutorial/10-immutable-redux-improvements)または[目次](https://github.com/verekia/js-stack-from-scratch)に戻る。
