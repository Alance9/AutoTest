<!-- Pyppeteer -->


> ### 1. 应用场景

?> Pyppeteer 常用于抓取页面数据，是 Puppeteer 的非官方版本，与 selenium 类似，支持操作浏览器页面元素。 <br>
<br>
selenium 需要下载配置驱动，pyppeteer 首次运行会自动下载 chromnium <br>
pyppeteer 默认下载链接需要重新配置，否则国内连接失败


> ### 2. 首次运行


``` python
# pip install pyppeteer
# https://npm.taobao.org/mirrors/chromium-browser-snapshots/

import asyncio
from pyppeteer import launch

base_url = r"https://alance9.gitee.io/auto-test/#/main/pyPic"

async def main(url=base_url):

    driver = await launch()
    page = await driver.newPage()

    # 访问网站
    await page.goto(url)
    await driver.close()

if __name__ == '__main__':
    asyncio.get_event_loop().run_until_complete(main())
```
```
PS E:\Code\pyMore> & D:/python/python.exe e:/Code/pyMore/pdf.py
[INFO] Starting Chromium download.
...
ConnectionResetError: [WinError 10054] 远程主机强迫关闭了一个现有的连接。

During handling of the above exception, another exception occurred:
...
urllib3.exceptions.MaxRetryError: HTTPSConnectionPool(host='storage.googleapis.com', port=443): Max retries exceeded with url: /chromium-browser-snapshots/Win_x64/1181205/chrome-win.zip (Caused by ProtocolError('Connection aborted.', ConnectionResetError(10054, '远程主机强迫关闭了一个现有的连接。', None, 10054, None)))
```

!> 下载链接报错 'win64': f'https://storage.googleapis.com/chromium-browser-snapshots/Win_x64/1181205/chrome-win.zip' <br>
改成国内镜像 'win64': f'http://cdn.npm.taobao.org/dist/chromium-browser-snapshots/Win_x64/1181205/chrome-win.zip'


> #### 查看浏览器对应的 Chromium 版本、下载路径、下载链接

``` python
import pyppeteer.chromium_downloader
print('Version: {}'.format(pyppeteer.__chromium_revision__))
print('Path: {}'.format(pyppeteer.chromium_downloader.chromiumExecutable.get('win64')))
print('Download: {}'.format(pyppeteer.chromium_downloader.downloadURLs.get('win64')))
```
```
Version: 1181205
Path: C:\Users\Z\AppData\Local\pyppeteer\pyppeteer\local-chromium\1181205\chrome-win\chrome.exe
Download: https://storage.googleapis.com/chromium-browser-snapshots/Win_x64/1181205/chrome-win.zip
```


> #### 修改下载链接，重新运行

!> pyppeteer 安装路径下，文件 chromium_downloader.py ，修改对应系统的下载链接，再次运行


``` python
# D:\python\Lib\site-packages\pyppeteer
# chromium_downloader.py

downloadURLs = {
    'linux': f'{BASE_URL}/Linux_x64/{REVISION}/chrome-linux.zip',
    'mac': f'{BASE_URL}/Mac/{REVISION}/chrome-mac.zip',
    'win32': f'{BASE_URL}/Win/{REVISION}/{windowsArchive}.zip',
    'win64': f'http://cdn.npm.taobao.org/dist/chromium-browser-snapshots/Win_x64/1181205/chrome-win.zip',
}
```
``` python
[INFO] Starting Chromium download.
Traceback (most recent call last):
...
OSError: Chromium downloadable not found at http://cdn.npm.taobao.org/dist/chromium-browser-snapshots/Win_x64/1181205/chrome-win.zip: Received <?xml version="1.0" encoding="UTF-8"?>
<Error>
  <Code>NoSuchKey</Code>
  <Message>The specified key does not exist.</Message>
  <RequestId>65E86E44D76E913234A3FACA</RequestId>
  <HostId>npmmirror.oss-cn-shanghai.aliyuncs.com</HostId>
  <Key>binaries/chromium-browser-snapshots/Win_x64/1181205/chrome-win.zip</Key>
  <EC>0026-00000001</EC>
  <RecommendDoc>https://api.aliyun.com/troubleshoot?q=0026-00000001</RecommendDoc>
</Error>
```

!> 再次运行报错，找不到 1181205 版本号

> #### 修改版本号，重新运行

?> pyppeteer 安装路径下，文件 \_\_init\_\_.py，改成国内镜像存在的版本号，如 1267556 <br>
pyppeteer 安装路径下，文件 chromium_downloader.py，下载链接也改成相同版本号 <br>
<br>
获取存在的版本号：https://npm.taobao.org/mirrors/chromium-browser-snapshots/

``` python
# D:\python\Lib\site-packages\pyppeteer
# __init__.py

__chromium_revision__ = '1267556' 
# 1267556  1181205

# D:\python\Lib\site-packages\pyppeteer
# chromium_downloader.py

downloadURLs = {
    'linux': f'{BASE_URL}/Linux_x64/{REVISION}/chrome-linux.zip',
    'mac': f'{BASE_URL}/Mac/{REVISION}/chrome-mac.zip',
    'win32': f'{BASE_URL}/Win/{REVISION}/{windowsArchive}.zip',
    'win64': f'http://cdn.npm.taobao.org/dist/chromium-browser-snapshots/Win_x64/1267556/chrome-win.zip',
}
```

> #### 再次运行

?> 安装路径下，自动生成目录 1267556，并解压 chrome-win.zip <br>
安装路径：C:\Users\Z\AppData\Local\pyppeteer\pyppeteer\local-chromium\1267556\chrome-win

```
[INFO] Starting Chromium download.
100%|████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 229M/229M [00:48<00:00, 4.70Mb/s]
[INFO] Beginning extraction
[INFO] Chromium extracted to: C:\Users\Z\AppData\Local\pyppeteer\pyppeteer\local-chromium\1267556
```

> #### 离线安装

?> 也可以离线安装，配置完文件，镜像下载压缩包 chrome-win.zip，放在手动创建的版本号目录，运行时会自动解压 <br>
国内镜像：https://npm.taobao.org/mirrors/chromium-browser-snapshots/ <br>
创建版本号目录：C:\Users\Z\AppData\Local\pyppeteer\pyppeteer\local-chromium\



> ### 3. 点击操作

?> 示例：点击折叠侧边栏

``` python
import asyncio
import time
from pyppeteer import launch

base_url = r"https://alance9.gitee.io/auto-test/#/main/pyPic"

async def main(url=base_url):

    driver = await launch()
    page = await driver.newPage()

    # 访问网站
    await page.goto(url)

    # 点击折叠侧边栏
    button = await page.querySelector(
        'body > main > button > div')
    await button.click()
    time.sleep(3)

    await driver.close()

if __name__ == '__main__':
    asyncio.get_event_loop().run_until_complete(main())
```


> ### 4. 全屏截图


?> 示例：[图像处理](https://alance9.gitee.io/auto-test/#/main/pyPic) 全屏截图 

查看截图：<a href='./_media/resource/shot.png' target='blank'>Click Here</a> 

``` python
import asyncio
import os
import time
from pyppeteer import launch

base_url = r"https://alance9.gitee.io/auto-test/#/main/pyPic"

async def main(url=base_url):

    driver = await launch()
    page = await driver.newPage()

    # 访问网站
    await page.goto(url)

    # 点击折叠侧边栏
    button = await page.querySelector(
        'body > main > button > div')
    await button.click()
    time.sleep(3)

    # 滚到到最低部，解决慢加载
    await page.evaluate('''async () => {
                    await new
                Promise((resolve, reject) => {
                    var
                totalHeight = 0;
                var
                distance = 100;
                var
                timer = setInterval(() => {
                    var
                scrollHeight = document.body.scrollHeight;
                window.scrollBy(0, distance);
                totalHeight += distance;
                if (totalHeight >= scrollHeight){
                clearInterval(timer);
                resolve();
                }
                }, 100);
                });
        }''')

    # 截图
    os.makedirs('media', exist_ok=True)
    await page.screenshot({'path': './media/test.png', 'fullPage': True})

    await driver.close()

if __name__ == '__main__':
    asyncio.get_event_loop().run_until_complete(main())
```


> ### 5. 数据抓取


?> 示例：[图像处理](https://alance9.gitee.io/auto-test/#/main/pyPic) 抓取内容，生成 MD 文档 <br>
示例：[图像处理](https://alance9.gitee.io/auto-test/#/main/pyPic) 抓取链接，生成 TXT 文档

<!-- MD 文档：<a href='./_media/resource/getMd.md' target='blank'>Click Here</a>  -->

``` python
import asyncio
import os
import time
from pyppeteer import launch

base_url = r"https://alance9.gitee.io/auto-test/#/main/pyPic"

async def main(url=base_url):

    driver = await launch()
    page = await driver.newPage()

    # 访问网站
    await page.goto(url)

    # 点击折叠侧边栏
    button = await page.querySelector(
        'body > main > button > div')
    await button.click()
    time.sleep(3)

    # 抓取数据  class
    element = await page.querySelector('.markdown-section')
    content = await page.evaluate('(element) => element.textContent', element)

    os.makedirs('media', exist_ok=True)
    with open('./media/test.md', 'w', encoding='utf-8') as f:
        f.writelines(content)

    # 抓取链接  tag
    element = await page.querySelector('a')
    content = await page.evaluate('(element) => element.textContent', element)

    with open('./media/link.txt', 'w', encoding='utf-8') as f:
        f.writelines(content)   # 首页

    await driver.close()

if __name__ == '__main__':
    asyncio.get_event_loop().run_until_complete(main())
```


> ### 6. 生成 PDF


?> 示例：[图像处理](https://alance9.gitee.io/auto-test/#/main/pyPic) 保留样式，生成图文并茂的 PDF 文档

PDF 文档：<a href='./_media/resource/getPdf.pdf' target='blank'>Click Here</a> 

``` python
import asyncio
import os
import time
from pyppeteer import launch

base_url = r"https://alance9.gitee.io/auto-test/#/main/pyPic"

async def main(url=base_url):

    driver = await launch()
    page = await driver.newPage()

    # 访问网站
    await page.goto(url)

    # 点击折叠侧边栏
    button = await page.querySelector(
        'body > main > button > div')
    await button.click()
    time.sleep(3)

    os.makedirs('media', exist_ok=True)

    # 生成图文并茂的 PDF
    await page.pdf({
        "path": "./media/test.pdf", "format": 'A4'})

    await driver.close()

if __name__ == '__main__':
    asyncio.get_event_loop().run_until_complete(main())
```


> ### 7. 参考示例


?> 示例：点击折叠侧边栏，全屏截图，抓取内容生成 MD，抓取链接生成 TXT

``` python
# pip install pyppeteer
# https://npm.taobao.org/mirrors/chromium-browser-snapshots/

import asyncio
import os
import time
from pyppeteer import launch

base_url = r"https://alance9.gitee.io/auto-test/#/main/pyPic"
w, h = 2180, 2500


async def main(url=base_url):

    driver = await launch(headless=False,  # 无头模式
                          defaultViewport={
                              "width": 1920, "height": 1080},  # 视图
                          isMobile=False,
                          args=['--start-maximized',
                                '--no-sandbox'],  # 最大化，禁用沙箱
                          ignoreDefaultArgs=[
                              '--enable-automation']  # 禁止显示正受自动软件控制
                          )
    page = await driver.newPage()

    # 访问网站
    await page.goto(url)
    # 等待
    await page.waitFor(1)
    await page.waitForSelector('.sidebar-toggle-button', {'timeout': 3000})

    # 设置浏览器大小
    await page.setViewport({'width': w, 'height': h})

    # 点击折叠侧边栏
    button = await page.querySelector(
        'body > main > button > div')
    await button.click()
    time.sleep(3)

    # 滚到到最低部，解决慢加载
    await page.evaluate('''async () => {
                    await new
                Promise((resolve, reject) => {
                    var
                totalHeight = 0;
                var
                distance = 100;
                var
                timer = setInterval(() => {
                    var
                scrollHeight = document.body.scrollHeight;
                window.scrollBy(0, distance);
                totalHeight += distance;
                if (totalHeight >= scrollHeight){
                clearInterval(timer);
                resolve();
                }
                }, 100);
                });
        }''')
    
    # 截图
    os.makedirs('media', exist_ok=True)
    await page.screenshot({'path': './media/test.png', 'fullPage': True})

    # 抓取数据  class
    element = await page.querySelector('.markdown-section')
    content = await page.evaluate('(element) => element.textContent', element)

    with open('./media/test.md', 'w', encoding='utf-8') as f:
        f.writelines(content)

    # 抓取链接  tag
    element = await page.querySelector('a')
    content = await page.evaluate('(element) => element.textContent', element)

    with open('./media/link.txt', 'w', encoding='utf-8') as f:
        f.writelines(content)

    # 生成图文并茂的 PDF
    await page.pdf({
        "path": "./media/test.pdf", "format": 'A4'})

    await driver.close()

if __name__ == '__main__':
    asyncio.get_event_loop().run_until_complete(main())
```


