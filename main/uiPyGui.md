<!-- pyautogui -->

> ### 1. 应用场景

?> PyAutoGui 可辅助 selenium 实现 Web-UI 自动化，也可以独立构建 PC-App 自动化。 <br>
缺点是，图片匹配作为断言，耗时较久，主要写基本功能的冒烟案例，减少案例数，通过性验证。 <br>
<br>
断言方法：<br>
1、 识别图片文字，参考：[图片处理](main/pyPic.md) <br>
2、 对比截图相似度 <br>
需要先保存预期图片，根据坐标定位模糊匹配，在精度阈值内认为是成功的 <br>
3、 通过鼠标操作，在固定位置复制文本断言


> ### 2. 鼠标交互

``` python
# pip install pyautogui

import pyautogui

# 获取当前屏幕尺寸、鼠标位置、鼠标是否在范围内
print(pyautogui.size(), pyautogui.position(), pyautogui.onScreen(1800, 1800))

# Size(width=3840, height=2160) Point(x=2292, y=550) True

""" 鼠标操作 """

# 坐标、点击次数、间隔、键位、过渡时间

# moveTo moveRel
# scroll vscroll
# dragTo dragRel
# button: left right middler
# click rightClick middleClick doubleClick tripleClick
# mouseDown mouseUp

# duration 过渡时间（s），即动作快慢
x, y = pyautogui.size()

# 移动
# 绝对定位
pyautogui.moveTo(x/2, y/2, duration=1)

# 相对定位
pyautogui.moveRel(180, 180, duration=2)


# 滚动条
# 垂直滚动
# + 向上、- 向下
pyautogui.scroll(1200)

# 水平滚动
# + 向右、- 向左
pyautogui.vscroll(-200)


# 拖拽
# mouseDown + moverTo + mouseUp
# 绝对定位
pyautogui.dragTo(600, 800,button='left',duration=1)

# 相对定位
pyautogui.dragRel(800, 800,button='left',duration=1)


# 单击
pyautogui.click(button='middle')
pyautogui.rightClick()
pyautogui.middleClick()

# # 多次点击
pyautogui.click(1000, 1000, clicks=3, interval=1, button='right', duration=1)

# 双击
pyautogui.doubleClick(1000, 1000, interval=1,
                      button='right', duration=1)

# 三击
pyautogui.tripleClick()

# 分解动作
pyautogui.mouseDown(1300, 100, button='left', duration=1)
pyautogui.mouseUp(1300, 300, button='left', duration=1)
```


> ### 3. 键盘交互

``` python
import pyautogui


""" 键盘操作 """

# 单键
pyautogui.press('capslockABC')

# 多键
pyautogui.press(['a', 'b', 'c'], interval=0.5)

# 组合键
pyautogui.hotkey('ctrl', 'shift', 'a')

# 文本
pyautogui.write('hhh', interval=0.5)
pyautogui.typewrite('Alance')

# 分解动作
pyautogui.keyDown('shift')
pyautogui.press('ctrl')
pyautogui.keyUp('shift')


# KEYBOARD_KEYS

['\t', '\n', '\r', ' ', '!', '"', '#', '$', '%', '&', "'", '(',
 ')', '*', '+', ',', '-', '.', '/', '0', '1', '2', '3', '4', '5', '6', '7',
 '8', '9', ':', ';', '<', '=', '>', '?', '@', '[', '\\', ']', '^', '_', '`',
 'a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm', 'n', 'o',
 'p', 'q', 'r', 's', 't', 'u', 'v', 'w', 'x', 'y', 'z', '{', '|', '}', '~',
 'accept', 'add', 'alt', 'altleft', 'altright', 'apps', 'backspace',
 'browserback', 'browserfavorites', 'browserforward', 'browserhome',
 'browserrefresh', 'browsersearch', 'browserstop', 'capslock', 'clear',
 'convert', 'ctrl', 'ctrlleft', 'ctrlright', 'decimal', 'del', 'delete',
 'divide', 'down', 'end', 'enter', 'esc', 'escape', 'execute', 'f1', 'f10',
 'f11', 'f12', 'f13', 'f14', 'f15', 'f16', 'f17', 'f18', 'f19', 'f2', 'f20',
 'f21', 'f22', 'f23', 'f24', 'f3', 'f4', 'f5', 'f6', 'f7', 'f8', 'f9',
 'final', 'fn', 'hanguel', 'hangul', 'hanja', 'help', 'home', 'insert', 'junja',
 'kana', 'kanji', 'launchapp1', 'launchapp2', 'launchmail',
 'launchmediaselect', 'left', 'modechange', 'multiply', 'nexttrack',
 'nonconvert', 'num0', 'num1', 'num2', 'num3', 'num4', 'num5', 'num6',
 'num7', 'num8', 'num9', 'numlock', 'pagedown', 'pageup', 'pause', 'pgdn',
 'pgup', 'playpause', 'prevtrack', 'print', 'printscreen', 'prntscrn',
 'prtsc', 'prtscr', 'return', 'right', 'scrolllock', 'select', 'separator',
 'shift', 'shiftleft', 'shiftright', 'sleep', 'space', 'stop', 'subtract', 'tab',
 'up', 'volumedown', 'volumemute', 'volumeup', 'win', 'winleft', 'winright', 'yen',
 'command', 'option', 'optionleft', 'optionright']
```


> ### 4. 弹窗交互

``` python
import pyautogui


""" 弹窗 """

# 暂时用不上

# 普通弹窗
pyautogui.alert(text='Warning', title='普通弹窗', button='确定')

# 确认框
pyautogui.confirm(text='Warning', title='确认框', buttons=['确定', '取消'])

# 输入框
pyautogui.prompt(text='Warning', title='输入框', default='正在输入中...')

# 密码框
pyautogui.password(text='Warning', title='密码框', default='123', mask='*')
```

> ### 5. 图片匹配

?> GUI 自动化的关键所在，根据准备的图片匹配，一步步执行案例步骤，最后断言，匹配上预期图片即成功。 <br>
<br>
示例参考：识别 vscode 展开、折叠文件按钮

> #### 折叠文件 
fileClick.png

![fileClick](../_media/resource/fileClick.png)
![fileClick1](../_media/resource/fileClick1.png ':size=28%')

> #### 展开文件 
fileClickAgain.png

![fileClickAgain](../_media/resource/fileClickAgain.png)
![fileClickAgain1](../_media/resource/fileClickAgain1.png ':size=28%')

``` python
import pyautogui


""" 截图 """

# 全屏截图
pyautogui.screenshot('./pygui/abc.png')

# 局部截图
# 左上角左边、宽度、长度
pyautogui.screenshot('./pygui/123.png', region=(200,800, 500, 500))


# 图片匹配
# 根据中心坐标，像素匹配
# 左上角左边、宽度、长度

x, y = pyautogui.size()

# 折叠文件
position = pyautogui.locateCenterOnScreen(
    './pygui/fileClick.png', region=(0, 0, int(x/2), int(y/2)))
pyautogui.moveTo(*position, duration=0.5)
pyautogui.click()

# 展开文件
position = pyautogui.locateCenterOnScreen(
    './pygui/fileClickAgain.png', region=(0, 0, int(x/2), int(y/2)))
pyautogui.moveTo(*position, duration=0.5)
pyautogui.click()


""" GUI 自动化，检测图片 """


def 识别图片(path, times=3):
    # 等待识别图片，判断图片是否存在
    if os.path.exists(path):
        logger.info('检测图片中...')
        x, y = pyautogui.size()
        # grayscale 灰度化
        # 设置精度阈值(0-1)，精度越高，匹配度越高
        flag = 0 
        while not flag:
            try:
                pyautogui.locateCenterOnScreen(path, region=(
                    0, 0, int(x/2), int(y/2)), grayscale=True, confidence=0.9)
                logger.info('检测到图片，进入下一步')
                return True
            except:
                pass
            # 检测 times 次，匹配不到返回 false
            if not times:
                logger.info(f'图片识别失败：{path}')
                return False
            times -= 1
    else:
        logger.info('找不到预期图片')
        return False


""" 案例断言 """


def test_pic_suc():
    result = 识别图片('./pygui/fileClick.png')
    assert result


def test_pic_fail():
    result = 识别图片('./pygui/fileClickAgain.png')
    assert result
```
```
test_pygui.py::test_pic_suc 2024-03-05 22:09:43,492 test_pygui.py 识别图片 [line- 131] INFO 检测图片中...
2024-03-05 22:09:43,653 test_pygui.py 识别图片 [line- 140] INFO 检测到图片，进入下一步
PASSED

test_pygui.py::test_pic_fail 2024-03-05 22:09:43,655 test_pygui.py 识别图片 [line- 131] INFO 检测图片中...
2024-03-05 22:09:44,330 test_pygui.py 识别图片 [line- 146] INFO 图片识别失败：./pygui/fileClickAgain.png
FAILED

====================================================================================== FAILURES ======================================================================================= 
____________________________________________________________________________________ test_pic_fail ____________________________________________________________________________________ 

    def test_pic_fail():
        result = 识别图片('./pygui/fileClickAgain.png')
>       assert result
E       assert False

test_pygui.py:161: AssertionError
```