# mobx入门

## 对 observables 作出响应

### 0. 基础代码：

```js
import {observable} from 'mobx'
class Store{
  @observable arr = [];
  @observable obj = {a: 1};
  @observable map = new Map();
  @observable str = 'hello';
  @observable num = 123;
  @observable bool = false;
}
const store = new Store();
```

### 1. computed

计算值是可以根据现有的状态或其它计算值衍生出的值,  跟vue中的computed非常相似。

```js
const result = computed(()=>store.str + store.num);
console.log(result.get());
// 监听数据的变化
result.observe((change)=>{
  console.log('result:', change);
})
//两次对store属性的修改都会引起result的变化
store.str = 'world';
store.num = 220;
```

computed可作为装饰器， 将result的计算添加到类中：

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
}
```

### 2. autorun

当你想创建一个响应式函数，而该函数本身永远不会有观察者时,可以使用 `mobx.autorun`

所提供的函数总是立即被触发一次，然后每次它的依赖关系改变时会再次被触发。

经验法则：如果你有一个函数应该自动运行，但不会产生一个新的值，请使用`autorun`。 其余情况都应该使用 `computed`。

```js
//aotu会立即触发一次
autorun(()=>{
  console.log(store.str + store.num);
})

autorun(()=>{
  console.log(store.result);
})
//两次修改都会引起autorun执行
store.num = 220;
store.str = 'world';
```

### 3. when

```
when(predicate: () => boolean, effect?: () => void, options?)
```

`when` 观察并运行给定的 `predicate`，直到返回true。 一旦返回 true，给定的 `effect` 就会被执行，然后 autorunner(自动运行程序) 会被清理。 该函数返回一个清理器以提前取消自动运行程序。

对于以响应式方式来进行处理或者取消，此函数非常有用。

```js
when(()=>store.bool, ()=>{
  console.log('when function run.....');
})
store.bool = true;
```

### 4. reaction

用法: `reaction(() => data, (data, reaction) => { sideEffect }, options?)`。

`autorun` 的变种，对于如何追踪 observable 赋予了更细粒度的控制。 它接收两个函数参数，第一个(*数据* 函数)是用来追踪并返回数据作为第二个函数(*效果* 函数)的输入。 不同于 `autorun` 的是当创建时*效果* 函数不会直接运行，只有在数据表达式首次返回一个新值后才会运行。 在执行 *效果* 函数时访问的任何 observable 都不会被追踪。

```js
// reaction
reaction(()=>[store.str, store.num], (arr)=>{
  console.log(arr.join('/'));
})
//只要[store.str, store.num]中任意一值发生变化，reaction第二个函数都会执行
store.num = 220;
store.str = 'world';
```