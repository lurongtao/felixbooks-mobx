# 在react中使用mobx

在react中使用mobx，需要借助mobx-react。

它的功能相当于在react中使用redux，需要借助react-redux。

首先来搭建环境：

```bash
create-react-app react-app
cd react-app
npm run eject
npm i @babel/plugin-proposal-decorators @babel/plugin-proposal-class-properties -D
npm i mobx mobx-react -S
touch 
```

修改package.json中babel的配置：

```json
  "babel": {
    "presets": [
      "react-app"
    ],
    "plugins": [
      [
        "@babel/plugin-proposal-decorators",
        {
          "legacy": true
        }
      ],
      [
        "@babel/plugin-proposal-class-properties",
        {
          "loose": true
        }
      ]
    ]
  }
```

注意：vscode编译器中，js文件使用装饰器会报红。解决方式：

在根目录编写写jsconfig.json

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