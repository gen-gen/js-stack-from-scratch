# 8 - React

アプリの表示にReactを使ってみましょう。

まず、 ReactとReactDOMをインストールします:

- `yarn add react react-dom`を実行します

この2つのパッケージは`"dependencies"`と`"devDependencies"`とに追加されます。ビルドツールと異なり、クライアントバンドルは本番環境用でも使われるためです。

`src/client/app.js`を`src/client/app.jsx`にリネームし、ReactとJSXのコードを書いてみましょう:

```javascript
import 'babel-polyfill';

import React, { PropTypes } from 'react';
import ReactDOM from 'react-dom';
import Dog from '../shared/dog';

const dogBark = new Dog('Browser Toby').bark();

const App = props => (
  <div>
    The dog says: {props.message}
  </div>
);

App.propTypes = {
  message: PropTypes.string.isRequired,
};

ReactDOM.render(<App message={dogBark} />, document.querySelector('.app'));
```

**注意**: もしReactとそのPropTypesをよく知らないのであれば、まずReactについて学習してからこのチュートリアルに戻ってきてください。後の章ではReactが多用されるため、よく理解しておく必要があります。

Gulpfile内の`clientEntryPoint`の値に`.jsx`拡張子を追加します:

```javascript
clientEntryPoint: 'src/client/app.jsx',
```

ここではJSXの構文を使うため、BabelにJSXを変換するよう伝える必要があります。BabelにJSX構文の処理方法を教えるReact用のBabelプリセットをインストールします: `yarn add --dev babel-preset-react` そして`package.json`ファイルの`babel`エントリを次のように変更します:

```json
"babel": {
  "presets": [
    "latest",
    "react"
  ]
},
```

これで`yarn start`実行後、`index.html`を開くとReactが"The dog says: Wah wah, I am Browser Toby"と出力するのを確認できるはずです。


次章: [9 - Redux](/tutorial/9-redux)

[前章](/tutorial/7-client-webpack)または[目次](https://github.com/verekia/js-stack-from-scratch)に戻る。
