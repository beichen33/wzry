# wzry手机端官网和管理后台

## 一、平台环境搭建
### 1.工具安装和环境搭建（nodejs,npm,mongodb,git）
### 2.初始化项目

## 二、管理后台页面
### 1. 增删改查
   (1) 创建Admin、Web、Server端

   (2) 安装插件

   (3) server端自定义脚本中用nodemon运行代码

   (4) 分类创建及编辑页（admin/src/views/CategoryEdit.Vue)

   (5) 列表页（admin/src/views/CategoryList.Vue）

   (6) admin端路由定义（admin/src/router/index.js）

   (7) admin端请求接口放在http.jszhong(admin/src/http.js)

   (8) 在Main.js中引用http.js(admin/src/main.js)

   (9) 后端连接数据库(server/plugins/db,js)

   (10) 创建表模板(server/models/Category.js)

   (11) 写后端接口(server/route/admin/index.js)

   (12) 监听端口（server/index.js）

### 2. 父级分类
   (1) 编辑页面添加选择父类按钮

   (2) 列表页显示父级分类

   (3) server端模型中添加parent字段

   (4) 修改后端接口

### 3. 通用CRUD接口
   (1) 添加mergeParams

   (2) 动态获取接口地址并在请求对象上挂载Model属性

   (3) 后端接口代码如下
   ```
   module.exports = app =>{
    const express = require('express')
    const router = express.Router({
        //合并URL参数，将父级的参数合并到router里面
        mergeParams: true
    })
    // 增
    router.post('/',async(req,res)=>{
        const model = await req.Model.create(req.body)
        res.send(model)
    })
    // 改
    router.put('/:id',async(req,res)=>{
        const model = await req.Model.findByIdAndUpdate(req.params.id,req.body)
        res.send(model)
     })
    //删
    router.delete('/:id',async(req,res)=>{
        const model = await req.Model.findByIdAndDelete(req.params.id,req.body)
        res.send({
            success: true
        })
    })
    // 查，populate关联取出
    router.get('/',async(req,res)=>{
        // const items = await req.Model.find().populate('parent').limit(10)
        const queryOptions = {}
        if(req.Model.modelName === 'Category'){
            queryOptions.populate = 'parent'
        }
        const items = await req.Model.find().setOptions(queryOptions).limit(10)
        res.send(items)
     })
     router.get('/:id',async(req,res)=>{
        const model = await req.Model.findById(req.params.id)
        res.send(model)
     })
    //  动态获取接口地址:resource,中间件处理请求模板
    app.use('/admin/api/rest/:resource',async(req,res,next)=>{
        const modelName = require('inflection').classify(req.params.resource)
        // req.Model是在请求对象上挂载Model属性
        req.Model = require(`../../models/${modelName}`)
        next()
    },router)
   ```
   (4) 修改前端请求接口

### 4. 物品管理
   (1) Main.Vue中添加物品列表

   (2) 复制Category编辑页和列表页为ItemEdit.Vue和ItemList.Vue并修改页面代码

   (3) 添加路由

   (4) 添加模型Item.js

### 5. 图片上传
   (1) 添加上传文件图标(ItemEdit.vue)

   (2) 添加upload文件上传文件夹(server/upload)

   (3) 在server端安装multer中间件处理文件上传

   (4) 写后端接口(server/route/admin/index.js)

   (5) 为了可以访问图片，创建静态文件托管(server/index.js)

### 6. 英雄编辑页
   (1) Main.vue中创建英雄列

   (2) 新建HeroList.vue和HeroEdit.vue

   (3) 路由中引用HeroList.vue和HeroEdit.vue

   (4) 添加英雄模型Hero.js

   (5) 英雄编辑页(HeroEdit.vue)

### 7. 技能编辑页
   (1) 将英雄编辑页的内容用tab包裹

   (2) 添加技能列表

   (3) 技能添加页面

### 8. 文章管理
   (1) Main.vue中添加侧边栏

   (2) 添加ArticleEdit.vue和ArticleList.vue

   (3) router.js中添加路由

   (4) 修改ArticleEdit.vue

   (5) 添加Article.js模型

   (6) 修改ArticleList.vue

   (7) 添加富文本编辑器

### 9. 首页账号管理
### 10. 管理员账号管理
### 11. 登陆页面
### 12. 登录接口
### 13. 服务端登陆校验
### 14. 客户端路由限制
### 15。 上传文件的登陆校验

## 三、移动端网站
### 1.“工具样式”概念和SASS
### 2. 样式重置
### 3. 网站色彩和字体定义(colors,text)
### 4. 通用flex布局样式定义
### 5. 常用边距定义(margin,padding)
### 6. 主页框架和顶部菜单
### 7. 首页顶部轮播图片
### 8. 使用精灵图片
### 9. 使用字体图标
### 10. 卡片组件
### 11. 列表卡片组件
### 12. 首页新闻资讯
   (1) 数据录入

   (2) 数据接口

   (3) 界面展示
### 13.首页英雄列表
   (1) 提取官网数据

   (2) 录入数据

   (3) 界面展示
### 14.新闻详情页
### 15.英雄详情页
   (1) 前端准备

   (2) 后台编辑

   (3) 前端顶部
   
   (4) 前端界面完善
