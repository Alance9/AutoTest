<!-- 导航栏 -->

> ### 1. 导航栏设置

```index.html

window.$docsify = {
  // 加载顶部导航栏
  loadNavbar: true
}
```

> 右上角导航栏，作为单个链接展示

![导航栏1](../_media/_resources/导航栏1.png)

?> 导航栏默认文件名 _navbar.md

```docsifyBlog/_navbar.md

* [:uk: English](/docsify_en/README.md "English")

* [:cn: 简体中文](/README.md "Chinese") 
```


> 作为下拉框选项链接展示

![导航栏2](../_media/_resources/导航栏2.png)

```docsifyBlog/_navbar.md 
* 主题切换
  * [:sunny: 黑夜](zh-cn/quickstart.md)
  * [:crescent_moon: 白天](zh-cn/quickstart.md)

* Language
  * [:uk: English](/docsify_en/ "English")
  * [:cn: 简体中文](/ "Chinese")
  ```

> ### 2. 多个导航栏

存在多个导航栏时，默认取主导航栏

存在子导航栏，则取子导航栏，不存在则取主导航栏

?> 实现语言切换，具体查看 [本地化](/docsify/localize)

```docsifyBlog/_navbar.md
<!-- 主导航栏 -->

* Language
  * [:uk: English](/docsify_en/README.md "English")
  * [:cn: 简体中文](/README.md "Chinese")

```

```docsifyBlog/docsify_en/_navbar.md
<!-- 子导航栏 -->

* Language
  * [:uk: English](/docsify_en/README.md "English")
  * [:cn: 简体中文](/README.md "Chinese")
```