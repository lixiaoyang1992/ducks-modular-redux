# Ducks: Redux Reducer Bundles

<img src="duck.jpg" align="right"/>

我发现当我写一个redux应用时，每添加一个功能的一部分，我总是需要给每一个用例添加 `{actionTypes, actions, reducer}` 元组。我一直把这些分开放在不同的文件，甚至不同的文件夹里，但是百分之九十五的情况，只有一对 reducer/actions需要它关联的action。

对我来说，这些琐碎的代码放到一个独立的模块中更有意义，并且易于打包到依赖库里面。

## 建议

### 示例

亦见: [Common JS Example](CommonJs.md).

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

一个模块

1.必需 `export default` 一个叫 `reducer()` 的函数  
2.必需 `export` action creators 作为函数  
3.必需包涵 action types 以这种格式 `npm-module-or-app/reducer/ACTION_TYPE`  
4.如果另一个 reducer 需要监听这些action或者它是一个发布了的可重用的库，你需要导出 action types 以这种格式 `UPPER_SNAKE_CASE`  

同样的指南也推荐给`{actionType, action, reducer}` bundles 被分享为可重用的Redux库的。

### 名字

Java有jars和beans，Ruby有gems，我建议把这些reducer bundles 叫做"ducks"，作为redux的最后一个音节（注：dux）。

### 使用

你还可以:

```javascript
import { combineReducers } from 'redux';
import * as reducers from './ducks/index';

const rootReducer = combineReducers(reducers);
export default rootReducer;
```

你还可以:

```javascript
import * as widgetActions from './ducks/widgets';
```
...并且它只会引入要传递给`bindActionCreators()`的action creators。

又是你会想`export`一些action creator之外的东西。也可以。这些规则不是说你 *只* 能`export` action creators。当那时，你只需要一一列出你需要的action creators ，没什么大不了的。

```javascript
import {loadWidgets, createWidget, updateWidget, removeWidget} from './ducks/widgets';
// ...
bindActionCreators({loadWidgets, createWidget, updateWidget, removeWidget}, dispatch);
```

### 示例

[React Redux Universal Hot Example](https://github.com/erikras/react-redux-universal-hot-example) uses ducks. See [`/src/redux/modules`](https://github.com/erikras/react-redux-universal-hot-example/tree/master/src/redux/modules).

[Todomvc using ducks.](https://github.com/goopscoop/ga-react-tutorial/tree/6-reduxActionsAndReducers)

### 实践

迁移到这种代码架构很[轻松的](https://github.com/erikras/react-redux-universal-hot-example/commit/3fdf194683abb7c40f3cb7969fd1f8aa6a4f9c57), 并且我预测它减少了未来的开发痛苦.

请提交任何反馈通过issue或者发推给[@erikras](https://twitter.com/erikras)。非常感谢。

祝编码愉快！

-- Erik Rasmussen


### 翻译

[한국어](https://github.com/JisuPark/ducks-modular-redux)

---

![C'mon! Let's migrate all our reducers!](migrate.jpg)
> Photo credit to [Airwolfhound](https://www.flickr.com/photos/24874528@N04/3453886876/).
