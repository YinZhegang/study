### 构建项目
+ 安装koa-generator
```bash
 yarn global add koa-generator
```
+ 使用koa-generator生成koa2项目 -e为使用ejes为模版
```bash
 koa2  -e projectname 
```
+ 开始项目
```bash
 cd project 
 yarn install
```
>目录结构

![koa2目录.png](https://upload-images.jianshu.io/upload_images/15430119-4cc0e76e145d1f28.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 重构目录
>当然这样的目录显然不符合我们当下的开发方式，我们要把它变成我们熟悉的MVP模式

+ pulic 目录为资源目录可以不用动
+ bin 目录是重要配置文件也不动
+ 新建app目录并在里面新建模块admin（用于博客后台管理），inde（用于博客正式内容），route（用于管理路由），db.js（用于配置链接数据库参数）
+ 在每个模块都新建controller，model， view 目录这样mvc模型就出来了
+ 删除其他目录及里面的文件
 
![koa2-2.png](https://upload-images.jianshu.io/upload_images/15430119-c2c9607ab980b707.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### 寻找优秀模版
>为了节省开发成本，我们需要去下载一些优秀的模版我用到的的是[amazeUI]([http://tpl.amazeui.org/content.html?7](http://tpl.amazeui.org/content.html?7)
)的模版文件。将静态文件都拷贝到public文件夹，html模版文件改为ejs后缀放入index模块的view中
![WX20190704-202126@2x.png](https://upload-images.jianshu.io/upload_images/15430119-809f6ee4c978615f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
同时需要修改模版文件里面静态资源的路径使之正确引入public的文件

>构建路由，这里为了分离路由和处理逻辑我们将处理逻辑放在controller目录下作为controller层，然后在router里面引入
![WX20190704-222216@2x.png](https://upload-images.jianshu.io/upload_images/15430119-267a1a04fda2929e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

>下面就是controller层文件的写法，koa2采用的是async/await的写法进行异步请求。并引入model。
![WX20190704-222707@2x.png](https://upload-images.jianshu.io/upload_images/15430119-d6019474ba9be2cd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

>现在mvc模式下的V和C已经搭建好了，就差M了，所以首先我们要安装一下Sequelize用于链接数据库，以下命令安装sequelize，点击可以去了解一下[sequelize]([https://www.jianshu.com/p/72a6b80b6873](https://www.jianshu.com/p/72a6b80b6873)
),这里不作过多介绍。
```bash
$ yarn add sequelize
```
>在app文件夹下新建db.js用于配置数据库
```javascript
var Sequelize = require('sequelize')

module.exports = new Sequelize('yzgblog','yzgblog','yzg_blog',{
    host:'你的数据库地址',
    dialect:'mysql',
    pool: {
        max: 5,
        min: 0,
        idle: 10000
    }
})
```
![WX20190704-223420@2x.png](https://upload-images.jianshu.io/upload_images/15430119-4eafb5f4125bd504.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
>在model文件夹下新建模型层如users（用户），blogs（博客表）等等。引入db.js配置
```javascript
let Sequelize = require('sequelize')
let sequelize = require('../../db')
let User = sequelize.define('users',{
    nickname:{
        type:Sequelize.STRING
    },
    avater:{
        type:Sequelize.STRING
    },
    email: {
        type: Sequelize.STRING
    }
})
module.exports = User;
```
>至此项目的大致框架，mvc分层就都搭建起来了
![WX20190704-231054@2x.png](https://upload-images.jianshu.io/upload_images/15430119-704b3d94706fb3ca.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![WX20190704-231109@2x.png](https://upload-images.jianshu.io/upload_images/15430119-9a1323da0f2749b8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



