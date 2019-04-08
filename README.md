# Ducks: Redux Reducer Bundles

<img src="duck.jpg" align="right"/>
我发现在创建redux应用时，按照功能性划分，每次会都添加 `{actionTypes, actions, reducer}` 这样的组合。我之前会把它们分成不同的文件，甚至分到不同的文件夹，但是95%的情况下，只有一对 reducer/actions 会用到对应的 actions。
对我来说，把这些相关的代码放在一个独立的文件中更方便，这样做还可以很容易的打包到软件库／包中。

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

上述规则也推荐用在可重用的redux 库中用来组织`{actionType, action, reducer}`

### 名称

Java 有 jars 和 beans. Ruby 有 gems. 我建议我们把这些 reducer 文件包称为 "ducks"，也就是 "redux" 的结尾音。

### 用法

你可以这样:

```javascript
import { combineReducers } from 'redux';
import * as reducers from './ducks/index';

const rootReducer = combineReducers(reducers);
export default rootReducer;
```

你也可以这样:

```javascript
import * as widgetActions from './ducks/widgets';
```
... 这样它只会引入action creators, 可以传给`bindActionCreators()`.

如果你除了action creator 以外，还 `export` 了其他的变量/函数，也没有问题。上面的规则并不是说你 *只能* `export` action creators 。如果是这种情况，你需要做的就是把你需要的action creator 引入，然后传给 bindActionCreators 

```javascript
import {loadWidgets, createWidget, updateWidget, removeWidget} from './ducks/widgets';
// ...
bindActionCreators({loadWidgets, createWidget, updateWidget, removeWidget}, dispatch);
```

### 示例

[React Redux Universal Hot Example](https://github.com/erikras/react-redux-universal-hot-example) uses ducks. See [`/src/redux/modules`](https://github.com/erikras/react-redux-universal-hot-example/tree/master/src/redux/modules).

[Todomvc using ducks.](https://github.com/goopscoop/ga-react-tutorial/tree/6-reduxActionsAndReducers)

### 实现

 迁移代码结构的过程堪称[无痛](https://github.com/erikras/react-redux-universal-hot-example/commit/3fdf194683abb7c40f3cb7969fd1f8aa6a4f9c57), 我能够预见到这样做会减少很多的开发痛苦。

有任何问题或者反馈欢迎通过新建github issue 或者 tweet 到 [@erikras](https://twitter.com/erikras)。

编码愉快!

-- Erik Rasmussen


### Translation

[한국어](https://github.com/JisuPark/ducks-modular-redux)
[中文](https://github.com/deadivan/ducks-modular-redux)
---

![C'mon! Let's migrate all our reducers!](migrate.jpg)
> Photo credit to [Airwolfhound](https://www.flickr.com/photos/24874528@N04/3453886876/).

---

[![Beerpay](https://beerpay.io/erikras/ducks-modular-redux/badge.svg?style=beer-square)](https://beerpay.io/erikras/ducks-modular-redux)  [![Beerpay](https://beerpay.io/erikras/ducks-modular-redux/make-wish.svg?style=flat-square)](https://beerpay.io/erikras/ducks-modular-redux?focus=wish)
