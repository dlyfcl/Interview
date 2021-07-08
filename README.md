# js中bind、apply、call的区别和用法。
答：call、和apply差不多。call（）使用一个指定的this值和其他参数来接收一个函数。function.call(thisArg, arg1, arg2, ...)
apply和call的区别是参数使用数组来接收。function.apply(thisArg, [arg1，arg2,...])
# 防抖和节流讲解一下并说明使用场景。
# debounce 防抖
debounch防抖函数主要应用场景，解决按钮快速点击，多次请求的问题和input输入框多次请求的问题。
export const debounce = (func, delay) => {
  let timer;
  return function (...args) {
    if (timer) {
      clearTimeout (timer);
    }
    timer = setTimeout (() => {
      func.apply (this, args);
      clearTimeout (timer);
    }, delay);
  }
}

# 节流
在无限加载页面时，每隔一段时间去请求ajax, 而不是用户停下滚动时才去加载页面
function throttle(func, delay){
    let prev = new Date();
    return funtion(){
       let contxt = this;
       let args = arguments;
       let now = new Date();
       if(new-prev >= delay){  // 时间间隔大于delay就会触发
           func().apply(contxt, args);
           prev = new Date();
       }
   }
}
# webpack介绍一下，它有哪几个模块，其作用分别是什么。
webpack 是一个前端资源的模块打包器。当 webpack 处理应用程序时，它会递归地构建一个依赖关系图(dependency graph)，其中包含应用程序需要的每个模块，
然后将所有这些模块打包成少量的bundle - 通常只有一个，由浏览器加载。

他的主要组成是：入口(entry)、输出(output)、loader、插件(plugins)
# entry：入口是webpack创建应用的起点。每个html都应该有一个入口，spa项目有一个mpa则应该有多个
单个模式
module.exports = {
  entry: './src/index.js'
};
多个模式
module.exports = {
  entry: {
    main: './src/main.js'
    index:'./src/index.js'
  }
};
# output：输出是告诉webpack打包输出在哪里的设置
const path = require('path')  
module.exports = {
  output:{

    filename:'[name].js',  //使用占位符---这样写是根据原入口文件名称输出出口的文件名称。

    path:path.resolve(__dirname,'dist')  //输出到当前文件夹下的dist目录。   

  }

}

# loader：可以将文件源代码转换为模块。可以在import或"加载"模块时预处理文件。（4.0中改为rules）
 const path = require('path');
  module.exports = {
    entry: './src/file.js',
    output: {
      path: path.resolve(__dirname, 'dist'),
      filename: 'my-first-webpack.bundle.js'
    },
    module: {
      rules: [
        { 
          test: /\.txt$/,      //正则匹配文件。
          use: 'raw-loader'   //安装raw-loader yarn add row-loader -D
        }
      ]
    }
  };

# plugins：借助各种插件扩展webpack功能
  const HtmlWebpackPlugin = require('html-webpack-plugin');
  const webpack = require('webpack'); //webpack 内置的一些插件
  const path = require('path');
  const config = {
    entry: './src/file.js',
    output: {
      path: path.resolve(__dirname, 'dist'),
      filename: 'my-first-webpack.bundle.js'
    },
    module: {
      rules: [
        { test: /\.txt$/, use: 'raw-loader' }
      ]
    },
    plugins: [
      new HtmlWebpackPlugin({template: './src/index.html'})  //模板使用的模板
    ]
  };
# resolve：配置解析模块规则
extensions: 自动补充文件的后缀名
alias属性配置文件的别名

resolve: {

  extensions: [".js", ".vue", ".json", ".ts", ".tsx"],

  alias: {

    vue$: "vue/dist/vue.js",

    "@": path.resolve(__dirname, "./src"),

    "@c": path.resolve(__dirname, "./src/components"),

    utils: path.resolve(__dirname, "./src/utils"),

    views: path.resolve(__dirname, "./src/views"),

    assets: path.resolve(__dirname, "./src/assets"),

    com: path.resolve(__dirname, "./src/components"),

    store: path.resolve(__dirname, "./src/store"),

  }

}
# Mode： 模块化， 区分开发模式和生产模式。

深入问的话就是 plugins和loader的区别。

# vue中keep-alive的作用以及实现原理

keep-alive实现原理就是将对应的状态放入一个cache对象中，对应的dom节点放入缓存dom中，当下次再次需要渲染时，
从对象中获取状态，从缓存dom中移出至挂载dom节点中。

# vuex的几个模块和作用。

state：定义了应用程序的数据，可以设置默认的初始状态。
getters：允许组件从 store 中获取数据 ，对state进行额外计算或设置的方法都放在getters里面，并暴露出去调用。
mutations：是唯一更改 store 中状态的方法，且必须是同步函数。但不可以直接调用mutation，必须使用commit函数来告诉Vuex更新存储并提交更改。
actions：执行异步操作来存取状态，但也不可以直接调用action，必须使用dispatch函数来执行。

#  vuex的state = "xxx" 和 commit/mutation的用法的区别在哪。

严格模式下第一种是不被允许的，会报错。

#  es6的新语法和特性介绍。

#  解构赋值的使用。

# js的数据类型有哪些。

string number boolean array object undefined null。
typeof(null) = ? 答： object

#  对于NaN、Infinity的理解。

js中数字除以0等于多少？（Infinity 无穷大）   0除以0等于多少。（NaN） 

#  判断一个数是否是NaN的方法。
Number.isNaN(NaN) // 判断是否是NaN
Number.isFinite(222) // 判断是否是有限数

# 举例说明es6中的class的用法。

# 字符串拼接的方法。

cancat合并数组的话是生成新数组还是改变原数组。

# vue中 key 值的作用

    使用key来给每个节点做一个唯一标识

    key的作用主要是为了高效的更新虚拟DOM。另外vue中在使用相同标签名元素的过渡切换时，也会使用到key属性，其目的也是为了让vue可以区分它们，

否则vue只会替换其内部属性而不会触发过渡效果。

#  如果页面挂载了一个对象的，然后对象新增了一个b属性，页面会发生变化吗？如果不会该怎么做？

不会，用vue的set方法更新对象

# MVVM的理解。

# 多维数组的去重方式
1. 转为对象
2. filter + indexOf 如下：
const arr = [1, 2, 2, 3, 5, 2, 3, 1];
const deduplicate = arr => 
  arr.filter((it, ix) => arr.indexOf(it) === ix)
  
# 任务队列
async function async1(params) {
  console.log("async1 start");
  await async2();
  console.log("async1 end");
}

async function async2(params) {
  console.log("async2");
}

console.log("script start");

setTimeout(() => {
  console.log("setTimeout");
}, 0)

async1();

new Promise(function(resolve) {
  console.log("promise1");
  resolve();
}).then(function() {
  console.log("promise2");
})
console.log("script end");

// script start
// async1 start
// async2
// promise1
// script end
// async1 end
// promise2
// setTimeout

# 判断是否是数组 arr

1.Array.isArray(arr)
2.arr instanceof Array
3.Array.prototype.isPrototypeOf(arr)
4.Object.prototype.toString(arr) === '[object Array]'

# eslint不让它检测某段代码

单行跳过
// eslint-disable-line  需要跳过检测的行在代码后面加上这个

多行跳过
/* eslint-disable */
export function getAddressByLngLat (lng, lat) {
  return new Promise((resolve) => {
    let myGeo = new BMap.Geocoder()
    myGeo.getLocation(new BMap.Point(lng, lat), function (result) {
      if (result) {
        resolve(result)
      }
    })
  })
}
/* eslint-enable */

# for of || for in

for of 循环数组得到数组的值，for in得到数组的index
for of 不能循环对象， for in循环对象得到key

# 关于闭包

闭包是能够读取其他函数内部变量的函数。
使用场景：setTimeout 回调函数 防抖函数 封装私有变量

# vue的nextTick原理

作用： 它可以在DOM更新完毕之后执行一个回调
使用场景： vue中异步加载数据，数据更新 => dom更新后才能进行 scroll-better 的初始化操作。
nextTick是全局vue的一个函数，在vue系统中，用于处理dom更新的操作。vue里面有一个watcher，用于观察数据的变化，然后更新dom，vue里面并不是每次数据改变都会触发更新dom，而是将这些操作都缓存在一个队列，在一个事件循环结束之后，刷新队列，统一执行dom更新操作。
vue用异步队列的方式来控制DOM更新和nextTick回调先后执行
micro-task因为其高优先级特性，能确保队列中的微任务在一次事件循环前被执行完毕
因为兼容性问题，vue不得不做了microtask向macrotask的降级方案

# vDom的原理

# vue中的方法变异是为什么

改变原数组,使视图更新

# promise.all是同步还是异步

里面顺序执行但是互不影响，从结果上来看是异步的，返回的数组里面的顺序也对应着传入的数组顺序

# Promise.race()
语法和all()一样，但是返回值有所不同，race根据传入的多个Promise实例，只要有一个实例resolve或者reject，就只返回该结果，其他实例不再执行。

# http的缓存机制

缓存分为两种：强缓存和协商缓存
1.强缓存：不会向服务器发送请求，直接从缓存中读取资源，在chrome控制台的Network选项中可以看到该请求返回200的状态码，并且size显示from disk cache或from memory cache两种（灰色表示缓存）。

2.协商缓存：向服务器发送请求，服务器会根据这个请求的request header的一些参数来判断是否命中协商缓存，如果命中，则返回304状态码并带上新的response header通知浏览器从缓存中读取资源；

共同点：都是从客户端缓存中读取资源；
区别是强缓存不会发请求，协商缓存会发请求。

# 浏览器从输入一个路由到展示一共经历了哪些步骤

输入url
dns查询到ip
建立tcp连接
客户端发送http请求
服务器处理请求
服务器响应请求
浏览器展示html
浏览器发送请求回去其他在html中的资源

# url从创建到页面显示一共经过了哪些步骤

1.输入url
2.dns解析域名
3.建立tcp链接
4.客户端发送http请求
5.服务器处理并响应http请求
6.浏览器展示html
7.浏览器获取css、js等资源
