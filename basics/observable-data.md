# mobx入门

## observable可观察的状态

### 1. map

```js
import {observable} from 'mobx'
// 声明
const map = observable.map({a: 1, b: 2});
// 设置
map.set('a', 11);
// 获取
console.log(map.get('a'));
console.log(map.get('b'));
// 删除
map.delete('a');
console.log(map.get('a'));
// 判断是否存在属性
console.log(map.has('a'));
```

### 2. object

```js
import {observable} from 'mobx'
// 声明
const obj = observable({a: 1, b: 2});
// 修改
obj.a = 11;
// 访问
console.log(obj.a, obj.b);
```

### 3. array

```js
import {observable} from 'mobx'
const arr = observable(['a', 'b', 'c', 'd']);
// 访问
console.log(arr[0], arr[10]);
// 操作
arr.pop();
arr.push('e');
```

### 4. 基础类型

```js
import {observable} from 'mobx'/
const num = observable.box(10);
const str = observable.box('hello');
const bool = observable.box(true);
// 获得值
console.log(num.get(), str.get(), bool.get());
// 修改值
num.set(100);
str.set('hi');
bool.set(false);
console.log(num.get(), str.get(), bool.get());
```