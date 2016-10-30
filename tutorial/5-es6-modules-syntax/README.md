# 5 - ES6モジュールの構文

ここでは、`const Dog = require('./dog')`を`import Dog from './dog'`に置き換えます。これは("CommonJS"のモジュール構文と比べると)より新しいES6モジュールの構文です。

`dog.js`内の`module.exports = Dog`を`export default Dog`に置き換えます。

`dog.js`では、`Dog`の名前は`export`の中でしか使われていないことに注意してください。これにより、次の例のように、匿名のクラスを直接エクスポートすることも可能です。

```javascript
export default class {
  constructor(name) {
    this.name = name;
  }

  bark() {
    return `Wah wah, I am ${this.name}`;
  }
}
```

`index.js`ファイルの`import`での'Dog'という名前は実際には使う人次第だということにもう気づいたかもしれません。このように書いても問題なく動きます。

```javascript
import Cat from './dog';

const toby = new Cat('Toby');
```

言うまでもなく、インポートしたclass / moduleの名前をそのまま使うことがほとんどです。

そうしない例としては、Gulpファイル内で`const babel = require('gulp-babel')`とする場合が挙げられます。

それでは、`gulpfile.js`ファイル内の`require()`はどうでしょうか？
代わりに`import`が使えるでしょうか？ 最新版のNodeはES6のほとんどの機能に対応していますが、ES6モジュールはまだ対応していません。ありあたいことに、GulpはBabelに助けを求めることができます。`gulpfile.js`を`gulpfile.babel.js`にリネームすれば、Babelは`import`されたモジュールをGulpに渡すことができます。

- `gulpfile.js`を`gulpfile.babel.js`にリネームします。

- `require()`を以下のように書き換えます:

```javascript
import gulp from 'gulp';
import babel from 'gulp-babel';
import del from 'del';
import { exec } from 'child_process';
```

`child_process`から`exec`が直接展開されるシンタックスシュガーに注意してください。実に見事です!

- `yarn start` を実行すると "Wah wah, I am Toby" と表示されるはずです。

原文: [5 - The ES6 modules syntax](https://github.com/verekia/js-stack-from-scratch/tree/master/tutorial/5-es6-modules-syntax)

次章: [6 - ESLint](/tutorial/6-eslint)

[前章](/tutorial/4-es6-syntax-class) または [目次](https://github.com/verekia/js-stack-from-scratch)に戻る。
