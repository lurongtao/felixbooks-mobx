# mobx入门

## observable装饰器

```js
import {observable} from 'mobx'

// observable这个函数可以识别当成普通函数调用还是装饰器调用
// 如果是装饰器，会自动识别数据类型，使用不同的包装转换方案。
class Store{
  @observable arr = [];
  @observable obj = {a: 1};
  @observable map = new Map();
  @observable str = 'hello';
  @observable num = 123;
  @observable bool = false;
}

const store = new Store();

console.log(store);
console.log(store.obj.a);
```

注意：vscode编译器中，js文件使用装饰器会报红。解决方式：

在根目录编写jsconfig.json

```json
{
  "compilerOptions": {
    "module": "commonjs",
    "target": "es6",
    "experimentalDecorators": true
  },
  "include": ["src/**/*"]
}
```