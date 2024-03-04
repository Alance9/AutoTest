<!-- 侧边栏 -->

***编辑好的文档，需要入口访问，即侧边栏菜单 _sidebar***

> ### 1. 侧边栏设置

```index.html
window.$docsify = {
    // 自定义目录级别
    subMaxLevel: 3,
    // 加载侧边栏
    loadSidebar: true,
}
```

```MD
<!-- subMaxLevel 设置为3，侧边栏可自动获取 1-3 级的副标题 -->

# Title 1
## Title 2
### Title 3

<!-- 忽略特定副标题 -->
# Title 1 <!-- {docsify-ignore} -->
<!-- 忽略所有副标题 -->
# Title 1 <!-- {docsify-ignore-all} -->
```

?> 侧边栏默认文件名 _sidebar.md

```docsifyBlog/_sidebar.md 
* Dosify 入门

    * [入门](docsify/install)
```

> ### 2. 多个侧边栏

存在多个侧边栏时，默认取主侧边栏

存在子侧边栏，则取子侧边栏，不存在则取主侧边栏

?> 切换到子目录时，自动获取该目录下的子侧边栏 _sidebar，从而实现本地化，具体查看 [本地化](/docsify/localize)

```docsifyBlog/_sidebar.md
<!-- 主侧边栏 -->

* 🌻 快速搭建一个博客系统

* Dosify 入门

    * [入门](docsify/install)
    * [项目架构](docsify/structure)
    * [主文件](docsify/pages)
    * [侧边栏](docsify/side)
    * [导航栏](docsify/nav)
    * [搜索框](docsify/search)
    * [本地化](docsify/localize)
    * [远程部署](docsify/deploy)
    * [常用插件](docsify/plugins)
    * [表情符号](docsify/emo)
    * [自定义设置](docsify/style)

```

```docsifyBlog/docsify_en/_sidebar.md
<!-- 子侧边栏 -->

* 🌻 Quickly build a blog system

* Getting started with Dosify

    * [Getting started](docsify_en_en/install)
    * [Project structure](docsify_en/structure)
    * [Home Page Document](docsify_en_en/style)
    * [Sidebar](docsify_en/side)
    * [Navigation Bar](docsify_en/nav)
    * [Search](docsify_en/search)
    * [Localization](docsify_en/localize)
    * [Remote Deploy](docsify_en/deploy)
    * [Common plug-ins](docsify_en/plugins)
    * [emoji](docsify_en/emo)
    * [Custom setting](docsify_en/style)
```