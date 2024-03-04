<!-- 搜索栏 -->

> ### 引用 search 插件

```index.html
<script>
    window.$docsify = {
        // 自定义搜索
      search: {
        maxAge: 86400000, // 过期时间，单位毫秒，默认一天
        placeholder: 'Type to search',

        // 支持本地化
        placeholder: {
          '/docsify_en/': 'Type to search',
          '/': '搜索'
        },

        noData: 'No Results!',

        // 支持本地化
        noData: {
          '/docsify_en/': 'No Results',
          '/': '找不到结果'
        },

        // 搜索标题的最大层级, 1 - 6
        depth: 6,
      }
 </script>

<!-- 搜索 -->
<script src="//cdn.jsdelivr.net/npm/docsify/lib/plugins/search.min.js"></script>
```
![搜索1](../_media/_resources/搜索1.png ':size=70%')

![搜索2](../_media/_resources/搜索2.png ':size=70%')