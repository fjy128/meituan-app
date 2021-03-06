# 项目介绍

- 前端---美团---服务端
- Vue CLI3 -> nuxt.js -> Vue2.5 -> vuex(前端技术)
- SSR -> vuex(美团)
- koa2 redis-> mongoose mongodb(美团)
- 使用 ssr+koa+mongodb 以服务端渲染的方式开发美团外卖项目
- 基础制知识：Vue全家桶基础 koa2核心知识 Nuxt.js
- 实战重点：登录、注册 、 SMTP服务、城市服务、推荐服务、搜索服务、地图服务、购物车、订单设计、组件复用设计、接口设计、数据对象模型、思维与技巧
- vuex 出现解决各组件间互不影响之间的传值

## Build Setup

``` bash
# install dependencies
$ npm run install

# serve with hot reload at localhost:3000
$ npm run dev

# build for production and launch server
$ npm run build
$ npm run start

# generate static project
$ npm run generate
```

For detailed explanation on how things work, check out [Nuxt.js docs英文地址](https://nuxtjs.org). [Nuxt.js中文地址](https://zh.nuxtjs.org/)

# 项目安装(通过脚手架安装)
```bash
 1、npm install -g npx
 2、npx create-nuxt-app [project-name]
```
- nuxt项目搭建：[github项目搭建地址](https://github.com/nuxt-community/koa-template) 或者官网
   - Nuxt.js目录
    - 1）README.md --辅助文件
    - 2）assets --静态资源文件css、img
    - 3)backpack.config.js --拓展编译脚本，不要随便懂
    - 4)build --编译后的文件
    - 5)components --放置vue组件的地方
    - 6)layouts --模版
    - 7)node_modules --安装包
    - 8)nuxt.config.js --NUXT的配置文件
    - 9)package-lock.json
    - 10)package.json --依赖文件
    - 11)pages --页面路口（vue文件）
    - 12)server --放koa相关文件
    - 13)static --静态文件
    - 14)store --vuex(只要有这个目录就是已经启用vuex了)
  
# 需求分析（接口设计原则）

1、用户注册登录问题

/users/signup  //注册

/users/signin  //登陆

/users/verify  //验证码

/users/exit    //推出

/users/getUser //用户登陆信息


2、城市热门服务类接口设计

/geo/getPosition

/geo/province

/geo/province/:id

/geo/city            // 获取所有城市

/geo/hostCity       // 获取热门城市

/geo/menu          // 左边表单类表数据

3、查询类服务接口设计

/search/top                  // 输入搜索关键字获取数据 params 关键字(input)、当前城市(city)、 签名(sign)

/search/resultsByKeywords

/search/hotPlace            // 获取热门搜索

/search/products

/search/product/:id

# 前期准备工作
一、redis准备工作： 

1、[安装redis地址](https://www.runoob.com/redis/redis-install.html)
```bash
1）mac版安装
安装 redis : brew install redis(mac) 
启动 redis : redis-server

2）window版安装
（1）[下载redis](https://www.runoob.com/redis/redis-install.html) 
（2）解压redis 安装包
（3）启动服务器  Win+R快捷键，输入CMD，进入CMD窗口，进入解压后文件所在路径，并输入以下指令：redis-server.exe redis.windows.conf 
  - D:\Redis-x64-3.0.504(环球雅途工作盘) 进入d盘 d:
  完成上面步骤就完成了window系统安装redis并启动
  window 进入 redis 解压包 执行redis-server (使用cmd)
  启动 redis-server
```
2、[redis官方文档](https://www.php.cn/manual/view/16111.html)

二、mongodb准备工作

```bash
1、安装 mongodb
2、启动 mongod --dbpath /Users/a/Desktop/meituan-app/data
(备注：‘/Users/a/Desktop/meituan-app/data’是你数据库存放地址，data就是数据库文件)
```
[mongodb官方文档](https://docs.mongodb.com/manual/reference/method/db.collection.find/#db.collection.find)

三、数据库导入数据

```bash
1）首先在RoTo 3T中导入数据
2）执行命令：mongoimport -d dbs -c test pois.dat (数据表在哪就在哪文件下执行)
其中 dbs 你的数据库名称，test 数据库的数据表名， pois是集合名， pois.dat即将导入数据库的数据源。
3）项目中使用到的数据表都放在根目录下的dbs中
```
四、项目中接口签名

```bash
axios.get(`http://cp-tools.cn/geo/getPosition?sign=${sign}`)
${sign}:签名  
获取签名地址：http://cp-tools.cn/sign
```

# passport  http://ju.outofmemory.cn/entry/99459
 
# 搭建项目遇到问题及解决方法

1、当启动项目的时，报【'babel-node' 不是内部或外部命令，也不是可运行的程序】错误；

```bash
 解决方法：执行 cnpm i babel-core babel-preset-es2015 babel-cli 可以修复
```
 2、启动项目中出现如下报错，需要安装一下,并创建文件 .babelrc

 ```bash
import Koa from‘koa’
SyntaxError: Unexpected token import

语法错误：此处不应该出现import

解决方法：

1）执行：cnpm install --save-dev babel-preset-env babel-cli
2）新建一个.babelrc文件，并在里面写入：
  {
    "presets":["es2015"]
  }
```

3、项目运行成功时，报【 No ESLint configuration found】错误解决办法

```bash
  解决方法：

1）执行：cnpm install eslint --save-dev 
2）创建eslint配置文件： .eslintrc.js
```

4、nuxt的asyncData数据不会根据页面路由参数改变重新渲染数据[参考地址](https://blog.csdn.net/zah521999/article/details/89944991)

```bash
1）问题分析：由于asyncData方法是在组件初始化 前被调用的，并且是在服务端调用，也就意味着只能在首次重新加载的时候调用，如果遇到了分页，当页码改变的时候不能做服务端的调用，路由参数改变也是如此。
2）解决方案：nuxt的官方文档中提供了watchQuery属性可以监听参数字符串的更改。将调用所有组件方法(asyncData, fetch, validate, layout, …)。 为了提高性能，默认情况下禁用
3）watchQuery的使用方法：（watchQuery其实监听的是路径上参数的变化，从而调用asyncData方法）
（1）首先如果要全局使用watchQuery，那就在nuxt.config.js中全局配置
      module.exports = {
          watchQuery: true
      }
（2）如果不在nuxt.config.js中全局配置，那就默认是只在当前页面的监听
      export default {
        //此处的page是当前路径上参数的key名
          watchQuery: ['page'] 
      }
```
# 需要优化问题
1、新用户注册模块，邮箱验证码后天接口需要区分大小写，后期要修正为不需要区分大小写

2、登陆使用127.0.0.1:3000，不会获取用户的信息，所以获取不到用户的登陆信息，用localhost:3000就可以

3、接口开发操作本地数据库接口完善，需完善获取热门城市、搜索、热门搜索

4、接口实现本地数据库连接

5、页面操作弹框提示

6、用户登录失败或者密码账号有错做相应的弹框提示

# 相关技术文档

- [vue router官方文档](https://router.vuejs.org/zh/guide/)
- [vuex官方文档](https://vuex.vuejs.org/zh/)


# 特殊命令
```bash
curl -d 'name=lilei&age=27' http://localhost:3000/addPreson(向数据库写入信息，-d表示post)
```

# 高德地图 

[开发者API](https://lbs.amap.com/api/javascript-api/guide/abc/prepare)

使用步骤：一、生成地图控件；
1. 首先，注册开发者账号，成为高德开放平台开发者

2. 登陆之后，在进入「应用管理」 页面「创建新应用」

3. 为应用添加 Key，「服务平台」一项请选择「 Web 端 ( JSAPI ) 」