<!-- 主文件 -->

***启动！编写文档***

> ### 1. 编辑 MD 文档

熟悉语法，可以直接在编辑器上编写内容，热重载支持调试

> 不熟悉语法，推荐使用 Joplin （开源软件）

![MD语法1](../_media/_resources/MD语法1.png ':size=80%')

> Joplin 支持导出 MD 文档

![MD语法2](../_media/_resources/MD语法2.png ':size=50%')

> 导出 MD 资源，将文件和图片拷贝到项目中，修改文件名、图片路径即可

![MD语法3](../_media/_resources/MD语法3.png ':size=70%')

![MD语法4](../_media/_resources/MD语法4.png ':size=70%')


> ### 2. MD 语法

Docsify 支持 MD 语法，具体参考：

基本语法：https://markdown.com.cn/basic-syntax/i

扩展语法：https://markdown.com.cn/extended-syntax/

```
MD 支持内嵌 HTML 标签，\ 转义
<u>下划线</u>

1-6 # 标题从大到小
### 标题3

斜体
*123*

加粗
**123**

斜体加粗
***123***

> 块
>> 嵌套块

1. 有序列表
- 无序列表
	- 嵌套无序列表
Tab 保持其他内容与列表齐列（四个空格）

- [ ] 任务块
- [x] 已勾选

引号围栏
`单词`
``句子`
围栏代码块 + 语法提示 （Tab 转义）
	```python
		print('123')
	```

---  分隔线，上下空行
***	 分隔线，上下空行

删除线
~~123~~

超链接
[超链接](网址 "title")	
<网址>
<邮箱>
可搭配斜体、加粗、反引号使用

图片
![提示](路径 "title")  

图片超链接
[![提示](/路径 "title")](网址)	

表格
:- 左对齐
-: 右对齐
:-: 居中
| Syntax | Description |
| - | - |
| Header | Title |
| Paragraph | Text |

表情符号 https://gist.github.com/rxaviers/7360908
笑哭 :joy:

```


> ### 3. Docsify 补充语法

```
强调内容
!> 123

普通提示
?>

链接的 target 属性
[提示](网址 ':target=_blank')
[提示](网址 ':target=_self')

图片缩放
![提示](路径 ':size=WIDTHxHEIGHT') 
![提示](路径 ':size=50x100')
![提示](路径 ':size=100')
![提示](路径 ':size=10%')

设置图片 id、class
':id=id'
':class=class'

锚点
- [test](#test)
<span id="test">123</span>
```
