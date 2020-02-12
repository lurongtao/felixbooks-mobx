# React + Mobx 案例

## 修改index.js

应用 react-mobx-app 创建完项目基本环境，修改项目根目录下的 index.js:

```js
import React from 'react'
import ReactDOM from 'react-dom'
import App from './App'

ReactDOM.render(
  <App/>,
  document.querySelector('#root')
)
```

## 创建 store

在项目根目录下创建store目录，在store目录下创建index.js，内容如下：

```js
import { observable, action, computed } from 'mobx'

class AppStore {

  @observable title = 'mobx'
  @observable todos = []

  @computed get desc(){
    return `一共${this.todos.length}条`
  }

  @action addTodo(todo) {
    this.todos.push(todo)
  }
  @action deleteTodo() {
    this.todos.pop()
  }
  @action resetTodo() {
    this.todos = []
  }
}

const store = new AppStore()

export default store
```

## 修改App.js

修改项目根目录下的 App.js

```js
import React, { Component } from 'react'
import Home from './pages/Home'
import { Provider } from 'mobx-react'
import store from './store/'
export default class App extends Component {
  render() {
    return (
      <Provider store={store}>
        <Home></Home>
      </Provider>
    )
  }
}
```

## 创建 Home.js

在项目根目录下创建 pages 文件夹，在 pages文件夹下创建 Home.js 组件，内容如下：

```js
import React, { Component } from 'react'
import {inject, observer} from 'mobx-react'

@inject('store') 
@observer
class Home extends Component {
  render() {
    const {store} = this.props
    return (
      <div>
        <div>{store.title}</div>
        <button onClick={this.handleTodos.bind(this, 'add')}>添加</button>
        <button onClick={this.handleTodos.bind(this, 'delete')}>删除</button>
        <button onClick={this.handleTodos.bind(this, 'reset')}>重置</button>
        <h6>{store.length}</h6>
        {
          store.todos.map((ele, index)=>(
            <div key={index}>
              {ele}
            </div>
          ))
        }
      </div>
    )
  }
  handleTodos(type){
    const {store} = this.props
    switch (type) {
      case 'add':
        store.addTodo('这是一条新内容')
        break
      case 'delete':
        store.deleteTodo()
        break
      case 'reset':
        store.resetTodo()
        break
      default:
        break
    }
  }
}
export default Home;
```