# NodeJs

2020-02-15 19:18

## 安装

下载地址：http://nodejs.cn/

安装包方式安装：https://www.cnblogs.com/jywang/p/15721411.html



## 新建nodejs项目

idea新建 ---》javaScript ---》Node.js 

新建HelloWorld.js文件   输入

```js
console.log("hello world!!!")
```

确保idea安装node.js插件 

右键运行



## 模拟tomcat

新建 httpserver.js 文件

```js
//导入模块是require  就类似与import java.io包
const http = require('http')

//1.创建一个httpserver服务
http.createServer(function (request,response) {

    //浏览器怎么认识hello world！！！
    response.writeHead(200,{'Content-type':'text/plain'});//这句话含义是：告诉浏览器将以text/plain去解析hello server这段代码
    //给浏览器输出的内容
    response.end("hello world!!!!");
})
    //2.监听一个端口8888
    .listen(8888);
console.log("你启动的服务是：http://localhost:8888以启动成功！")
//3.启动运行服务 node httpserver.js

//4.在浏览器访问 http://localhost:8888
```



## Node-操作MYSQL数据库

参考 https://www.npmjs.com/package/mysql

1.安装mysql依赖

在项目终端 Terminal

```sql
npm install mysql
```

2.创建db.js



# ES6

ECMAScript 6（简称ES6）是于2015年6月正式发布的JavaScript语言的标准，正式名为ECMAScript 2015（ES2015）。它的目标是使得JavaScript语言可以用来编写复杂的大型应用程序，成为企业级开发语言 
另外，一些情况下ES6也泛指ES2015及之后的新增特性，虽然之后的版本应当称为ES7、ES8等。

1998.06 es2.0发布

1999.12 es3.0发布

2007.10 es4.0起草

2009.12 es5.2发布

2011.06 es5.1发布

2015.06 es6.0发布



## ES6的语法：let和const命令

> 变量和常量的严格区分

```js
/**
     * let和const解决
     * 1.var的变量穿透的问题
     * 2.常量修改的问题
     */
    for(var i = 0;i<5;i++){
        console.log(i);
    }
    //这里就造成变量穿透
    console.log(i)

    for(let i = 0;i<5;i++){//修改为let可变为局部变量
        console.log(i);
    }
    //这里就造成变量穿透
    console.log(i)//报错  i未定义

    console.log("==============================================")

    //PI = 3.1415926535  var定义可以被修改
    var PI = Math.PI;
    PI = 100;
    console.log(PI);

    //const 定义的常量不可被修改
    const PI1 = Math.PI;
    PI1 = 100;
    console.log(PI1);

    //在实际开发和生产中,如果是小程序，uniapp或者一些脚手架的，可以大胆的去使用let和const
    //但是如果你是web开发。建议大家还是使用var，因为在一些低版本的浏览器还是不支持let和const
```



## ES6的语法：模板字符串

> 以前是使用 ''  或者  "" 来把字符串套起来
>
> 现在：``  【反引号   键盘 esc下面 tab上面】

```js
    //字符串会牵涉到动态部分
    var person = {
        name : "chj",
        address : "杭州"
    };
    let address = "我"+person.name+"目前在"+person.address;
    console.log(address)

    //es6的语法模板字符串语法
    let address2 = `我${person.name}目前在${person.address}`;
    console.log(address2)

	//支持换行
    let address3 = `我${person.name}
			目前在${person.address}`;
    console.log(address3)
```





## ES6的语法：函数默认参数与箭头函数

### 函数默认参数

> 函数默认参数：在方法的参数后面加上一个默认值即可

```js
 //函数默认参数
    function sum(a,b) {
        return a + b;
    }
    var result = sum(3,4);
    console.log(`result = ` + result);// 此时 a = 3, b = 4   3 + 4 = 7
    var result1 = sum(3);
    console.log(`result = ` + result1); // 此时 a = 3, b = undefined  , 3 + undefined = NAN

    //ES6 传递一个默认的参数
    function sum(a,b = 100) {
        return a + b;
    }
    var result2 = sum(3,4);
    console.log(`result = ` + result2);// 此时 a = 3, b = 4   3 + 4 = 7
    var result3 = sum(3);
    console.log(`result = ` + result3);// 此时 a = 3, b没有传参 使用默认值100  , 3 + 100 = 103
```



### 箭头函数

```js
        //箭头函数 - 重点 （在未来的项目开发中，比如小程序，unApp，一些常见的脚手架大量使用）
        var sum = function (a,b) {
            return a + b;
        }

        //ES箭头函数改进1
        var sum = (a,b) =>{
            return a + b;
        }

        //ES箭头函数改进2
        var sum = (a,b) => a + b;

        //类似于lambda表达式
        /**
         * 1:去掉function
         * 2:在括号后面加箭头
         * 3:如果逻辑代码仅有return可以直接省去（如果有逻辑体，就不能省略）
         * 4:如果参数只有一个，可以把括号也省去（如果有多个参数就不能省去）
         */

        //例1
        var arr = [1,2,3,4,5,6];
        var newArr = arr.map(function (obj) {
            return obj * 2;
        });
        console.log(newArr);

        var newArr1 = arr.map( obj =>  obj * 2);
        console.log(newArr1);
```



## ES6的语法：对象初始化简写

> 它是指：如果一个对象中的key和value的名字一样的情况下可以定义成一个

```js
    //定义对象
    var name = "chj";
    var address = "杭州";
    var info = {
        name : name,
        address : address,
        go:function () {
            console.log("我在杭州！")
        }
    };

    //ES6简写
    /**
     * 1:如果key和变量的名字一致，可以只定义一次
     * 2:如果value是一个函数，可以把 `:function`去掉，只剩下（）即可
     */
    let names = "chj";
    let addresss = "杭州";
    let infos = {
        names,
        addresss,
        gos() {
            console.log("我在杭州！")
        }
    };
    console.log(infos.names);
    console.log(infos.addresss);
    infos.gos();


    //例1
    $.post("/index",{name:name,address:address},function () {
                console.log("以前的写法")
    })
    $.post("/index",{name,address},function () {
        console.log("ES6的写法")
    })

```





## ES6的语法：对象解构

>对象解构 ——es6提供一些获取快捷获取对象属性和行为的方式

```js
    /**
     *  对象是key:value 存在，获取对象属性和方法的方式有两种
     *  1:通过。
     *  2:通过[]
     */
    var name = "chj";
    var address = "杭州";
    var info = {
        name,
        address,
        go() {
            console.log("我在杭州！")
        }
    };
    //通过.的方式
    console.log(info.name);
    console.log(info.address);
    info.go();

    //通过[]的方式
    console.log(info["name"]);
    console.log(info["address"]);
    info["go"]();

    //ES6对象解构  - 其实就是快速获取属性和方法的方式一种方式
    var {name,address,go} = info;
    //还原代码
    //var name = info.name;
    //var address = address.name;
    console.log(name);
    console.log(address);
    go();

```





## ES6的语法：传播操作符【...】

> 把一个对象的属性传播到另一个对象中

```js
    //对象传播操作符
    var person = {
        name : "chj",
        address : "杭州",
        phone : "123456",
        go(){
            console.log("我在杭州！");
        }
    }
    //解构出来
    var {name,phone,...person2} = person;//person 中的属性name,address,phone,go 其中name,phone已经被解构出来  person2就是剩下的所有属性已经方法（address和go）
    console.log(name);
    console.log(phone);
    console.log(person2);
```



## ES6的语法：数组map和reduce方法使用

### Map（）

> 方法可以将原数组中的所有元素通过一个函数进行处理并放入到一个新数组中并返回该新数组

```js
    //要对arr数组每个元素*2
    let arr = [1,2,3,4,5,6];
    //传统方式
    let newarr = [];
    for(let i=0;i<arr.length;i++){
        newarr.push(arr[i]*2)
    }
    console.log(newarr);


    //ES6 map -- 自带的循环,并且会把处理的值回填对应的位置
    let newarr2 = arr.map(function (ele) {
            return ele*2
    });
    console.log(newarr2);

    let  newarr3 = arr.map(ele=>ele*2);
    console.log(newarr3);

    //map处理对象的数据
    let users = [{age:10,name:"张三"},{age:15,name:"李四"},{age:12,name:"王五"}];
    let user =  users.map(function (ele) {
          ele.age = ele.age+1;
          ele.check = true;//在数组里添加一个元素
        return ele;
    })
    console.log(users)
```



### reduce（）

> reduce（function（），初始值（可选））
>
> 接收一个函数（必须）和一个初始值（可选），该函数接收两个参数
>
> - 第一个参数是上一次reduce处理的结果
> - 第二个参数是数组中要处理的下一个元素
>
> reduce（）会从左到右依次把数组中的元素用reduce处理，并把处理的结果作为下次reduce的第一个参数。如果是第一次，会把前两个元素作为计算参数，或者把用户指定的初始值作为起始参数

```js
    let arr = [1,2,3,4,5,6,7,8,9,10];
    //第一次循环  a = 1 , b = 2 , a+b=3
    //第二次循环  a = 3 , b = 3 , a+b=6
    //第三次循环  a = 6 , b = 4 , a+b=10
    //.....
    let result = arr.reduce(function (a,b) {
        return a + b;
    })
    console.log(result)


```





# NPM包管理器

## 简介

官方网站：https://www.npmjs.com/

NPM全称Node Package Manager，是Node.js包管理工具，是全球最大的模块生态系统，里面所有的模块都是开源免费的；也是Node.js的包管理工具，相当于前端的Maven

```js
#在命令提示符输入npm -v 可查看当前npm版本
npm -v
```



## 使用npm管理项目

### 创建文件夹npm

### 项目初始化

```js

        -npm init（-npm init -y 自动帮我们定义好下面的内容）
            -得到package.json 这个文件里的内容如下：
            {
              "name": "one_nodejs", //工程名
              "version": "1.0.0",   //版本
              "main": "index.js",   //如果js
              "scripts": {          //运行脚本
                "test": "echo \"Error: no test specified\" && exit 1"
              },
              "keywords": [],
              "author": "",         //开发者
              "license": "ISC",     //授权协议
              "dependencies": {     //安装的模块
                "mysql": "^2.18.1"
              },
              "devDependencies": {},
              "description": ""//描述
            }
            类似于：pom.xml 文件作用管理依赖
```

### 修改npm镜像

> NPM官方的管理的包都是从 http://npmjs.com 下载的，但是这个网站在国内速度很慢
>
> 这里推荐使用淘宝NPM镜像 http://npm.taobao.org/
>
> 淘宝NPM镜像是一个完整npmjs.com镜像，同步频率目前为10分钟一次，以保证尽量与官方服务同步

```js
#经过下面的配置，以后所有的 npm install 都会经过淘宝的镜像地址下载
npm install -g cnpm --registry=https://registry.npm.taobao.org
#查看npm配置信息
npm config list
```



# Babel

## 简介

​	ES6的某些高级语法在浏览器环境甚至Node.js环境中无法执行

​	Babel是一个广泛使用的转码器，可以将ES6代码转换为ES5代码，从而在现有环境执行

​	这意味着，你可以现在就用ES6编写程序，而不用担心现有环境是否支持

## 安装

安装命令行转码工具

> Babel提供babel-cli工具，用于命令行转码

```js
npm intall -g babel-cli

#查看是否安装成功
babel --version
```

## 使用

创建BabelDemo/exmaple.js

```js
//es6
let name = "chj";
const title = "杭州";
let arr = [1,2,3,4,5,6,7,8,9,10];
let newArr = arr.map(a=>a*2);
console.log(name);
console.log(title);
console.log(arr);
console.log(newArr);
```

根目录下创建  .babelrc

```js
{
  "presets": ["es2015"],
  "plugins": []
}
```

 安装 es2015

```js
cnpm install --save-dev babel-preset-es2015
```

 命令行输入

```js
babel BabelDemo -d dist
#babel 源文件夹  -d 新文件夹  进行语法转换
```

打开dist文件夹

得到exmaple.js

```js
"use strict";

//es6
var name = "chj";
var title = "杭州";
var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
var newArr = arr.map(function (a) {
  return a * 2;
});
console.log(name);
console.log(title);
console.log(arr);
console.log(newArr);
```



## 自定义脚本

```js
{
  "name": "one_nodejs",
  "version": "1.0.0",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    #新增语法转换脚本   "dev" : "babel BabelDemo -d dist"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "mysql": "^2.18.1"
  },
  "description": "",
  "devDependencies": {
    "babel-preset-es2015": "^6.24.1"
  }
}


命令行  输入
#npm run dev
```



# 模块化开发

## 简介

模块化产生的背景

随着网站逐渐变成“互联网应用程序”，嵌入网页的JavaScript代码越来越庞大，越来越复杂

JavaScript模块化编程，已经成为一个迫切的需求。理想情况下，开发者只需要实现核心的业务逻辑，其他都可以加载别人已经写好的模块。但是，JavaScript不是一种模块化编程语言，它不支持类（class）,包（package）等概念，也吧支持模块（module）

**模块化规范**

- CommonJS模块化规范
- ES6模块化规范



## CommonJS规范

> 每个文件就是一个模块，有自己的作用域。在一个文件里面定义的变量、函数、都是私有的，对其他文件不可见

创建module文件夹

创建commonjs文件夹

创建四则运算.js文件

```js
//工具类
const sum = function (a,b) {
    return a + b;
}
const sub = function (a,b) {
    return a - b;
}
const mul = function (a,b) {
    return a * b;
}
const di = function (a,b) {
    return a / b;
}

//导出给别人使用
module.exports = {
    sum,sub,mul,di
}
```

创建导入测试.js文件 测试

```js
//require
const m = require("./四则运算.js");

console.log(m.sum(1,2));
console.log(m.sub(2,1));
console.log(m.mul(1,2));
console.log(m.di(1,2));
```



## ES6规范

> ES6使用 export和import来导出、导入模块

创建module文件夹

创建es6文件夹

创建useApi.js文件

```js
export function getList() {
    //真实业务中，异步获取数据
    console.log("获取数据列表")
}

export function save() {
    //真实业务中，异步获取数据
    console.log("保存数据")
}
```

创建userTest.js文件 测试

```js
import {getList,save} from "./userApi.js";
getList();
save();

//默认不支持es6语法的  使用babel转换 (参考Babel)
```



## ES6规范2

> ES6使用 export和import来导出、导入模块

创建module文件夹

创建es6（2）文件夹

创建useApi.js文件

```js
//前端开发中，经常看到这样的代码
export default {
    getList2() {
        //真实业务中，异步获取数据
        console.log("获取数据列表")
    },
    save2() {
        //真实业务中，异步获取数据
        console.log("保存数据")
    }
}

```

创建userTest.js文件 测试

```js
import user from '../es6(2)/userApi'
user.getList2();
user.save2();

//默认不支持es6语法的  使用babel转换 (参考Babel)
```



# Webpack

Webpack是一个前端资源加载/打包工具。它将根据模块的依赖关系进行静态分析，然后将这些模块按照指定的规则生成对应的静态资源

Webpack可以将多种静态资源js、css、less转换成一个静态文件，减少了页面的请求

## 安装

1.全局安装

```js
npm install -g webpack webpack-cli
```

2.安装后查看版本号

```js
webpack -v
```



## 使用

### JS打包

1. 创建一个nodejs项目 

2. 创建一个src目录

3. 在src存放两个需要合并的util.js和common.js

   1. common.js

      ```js
      //输出
      exports.info = function (str) {
          //往控制台输出
          console.log(str);
          //往浏览器输出
          document.write(str);
      }
      ```

   2. util.js

      ```js
      //相加函数
      exports.add  =(a,b) => a + b;
      ```

4. 准备一个入口文件main.js ，其实就是模块集中进行引入

   main.js

   ```js
   //导入utils
   const util  =require('./utils');
   //导入common
   const common  =require('./common');
   
   common.info("hello world , "+util.add(1,2));
   ```

   

5. 在根目录下定义个webpack.config.js文件配置打包的规则

   ```js
   //导入path模块 nodejs内置模块
   const path = require("path");
   //定义JS打包规则
   module.exports = {
       //入口函数从哪里开始进行编译打包
       entry : "./src/main.js",
       //编译成功以后吧内容输出位置
       output:{
           //定义输出的指定目录 __dirname当前项目根目录，产生一个dist文件夹
           path:path.resolve(__dirname,"./dist"),
           //合并的js文件存储在dist/bundle.js文件中
           filename:"bundle.js"
       }
   }
   
   ```

6. 命令行执行webpack查看效果（经过打包加密后的文件）(webpack -w 可以监听自动实施打包)

   bundle.js

   ```js
   (()=>{var o={648:(o,r)=>{r.info=function(o){console.log(o),document.write(o)}},555:(o,r)=>{r.add=(o,r)=>o+r}},r={};function n(t){var e=r[t];if(void 0!==e)return e.exports;var d=r[t]={exports:{}};return o[t](d,d.exports,n),d.exports}(()=>{const o=n(555);n(648).info("hello world , "+o.add(1,2))})()})();
   ```

   可以直接引入到html文件中

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <title>Title</title>
       <script src="bundle.js"></script>
   </head>
   <body>
   
   </body>
   </html>
   ```

   

### css打包

> Webpack 本身只能处理JavaScript模块，如果要处理其他类型的文件，就需要使用loader进行转换
>
> Loader可以理解为是模块和资源的转换器
>
> 首先我们需要安装相关Loader插件
>
> - css-loader是将css装载到javascript
> - style-loader是让javascript认识css



1、安装style-loader和css-loader

```js
npm install --save-dev style-loader css-loader
```



2、修改webpack.config.js

```js
//导入path模块 nodejs内置模块
const path = require("path");
//定义JS打包规则
module.exports = {
    //入口函数从哪里开始进行编译打包
    entry : "./src/main.js",
    //编译成功以后吧内容输出位置
    output:{
        //定义输出的指定目录 __dirname当前项目根目录，产生一个dist文件夹
        path:path.resolve(__dirname,"./dist"),
        //合并的js文件存储在dist/bundle.js文件中
        filename:"bundle.js"
    },
    module:{
        rules:[{
            test:/\.css$/,//把项目中所有的.css结尾的文件进行打包
            use:["style-loader","css-loader"]
        }]
    }
}

```



3、在src文件夹创建style.css

```js
body{
    background: yellow;
}
```

 

4、修改main.js，在第一行引入style.css

```js
//导入utils
const util  =require('./utils');
//导入common
const common  =require('./common');
//导入css
require('./style.css');

common.info("hello world , "+util.add(1,2));
```



5、运行编译命令

```js
webpack
```





# Vue-element-admin

vue-element-admin是一个后台前端解决方案，它基于vue和elemenet-ui实现。它使用了最新的前端技术栈，内置了i18国际化解决方案，动态路由，权限验证，提炼了典型的业务模型，提供了丰富的功能组件，它可以帮助你快速搭建企业级中后台产品原型

官方网站https://gitee.com/julywind/vue-element-admin