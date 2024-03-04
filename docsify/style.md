<!-- 自定义设置 -->

> ###  1. 自定义封面

`_coverpage.md` 定义首页内容，可替换自定义封面，默认为随机渐变

```_coverpage.md 
<!-- 首页 -->

# Alance <small>share 1.0</small>

> 分享编程与测试的经验

- HTML. CSS. JS
- PYTHON. JAVA. RUBY
- TEST FUNCTION. AUTO. API et

[Start Browse →](README.md)

<!-- 替换封面 -->
![bg](_media/云朵与羊群-新1.png)

```

> 封面透明度设置

```style.css
section.cover.has-mask .mask{
    opacity:.1;
}
```

> ###  2. 自定义主题色

`themeColor` 默认主题色为 `#42b983`

```index.html
window.$docsify = {
    // 主题色
    themeColor: "#96b97d"
}
```

> ###  3. 其他设置

```index.html
window.$docsify = {
    // 目录标题
    name: 'Docsify',

    // 自定义目录级别
    subMaxLevel: 3,
    
    // 加载侧边栏
    loadSidebar: true,

    // 加载顶部导航栏
    loadNavbar: true,

    // 开启渲染封面
    coverpage: true,
}
```

```style.css
<!-- 自定义样式 -->

nav {
    font: 16px bold;
}

nav.app-nav li {
    min-width: 30px;
    margin-right: 20px;
}

nav.app-nav li ul {
    min-width: 100px;
}

nav.app-nav a {
    color: #34495e;
}

nav.app-nav a.active {
    color: #34495e;
    border-bottom: none;
}

aside {
    margin-left: 30px;
}

aside .sidebar-nav ul li p{
    color: #96b97d;
}

aside .sidebar-nav ul li a {
    padding: 0;
}

section.cover.has-mask .mask{
    opacity:.1;
}
```
