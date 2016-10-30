# 4 - Using the ES6 syntax with a class

- 新しいファイル`src/dog.js`を作り、以下のES6クラスを書きます:

```javascript
class Dog {
  constructor(name) {
    this.name = name;
  }

  bark() {
    return `Wah wah, I am ${this.name}`;
  }
}

module.exports = Dog;
```

以前から他の言語でOOPをしたことがあるなら、特に意外な点はないでしょう。しかし、JavaScriptに取り入れられたのはつい最近なのです。このクラスは`module.exports`へ代入することにより、外の世界にさらされます。

典型的なES6コードはクラスや`const`、`let`、 `bark()`で使われているような(backtickによる)"テンプレート文字列"、アロー関数(`(param) => { console.log('Hi'); }`)などが使われます。この例では使われていないものもありますが。

`src/index.js`ファイルに、以下のように書きます:

```javascript
const Dog = require('./dog');

const toby = new Dog('Toby');

console.log(toby.bark());
```

見ての通り、先ほど使ったコミュニティで作られた`color`packageとは異なり、自分で作ったファイルをrequireするときには、`require()`内で`./`を使います。

- `yarn start`を実行します。'Wah wah, I am Toby'と表示されるはずです。

- `lib`に生成されたコードを見て、コンパイルされたコードがどのようになっているかを確認します(例えば`const`の代わりに`var`があるなど).

原文: [4 - Using the ES6 syntax with a class](https://github.com/verekia/js-stack-from-scratch/tree/master/tutorial/4-es6-syntax-class)

次章: [5 - ES6のモジュールの構文](/tutorial/5-es6-modules-syntax)

[前章](/tutorial/3-es6-babel-gulp) または [目次](https://github.com/verekia/js-stack-from-scratch)に戻る。
