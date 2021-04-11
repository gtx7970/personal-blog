---
title: "webpack"
metaTitle: "This is the title tag of this page"
metaDescription: "This is the vue description"
---

webpack

为何要进行构建和打包

module chunk bundle

module -> 各模块文件 (js, css, 图片...)
chunk -> 多个模块的合成， entry, import(), splitChunk
bundle -> 最终输出的文件

webpack 懒加载

webpack性能优化

### 基本配置

拆分配置, merge

多入口

抽离压缩css

抽离公共代码
```js
module.exports = {
  entry: '',
  // mutiple entry
  entry: {
    index: '',
    other: ''
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        loader: ['babel-loader'],
        include: '',
        exclude: ''
      },
      {
        test: /\.vue$/,
        loader: ['vue-loader']
      },
      {
        test: /\.css$/,
        // right ->  left
        loader: ['style-loader', 'css-loader', 'postcss-loader']
      },
        // 抽离less
      {
        test: /\.less$/,
        loader: [
          MiniCssExtractPlugin.loader,
          'css-loader',
          'less-loader',
          'postcss-loader'
        ]
      },
      {
        test: /\.(png|jpg|jepg|gif)$/,
        options: {
          limit: 5 * 1024,
          outputPath: '/img'
        }
      }
    ]
  },
  output: {
    // 根据内容计算hash
    filename: '[name].[contentHash:8].js',
    path: '',
    publicPath: ''
  },
  plugins: [

    // index.html
    new HtmlWebpackPlugin({
      template: '',
      filename: 'index.html',
      chunks: ['index'， 'vendor', 'common']
    }),

    // other.html
    new HtmlWebpackPlugin({
      template: '',
      filename: 'other.html',
      chunks: ['other']
    }),

    new MiniCssExtracPlugin({
      filename: 'css/mian.[contentHash:8].css'
    })
  ],

  optimization: {
    // compress css
    minimizer: [new TerserJSPlugin({}), new OptimizeCSSAssetPlugin({})]，

    splitChunks: {
      // inital async all
      chunks: 'all',

      cacheGroup: {
        // 第三方模块
        vendor: {
          name: 'vendor',
          priority: 1,
          test: /node_modules/,
          minSize: 0,
          minChunks: 1 // 最少复用
        },

        common: {
          name: 'common',
          priority: 1,
          minSize: 0,
          minChunks: 2  // 最少复用2次以上
        }
      }
    }
  }
}
```

懒加载

```js
// 自定义 chunk
import(/*webpackChunkName app*/'./a,js').then(res => {
  console.log(res.default)
})
```

### webpack性能优化
babel-loader
```js
{
  test: /.js$/,
  use: ['babel-loader?cacheDirectory'],
  include: [],
  exclude: /node_modules/
}
```

Cache-loader
```js
{
  test: /.js$/,
  use: ['cache-loader', 'babel-loader']
}
```

happyPack
多进程打包，提高构建速度
```js
module.exports = {
  module: {
    rule: [
      {
        test: /\.js$/,
        loader: ['happypack/loader?id=babel']
      }
    ]
  },
  plugins: [
    new HappyPack({
      id: 'babel',
      loaders: ['babel-loader?cacheDirectory']
    })
  ]
}
```

thread-loader

parallel-webpack

ParallelUglifyPlugin
多进程压缩
```js
module.exports = {
  plugins: [
    // https://webpack.wuhaolin.cn/4%E4%BC%98%E5%8C%96/4-4%E4%BD%BF%E7%94%A8ParallelUglifyPlugin.html
    new ParallelUglifyPlugin({
      uglifyJS: {
        ouput: {
          beautify: false, // 最紧凑的输出
          comments: false // 删除注释
        }，
        compress: {
          drop_console: true,

          collapse_vars: true,

          reduce_vars: true
        }
      }
    })
  ]
}
```

多进程
项目较大，打包较慢，开启多进程提高速度
项目较小， 进程开销

ignorePlugin
```js
new Webpack.IgnorePlugin({
  resourceRegExp: /^\.\/locale$/,
  contextRegExp: /moment$/
})
```

按需应引入类库

导入时引入特定模块
babel-plugin-loadsh
babel-plugin-import

Externals
将依赖的框从模块构建过程中移除

include/exclude

noParse
```js
module.exports = {
  module: {
    noParse: /jquery|lodash/
  }
}
```

DllPLugin
```js
// DllPlugin
// DllReferrencePlugin
const DllPlugin = require('webpack/lib/DllPlugin')

// webpack.dll.js
module.exports = {
  entry: {
    react: ['react', 'react-dom']
  },
  output: {
    filename: '[name].dll.js',
    library: '_dll_[name]', //_dll_react
  },
  plugins: [
    new DllPLugin({
      name: '_dll_[name]',
      path: path.join(distPath, '[name].manifest.json')
    })
  ]
}

// webpack.dev.js
module.exports = {
  plugins: [
    new DllReferencePlugin({
      manifest: require(path.join(distPath, 'react.mainfest.json'))
    })
  ]
}
```

热更新
网页不刷新，状态不丢失

```js
module.exports = {
  plugins: [
    new HotModuleReplacementPlugin()
  ],
  devserver: {
    hot: true
  }
}

// 开启热更新
if (module.hot) {
  // 设置监听范围
  module.hot.accept(['./a'], () => {
    //
  })
}
```
压缩

TerserWebpackPlugin
UglifyJSPlugin
CSSMinimizerWebpackPlugin

### 优化打包效率

### 优化产出代码
体积更少
合理分包, 不重复加载
速度更快，

小图片 base64编码
bundle 加hash
懒加载
拆分第三方库， 公共代码
IngorePlugin
cdn加速 （publicPath）
production (开始代码压缩， 框架开发阶段的warning， 启动tree-shaking)

esm cjs区别
esm静态引入， 编译时引入 -> 静态分析，实现tree-shaking
cjs 动态引入，执行时引入

scope hosting
```js
// 创建函数作用域更少

const ModuleConcatenationPlugin = require('webpack/lib/optimizeModuleConcatenationPlugin')

module.exports = {
  resolve: {
    mainFields: ['jsnext:main', 'browser', 'main'],
    plugins: [
      new ModuleConcatenationPlugin()
    ]
  }
}

```

webpack5 持久化缓存
```js
module.exports = {
  cache: {
    type: 'filesystem',
    cacheLoaction: path.resolve(__dirname, '.appcache'),
    buildDependencies: {
      config: [__filename]
    }
  }
}
```

### 优化构建速度


### 为何要进行打包、构建
1. 体积更小（tree-shking、压缩、合并），加载更快
2. 编译高级语法(ts, es6+, 模块化， 预处理器)
3. 兼容性，错误检测（polyfill, postcss, eslint)

统一 高效开发流程
统一构建流程，产出标准
集成构建规范()

### Proxy不能被Polyfill

### webpack基本工作流程

```js
const webpack = (options, cb) => {
  options = ''
  let compiler = new Compiler(options.context)
}
```

Compiler hooks

Compilation hooks

```js
const abc = () => {
  console.log()
}
```
