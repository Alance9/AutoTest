<!-- 进程处理 -->

> ### 1. 查询进程

``` python
pip install psutil

import psutil

# 获取所有进程
pids = psutil.pids()

# 获取所有进程的应用名
names = []
print(f'查询 {app_name} 进程中 ...')
for pid in pids:
try:
    p = psutil.Process(pid)
    names.append(p.name())
except psutil.AccessDenied and psutil.NoSuchProcess:
    pass

# print(names)
# 判断程序是否运行中
if app_name in names:
    print(f'{app_name} 运行中')
else:
    print(f'{app_name} 非运行状态')

```

> ### 2. 启动进程

``` python
import subprocess

print(f'启动 {app_name}')
subprocess.Popen(path, shell=True)
```

> ### 3. 终止进程

``` python
import os

print(f'关闭 {app_name}')
# 解决乱码问题
os.system('chcp 65001 > nul')
# 杀死程序进程
os.system(fr'taskkill /f /im {app_name}')
```

> ### 4. 应用程序的生命周期

?> 应用程序：启动、运行、终止

``` python
import os
import subprocess
import time
import psutil
path = 'A:\Joplin\JoplinPortable.exe'
app_name = 'Joplin.exe'


class Joplin():

    def 启动(self, path):
        print(f'启动 {app_name}')
        subprocess.Popen(path, shell=True)
        time.sleep(10)

    def 运行中(self):
        # 获取所有进程
        pids = psutil.pids()
        # 获取所有进程的应用名
        names = []
        print(f'查询 {app_name} 进程中 ...')
        for pid in pids:
            try:
                p = psutil.Process(pid)
                names.append(p.name())
            except psutil.AccessDenied and psutil.NoSuchProcess:
                pass
        # print(names)
        if app_name in names:
            print(f'{app_name} 运行中')
            return True
        else:
            print(f'{app_name} 非运行状态')
            return False

    def 关闭(self):
        print(f'关闭 {app_name}')
        # 解决乱码问题
        os.system('chcp 65001 > nul')
        # 杀死程序进程
        os.system(fr'taskkill /f /im {app_name}')
        time.sleep(3)
        if self.运行中():
            print(f'关闭 {app_name} 进程失败')
            return False
        else:
            print(f'{app_name} 进程已关闭')

    def 重启(self, path):
        self.关闭()
        self.启动(path)


if __name__ == '__main__':
    app = Joplin()
    if app.运行中():
        print('------------ 关闭 ------------')
        app.关闭()
    print('------------ 启动 ------------')
    app.启动(path)
    app.运行中()
    print('------------ 关闭 ------------')
    app.关闭()
    print('------------ 重启 ------------')
    app.重启(path)
    app.运行中()
```

```
查询 Joplin.exe 进程中 ...
Joplin.exe 非运行状态
------------ 启动 ------------
启动 Joplin.exe
查询 Joplin.exe 进程中 ...
Joplin.exe 运行中
------------ 关闭 ------------
关闭 Joplin.exe
SUCCESS: The process "Joplin.exe" with PID 11180 has been terminated.
SUCCESS: The process "Joplin.exe" with PID 55640 has been terminated.
SUCCESS: The process "Joplin.exe" with PID 65148 has been terminated.
SUCCESS: The process "Joplin.exe" with PID 60320 has been terminated.
查询 Joplin.exe 进程中 ...
Joplin.exe 非运行状态
Joplin.exe 进程已关闭
------------ 重启 ------------
关闭 Joplin.exe
ERROR: The process "Joplin.exe" not found.
查询 Joplin.exe 进程中 ...
Joplin.exe 非运行状态
Joplin.exe 进程已关闭
启动 Joplin.exe
查询 Joplin.exe 进程中 ...
Joplin.exe 运行中
```