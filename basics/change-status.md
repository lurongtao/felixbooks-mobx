# mobx入门

## 改变 observables状态

### 1. action

接上面案例，添加action到类中：

```js
class Store{
  @observable arr = [];
  @observable obj = {a: 1};
  @observable map = new Map();

  @observable str = 'hello';
  @observable num = 123;
  @observable bool = false;

  @computed get result(){
    return this.str + this.num;
  }

  @action bar(){
    this.str = 'world';
    this.num = 40;
  }
}
const store = new Store();

//调用action，只会执行一次
store.bar();
```

### 2. action.bound

`action.bound` 可以用来自动地将动作绑定到目标对象。

```js
class Store{
  @observable arr = [];
  @observable obj = {a: 1};
  @observable map = new Map();

  @observable str = 'hello';
  @observable num = 123;
  @observable bool = false;

  @computed get result(){
    return this.str + this.num;
  }

  @action bar(){
    this.str = 'world';
    this.num = 40;
  }
	
  //this 永远都是正确的
  @action.bound foo(){
    this.str = 'world';
    this.num = 40;
  } 
}

const store = new Store();
setInterval(store.foo, 1000)
```

### 3. runInAction

`runInAction` 是个简单的工具函数,若只需执行一次，可使用`runInAction` 替换`action` 

```js
runInAction(()=>{
  store.num = 220;
  store.str = 'world';
})	
```