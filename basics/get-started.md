# mobx搭建环境

```bash
mkdir my-app
cd my-app
npm init -y
npm i webpack webpack-cli webpack-dev-server -D
npm i html-webpack-plugin -D
npm i babel-loader @babel/core @babel/preset-env -D
npm i @babel/plugin-proposal-decorators @babel/plugin-proposal-class-properties -D
npm i mobx -S
mkdir src
mkdir dist
touch index.html
touch src/index.js
touch webpack.config.js
```

编写webpack.config.js

```js
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  mode: 'development',
  entry: path.resolve(__dirname, 'src/index.js'),
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'main.js'
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: ['@babel/preset-env'],
            plugins: [
              //支持装饰器
              ["@babel/plugin-proposal-decorators", { "legacy": true }],
              ["@babel/plugin-proposal-class-properties", { "loose" : true }]
            ]
          }
        }
      }
    ]
  },
  plugins: [new HtmlWebpackPlugin()],
  devtool: 'inline-source-map'
}
```

编写index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  
</body>
</html>
```