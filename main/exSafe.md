<!-- 安全测试 -->

?> 测试工具：BurpSuit、Fiddler

> ### 1. 敏感信息

```
敏感信息：客户姓名、手机号码、证件号、卡号、地址

1、 日志不允许直接打印，需要加密：用户、密码、验签 token 等
2、 页面源码（含注释），避免暴露服务器文件路径、账户密码
3、 避免前端做了加密，但后端返参还是未加密
4、 加密原则 *
姓名，周**
证件，加密后6位
卡号，除了前6后4，中间加密
手机号，除了前3后4，中间加密
```


> ### 2. SQL注入

```
1、 输入框输入单引号'，查看返回是否含SQL报错
2、 输入 ' or 1=1，验证后端代码的拼接是否使用占位符 ?
3、 前端输入 SQL，抓包查看 SQL 标签是否转义
```


> ### 3. 跨站XSS

```
1、 输入 script，如 alert 弹窗、javascript 跳转到其他网页
2、 输入 HTML 标签，如 IMG、IFRANME 正常展示，则后端没有过滤
```

> ### 4. 防重放

```
1、 短信轰炸，短信发送返回需要有时间间隔
2、 前端发送倒计时中，刷新页面重发，注意后端返回的时间间隔
```

> ### 5. 防撞库

```
1、 用户登录，不允许频繁尝试
2、 需要设置错误次数、重试等待时间
3、 加入验证码计时机制
```

> ### 6. 越权访问

```
1、 水平越权，权限相同，能访问他人的仅个人可见页面
2、 垂直越权，权限不同，能访问到不属于当前权限的页面
```

> ### 7. 上传下载

```
1、 限制上传格式、大小，使用 burpsuit 篡改上送文件内容（超过最大值）
2、 非法格式的文件，修改成合法后缀，上传查看后端是否做有效的检测
3、 上传文件名不含.. /xs & ，编码不含 00
4、 下载文件，请求服务器的其他路径，查看是否做权限拦截
```

> ### 8. 外部XML注入

```
XXE 待实践
```