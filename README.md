# Ducks: Redux Reducer Bundles

<img src="duck.jpg" align="right"/>
我发现在创建redux应用时，按照功能性划分，每次都添加`{actionTypes, actions, reducer}`这样的组合。我之前会把它们分成不同的文件，甚至分到不同的文件夹，但是95%的情况下，只有一对 reducer/actions 会用到对应的 actions。
对我来说，把这些相关的代码放在一个独立的文件中更方便，这样还能够很容易的打包到软件库／包中。

## 提议
### 示例

可参见: [Common JS Example](CommonJs.md).

```javascript
// widgets.js

// Actions
const LOAD   = 'my-app/widgets/LOAD';
const CREATE = 'my-app/widgets/CREATE';
const UPDATE = 'my-app/widgets/UPDATE';
const REMOVE = 'my-app/widgets/REMOVE';

// Reducer
export default function reducer(state = {}, action = {}) {
  switch (action.type) {
    // do reducer stuff
    default: return state;
  }
}

// Action Creators
export function loadWidgets() {
  return { type: LOAD };
}

export function createWidget(widget) {
  return { type: CREATE, widget };
}

export function updateWidget(widget) {
  return { type: UPDATE, widget };
}

export function removeWidget(widget) {
  return { type: REMOVE, widget };
}
```
### 规则

一个模块 ...

1. **必须** `export default` 函数名为 `reducer()` 的 reducer
2. **必须**  作为函数 `export` 它的 action creators 
3. **必须**  把 action types 定义成形为 `npm-module-or-app/reducer/ACTION_TYPE` 的字符串
3. 如果有外部的reducer需要监听这个action type，或者作为可重用的库发布时， **可以**  用 `UPPER_SNAKE_CASE` 形式 export 它的 action types。

These same guidelines are recommended for `{actionType, action, reducer}` bundles that are shared as reusable Redux libraries.

### Name

Java has jars and beans. Ruby has gems. I suggest we call these reducer bundles "ducks", as in the last syllable of "redux".

### Usage

You can still do:

```javascript
import { combineReducers } from 'redux';
import * as reducers from './ducks/index';

const rootReducer = combineReducers(reducers);
export default rootReducer;
```

You can still do:

```javascript
import * as widgetActions from './ducks/widgets';
```
...and it will only import the action creators, ready to be passed to `bindActionCreators()`.

There will be some times when you want to `export` something other than an action creator. That's okay, too. The rules don't say that you can *only* `export` action creators. When that happens, you'll just have to enumerate the action creators that you want. Not a big deal.

```javascript
import {loadWidgets, createWidget, updateWidget, removeWidget} from './ducks/widgets';
// ...
bindActionCreators({loadWidgets, createWidget, updateWidget, removeWidget}, dispatch);
```

### Example

[React Redux Universal Hot Example](https://github.com/erikras/react-redux-universal-hot-example) uses ducks. See [`/src/redux/modules`](https://github.com/erikras/react-redux-universal-hot-example/tree/master/src/redux/modules).

[Todomvc using ducks.](https://github.com/goopscoop/ga-react-tutorial/tree/6-reduxActionsAndReducers)

### Implementation

The migration to this code structure was [painless](https://github.com/erikras/react-redux-universal-hot-example/commit/3fdf194683abb7c40f3cb7969fd1f8aa6a4f9c57), and I foresee it reducing much future development misery.

Please submit any feedback via an issue or a tweet to [@erikras](https://twitter.com/erikras). It will be much appreciated.

Happy coding!

-- Erik Rasmussen


### Translation

[한국어](https://github.com/JisuPark/ducks-modular-redux)

---

![C'mon! Let's migrate all our reducers!](migrate.jpg)
> Photo credit to [Airwolfhound](https://www.flickr.com/photos/24874528@N04/3453886876/).

---

[![Beerpay](https://beerpay.io/erikras/ducks-modular-redux/badge.svg?style=beer-square)](https://beerpay.io/erikras/ducks-modular-redux)  [![Beerpay](https://beerpay.io/erikras/ducks-modular-redux/make-wish.svg?style=flat-square)](https://beerpay.io/erikras/ducks-modular-redux?focus=wish)
