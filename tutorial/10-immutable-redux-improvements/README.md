# 10 - Immutable JS と Redux の改良

## Immutable JS

前章とは違い、本章はとても簡単で、ちょっとした変更を行うだけです。

まず、コードベースに**Immutable JS**を追加します。Immutableはオブジェクトを変更(mutating)することなしに扱うためのライブラリです。例えば以下のようにする代わりに:

```javascript
const obj = { a: 1 };
obj.a = 2; // Mutates `obj`
```

こうすることができます:

```javascript
const obj = Immutable.Map({ a: 1 });
obj.set('a', 2); // Returns a new object without mutating `obj`
```

このアプローチは**関数型プログラミング**のパラダイムに則ったもので、Reduxとの相性がとても良くなっています。実際、reducer関数は引数として渡される状態を変更しない、純粋な関数でなければならず、状態オブジェクトを新しく作って返すようになっています。それではImmutableを使い、このやり方を強制してみましょう。

- `yarn add immutable`を実行します。

このコードベースでは`Map`を使うのですが、ESLintとAirbnbの設定はクラス以外に大文字の名前を使うと警告を出します。`package.json`の`eslintConfig`に以下を追加します:

```json
"rules": {
  "new-cap": [
    2,
    {
      "capIsNewExceptions": [
        "Map",
        "List"
      ]
    }
  ]
}
```
これは`Map`と`List`(この2つのImmutableなオブジェクトはずっと使うことになります)を例外扱いするようESLintルールを変更するものです。この冗長なJSONフォーマットはYarn/NPMによって自動的に行われるもので、残念ながら簡潔にはできません。

それはさておき、Immutableに戻りましょう:

`dog-reducer.js`を以下のように修正します:

```javascript
import Immutable from 'immutable';
import { MAKE_BARK } from '../actions/dog-actions';

const initialState = Immutable.Map({
  hasBarked: false,
});

const dogReducer = (state = initialState, action) => {
  switch (action.type) {
    case MAKE_BARK:
      return state.set('hasBarked', action.payload);
    default:
      return state;
  }
};

export default dogReducer;
```

初期状態はImmutableのMapを使って作られます。そして新しい状態は、それ以前の状態を変更することのない`set()`を使って作られるようになります。

`containers/bark-message.js`の`mapStateToProps`関数を、`.hasBarked`の代わりに`.get('hasBarked')`を使うよう修正します:

```javascript
const mapStateToProps = state => ({
  message: state.dog.get('hasBarked') ? 'The dog barked' : 'The dog did not bark',
});
```

アプリケーションは以前と同様の振る舞いをするはずです。

**注意**: BabelがImmutableについて100KB制限を超えていると警告する場合, `package.json`の`babel`のところに`"compact": false`を追加します。

上記のコード片を見ると分かる通り、状態オブジェクトはイミュータブルではない、素のJavaScriptオブジェクトである`dog`オブジェクトを属性として持っています。
これはこれで構わないのですが、イミュータブルオブジェクトしか扱いたくない場合、Reduxの`combineReducers`関数を置き換えるため、`redux-immutable`パッケージをインストールできます。

**オプション**:

- `yarn add redux-immutable`を実行します。
- `app.jsx`にある`combineReducers`関数を`redux-immutable`からimportしたものに置き換えます。
- `bark-message.js`の`state.getIn(['dog', 'hasBarked'])`を`state.dog.get('hasBarked')`に置き換えます。

## Reduxアクション

アプリにアクションを加えていくにつれて、同じボイラープレートを何度も書き加えていくことになります。`redux-actions`パッケージを使えば、ボイラープレートのコードを減らしてくれます。`dog-actions.js`ファイルを`redux-actions`で簡潔に書き換えることができます。

```javascript
import { createAction } from 'redux-actions';

export const MAKE_BARK = 'MAKE_BARK';
export const makeBark = createAction(MAKE_BARK, () => true);
```

`redux-actions`は先ほど実装したようなアクションである[Flux Standard Action](https://github.com/acdlite/flux-standard-action)モデルを実装したもので、このモデルに従っていれば`redux-actions`はシームレスに導入できます。

- `yarn add redux-actions`を忘れずに実行します。

次章: [11 - MochaとChai, Sinonによるテスティング](/tutorial/11-testing-mocha-chai-sinon)

[前章](/tutorial/9-redux) または [目次](https://github.com/verekia/js-stack-from-scratch)に戻る。
