# 9 - Redux

本章では(おそらくここまでで最も難しい章なのですが)、アプリケーションに[Redux](http://redux.js.org/)を追加し、Reactと連携させます。Reduxはアプリケーションの状態を管理するもので、アプリの状態を表現する素のJavaScriptオブジェクトである**store**、一般的にユーザが起点となる**action**、アクションハンドラにも見える**reducer**が合わさったものです。reducerはアプリケーションの状態(*store*)に影響を及ぼし、アプリケーションの状態が変更がされると、アプリケーションに何かが起こります。Reduxの分かりやすい動くデモが[ここ](http://slides.com/jenyaterpil/redux-from-twitter-hype-to-production#/9)にあります。

Reduxを可能な限りシンプルに扱うにはどうすればよいか説明するため、アプリはメッセージとボタンで構成されるようにします。
メッセージは犬が鳴いているかどうか（最初は鳴いていません）を伝え、ボタンを押すと犬が鳴き、メッセージが更新されるようにします。

ここでは、`redux`と`react-redux`の2つのパッケージが必要になります。

- `yarn add redux react-redux`を実行します。

2つのフォルダ、`src/client/actions`と`src/client/reducers`を作るところからはじめます。

- `actions`内に、以下の`dog-actions.js`ファイルを作ります:

```javascript
export const MAKE_BARK = 'MAKE_BARK';

export const makeBark = () => ({
  type: MAKE_BARK,
  payload: true,
});
```

ここではアクションタイプ`MAKE_BARK`を定義しています。また、`MAKE_BARK`アクションのトリガーとなる関数(これは*action creator*とも言います)`makeBark`も定義します。
どちらも他のファイルから利用するため、exportされています。このアクションは[Flux Standard Action](https://github.com/acdlite/flux-standard-action)モデルで実装されています。これは`type`属性と`payload`属性を持っているのはそのせいです。

- `reducers`フォルダに以下のような`dog-reducer.js`ファイルを作ります:

```javascript
import { MAKE_BARK } from '../actions/dog-actions';

const initialState = {
  hasBarked: false,
};

const dogReducer = (state = initialState, action) => {
  switch (action.type) {
    case MAKE_BARK:
      return { hasBarked: action.payload };
    default:
      return state;
  }
};

export default dogReducer;
```


`dogReducer`プロパティが

ここではアプリの初期状態として、`hasBarked`プロパティが`false`になっている状態を定義しています。また、どのアクションが実行されたかによって状態を作る関数`dogReducer`も定義しています。この関数は状態を変更することがありません。代わりに新しい状態オブジェクトを返します。

- *store*を作るため`app.jsx`を修正します。ファイルの中身全体を以下の内容に置き換えます:

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import { createStore, combineReducers } from 'redux';
import { Provider } from 'react-redux';
import dogReducer from './reducers/dog-reducer';
import BarkMessage from './containers/bark-message';
import BarkButton from './containers/bark-button';

const store = createStore(combineReducers({
  dog: dogReducer,
}));

ReactDOM.render(
  <Provider store={store}>
    <div>
      <BarkMessage />
      <BarkButton />
    </div>
  </Provider>
  , document.querySelector('.app')
);
```

このように、Reduxの関数`createStore`がstoreを作ります。
storeオブジェクトはReduxの`combineReducers`関数を使って、reducerを全て(この例では1つだけですが)集めたものです。
各reducerはここで名前がつけられます。ここでは`dog`という名前をつけています。
ここは純粋なReduxの部分です。

それでは`react-redux`を使ってReduxとReactを結びつけましょう。`react-redux`がReactアプリにstoreを渡せるようにするため、アプリ全体を`<Provider>`コンポーネントで包みます。このコンポーネントは1つだけしか子要素を持てません。そのためここでは`<div>`要素を作り、この`<div>`要素にアプリの2つの重要な要素である`BarkMessage`と`BarkButton`を持たせるようにします。

`import`セクションにあった通り、`BarkMessage`と`BarkButton`は`containers`フォルダからimportしたものです。**Components** と **Containers**という２つのコンセプトを紹介するよい機会です。
*Component* は *賢くない(dumb)* Reactコンポーネントです。これはReduxの状態について何も知らないという意味です。*Containers* は *賢い(smart)*なコンポーネントです。これは状態を知っており、他の賢くないコンポーネントに*接続(connect)*できるためです。

- `src/client/components`と`src/client/containers`の2つのフォルダを作ります。

- `components`の中に、以下のファイルを作ります:

**button.jsx**

```javascript
import React, { PropTypes } from 'react';

const Button = ({ action, actionLabel }) => <button onClick={action}>{actionLabel}</button>;

Button.propTypes = {
  action: PropTypes.func.isRequired,
  actionLabel: PropTypes.string.isRequired,
};

export default Button;
```

そして、**message.jsx**:

```javascript
import React, { PropTypes } from 'react';

const Message = ({ message }) => <div>{message}</div>;

Message.propTypes = {
  message: PropTypes.string.isRequired,
};

export default Message;

```

これらは*賢くない*コンポーネントの例です。ロジックがなく、Reactの**props**経由で問い合わせが来たものをそのまま返すだけのものです。`button.jsx` と `message.jsx` の主な違いは、`Button`はそのプロパティとして**アクション(action)** を持っていることです。アクションは`onClick`イベントに結び付けられています。アプリのコンテキストで、`Button`ラベルは変化することがありませんが、`Message`コンポーネントはアプリの状態を反映し、状態によって変化します。

繰り返しますが、 *コンポーネント* はReduxの **アクション** やアプリの**状態** について何も知りません。そのため、適切な*actions* と *data*を2つの賢くないコンポーネントに伝えるための賢い**コンテナ**を作ることになるのです。

- `containers`内に、以下のファイルを作ります:

**bark-button.js**

```javascript
import { connect } from 'react-redux';
import Button from '../components/button';
import { makeBark } from '../actions/dog-actions';

const mapDispatchToProps = dispatch => ({
  action: () => { dispatch(makeBark()); },
  actionLabel: 'Bark',
});

export default connect(null, mapDispatchToProps)(Button);
```

そして **bark-message.js**:

```javascript
import { connect } from 'react-redux';
import Message from '../components/message';

const mapStateToProps = state => ({
  message: state.dog.hasBarked ? 'The dog barked' : 'The dog did not bark',
});

export default connect(mapStateToProps)(Message);
```

`BarkButton`は`Button`と`makeBark`アクション、そしてReduxの`dispatch`メソッドを結びつけます。
また、`BarkMessage`はアプリケーションの状態と`Message`を結びつけます。
状態が変更されると、`Message`は自動的に適切な`message`属性を再描画します。
これらは`react-redux`の`connect`関数によって動作します。

- `yarn start`を実行し、`index.html`を開きます。そうすると、"The dog did not bark"とボタンが表示されます。ボタンをクリックすると、"The dog barked"というメッセージが表示されるはずです。

次章 : [10 - Immutable JS と Redux の改良](/tutorial/10-immutable-redux-improvements)

[前章](/tutorial/8-react) または [目次](https://github.com/verekia/js-stack-from-scratch) に戻る。
