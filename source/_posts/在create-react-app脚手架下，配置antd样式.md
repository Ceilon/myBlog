---
title: 在create-react-app脚手架下，配置webpack
date: 2019-08-03 15:25:14
categories: 代码
thumbnail: https://gw.alipayobjects.com/zos/rmsportal/KDpgvguMpGfqaHPjicRK.svg
tags:
---

>- ### 配置creact-react-app支持antd样式
>
>#### 1.配置webpack.config.js的baelrc设置为true
>
>```js
>            {
>              test: /\.(js|mjs)$/,
>              exclude: /@babel(?:\/|\\{1,2})runtime/,
>              loader: require.resolve('babel-loader'),
>              options: {
>                babelrc: true,//将次设为true即可
>                configFile: false,
>                compact: false,
>                presets: [
>                  [
>                    require.resolve('babel-preset-react-app/dependencies'),
>                    { helpers: true },
>                  ],
>                ],
>                cacheDirectory: true,
>                cacheCompression: isEnvProduction,
>                
>                // If an error happens in a package, it's possible to be
>                // because it was compiled. Thus, we don't want the browser
>                // debugger to show the original code. Instead, the code
>                // being evaluated would be much more helpful.
>                sourceMaps: false,
>              },
>            }
>```
>
>#### 2.配置package.json中的babel项
>
>```json
>  "babel": {
>    "presets": [
>      "react-app"
>    ],
>    "plugins": [
>      [
>        "import",
>        {
>          "libraryName": "antd",
>          "style": "css"
>        }
>      ]
>    ]
>  }
>```
>
>#### 3.安装依赖包
>
>`npm i babel-plugin-import --save`

