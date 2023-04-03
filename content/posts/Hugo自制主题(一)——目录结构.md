---
title: "Hugo自制主题(一)——目录结构"
date: 2023-02-04T12:29:43+08:00
draft: true
---

学习路线：
1.认识网站目录结构，初步了解作用
2.学习content下内容如何使用模板layout下文件生成网站页面
3.编写主题下layout文件


# 目录结构

## 目录

| 目录        | 作用                                       |     |
| ----------- | ------------------------------------------ | --- |
| archetypes  | 新建文章时的默认模版                       |     |
| assets忽略  |                                            |     |
| content重要 | 生成网站的原始材料                         |     |
| data        |                                            |     |
| layouts     |                                            |     |
| public忽略  | 生成渲染后可部署的网站资源                 |     |
| resources   |                                            |     |
| static      | 静态资源存放位置如图片，样式文件，脚本文件 |     |
| i18n        | 存放国际话化js文件                         |     |
| themes      | 网站的主题ui                               |     |
| config.toml | 网站的配置文件                             |     |



# 博客网页生成规则
**页面= content下内容+layouts模板
hugo按照一定规则找寻content对应的模板，从而生成页面
下面看content下的内容和layout下的模板类型，在了解对应匹配的规则
**
## Content目录
``` javascript
└── content
    ├── _index.md          // [home]            <- https://example.com/ **
    ├── about.md           // [page]            <- https://example.com/about/
    ├── posts               
    |   ├── _index.md      // [section]         <- https://example.com/posts/ **         
    |   ├── firstpost.md   // [page]            <- https://example.com/posts/firstpost/
    |   ├── happy           
    |   |   ├── _index.md  // [section]         <- https://example.com/posts/happy/ **
    |   |   └── ness.md    // [page]            <- https://example.com/posts/happy/ness/
    |   └── secondpost.md  // [page]            <- https://example.com/posts/secondpost/
    └── quote   
        ├── _index.md      // [section]         <- https://example.com/quote/ **           
        ├── first.md       // [page]            <- https://example.com/quote/first/
        └── second.md      // [page]            <- https://example.com/quote/second/

// hugo默认生成的页面, 没有对应的markdown文章
分类列表页面               // [taxonomyTerm]    <- https://example.com/categories/  **
某个分类下的所有文章的列表  // [taxonomy]        <- https://example.com/categories/one-category  **
标签列表页面               // [taxonomyTerm]    <- https://example.com/tags/  **
某个标签下的所有文章的列表  // [taxonomy]        <- https://example.com/tags/one-tag  **
```
[]内为文件的属性Kind属性，分为两类用于模板的匹配规则
- single(单页面 - page) 
- list(列表页 - home, section, taxonomyTerm, taxonomy)。
| 列表页 | 对应url | 匹配模板 |
| --------- | ---- | --- |
| home| / |    
| section | /posts/  |
| taxonomyTerm |  /categories/  |layouts/categories/term.html|
| taxonomy |  /categories/技术/  |
## 主题layouts目录
``` txt
├── layouts
└── themes
    └── mytheme
        └── layouts
            ├── 404.html             // 404页面模板
            ├── _default
            │   ├── baseof.html      // 默认的基础模板页, 使用的方式是'拼接', 而不是'继承'.
            │   ├── list.html        // 列表模板  
            │   └── single.html      // 单页模板
            ├── index.html           // 首页模板
            └── partials             // 局部模板, 通过partial引入
                ├── footer.html
                ├── header.html
                └── head.html  
```
layouts模板   优先使用layouts(一般没有内容)，次使用themes/mythem/layouts

## 对应生成规则

### content页面属性
 -  **kind**: 用于确定页面的类型, 单页面使用single.html为默认模板页, 列表页使用list.html为默认模板页, 值不能被修改
 - **section**: 用于确定section tree下面的文章的模板. section tree的结构是由content目录结构生成的, 不能被修改, content目录下的一级目录自动成为root section, 二级及以下的目录, 需要在目录下添加_index.md文件才能成为section tree的一部分. 如果页面不在section tree下section的值为空
 - **type**: 可以在Front Matter中设置, 用户指定模板的类型. 如果没设定type的值, type的值等于section的值 或 等于page(section为空的时候)
 - **layout**: 可以在Front Matter中设置, 用户指定具体的模板名称.

### layout模板内容
**特定页面模板:** index首页模板
**对应某一类页面模板:**
**全站模板:** default目录下的list列表、single单页
### 匹配优先级
按照content下属性匹配
 - 特定页面的模板
 - 应对某一类页面的模板
 - 应对全站的模板: 存放在_default目录下面的list.html 和 single.html页面



## themes主题目录

| 主题文件目录           | 作用                                                |     |
| ---------------------- | --------------------------------------------------- | --- |
| archetypes             | 新建文章时的默认模版                                |     |
| assets忽略             |                                                     |     |
| data                   |                                                     |     |
| exampleSite忽略        | 示例站点文件替换网站根目录，便于测试发布            |     |
| i18n                   | 存放国际话化js文件                                  |     |
| images                 | 主题图片资源                                        |     |
| layouts重要            | 布局样式                                            |     |
| static                 | 静态资源存放位置如图片imgs，样式文件css，脚本文件js |     |
| themes                 | 网站的主题ui                                        |     |
| theme.toml名字可能任意 | 网站的配置文件                                      |     |
