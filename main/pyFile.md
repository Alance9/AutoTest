<!-- 文件处理 -->

> ### 1. 文件操作模式

?> open(file, mode='r', buffering=-1, encoding=utf8) <br>
<br>
<b> mode </b> <br>
r：只读 <br>
w：写 <br>
a：追加 <br>
b: 处理二进制文件，如图片、视频 <br>
+：可读写 <br>
<br>
r+：不存在报错，左到右覆盖内容，焦点在第一位 <br>
w+：不存在新建，覆盖所有内容，焦点在第一位 <br>
a+：不存在新建，追加内容，焦点在最后 <br>
读取所有数据，需把焦点定在首位：file.seek(0) <br>

> ### 2. 文件生命周期

?> 文件夹创建、销毁 <br>
文件创建、销毁 <br>
文件打开、写入、读取、关闭 <br>

``` python
import os
import shutil

p = './python/file'
f = 'test.txt'
fp = os.path.join(p, f)

# 创建文件夹
if not os.path.exists(p):
    os.makedirs(p)

# 创建文件
if not os.path.exists(fp):

    # 方式一，手动关闭文件
    f = open(fp, 'w+')
    # 写入文件
    s = 'It is a Cat.\nYes.'
    f.writelines(s)
    # 关闭文件
    f.close()

# 方式二，文件自动关闭
with open(fp, 'r+') as file:
    # 读取文件
    content = file.read()
    print(content)
    # It is a Cat.
    # Yes.    

    # 文件定位
    file.seek(0)
    content = file.read(12)
    print(content)  # It is a Cat.

# 文件检索
for f_name in os.listdir(p):
    if fnmatch.fnmatch(f_name, '*.txt'):
        print(f_name)   # test.txt

# 删除文件
os.remove(fp)

# 删除空文件夹
os.rmdir(p)

# 删除非空文件夹
shutil.rmtree(p)
```

> ### 3. 文件检索、重命名

``` python
import fnmatch
import os

p = './python/file'
f = 'test.txt'

# 文件检索
for f_name in os.listdir(p):
    if fnmatch.fnmatch(f_name, '*.txt'):
        print(f_name)

# 重命名文件
os.rename(fp, os.path.join(p, '123.txt'))
os.rename(os.path.join(p, '123.txt'), os.path.join(p, 'test.txt'))
```
```
test.txt
```

> ### 4. 文件复制、移动

``` python
import os
import shutil

p = './python/file'
f = 'test.txt'

# 复制文件
d = shutil.copytree('./python/file', './file')
time.sleep(10)
print(d)
shutil.rmtree('./file')

# 移动文件
d = shutil.move(fp, './python')
print(d)
shutil.move('./python/test.txt', p)
```
```
./file
./python\test.txt
```

> ### 5. 压缩解压 ZIP / TAR

> #### ZIP 格式

``` python
import zipfile
import os


p = './python/next'
f1 = 'test1.txt'
f2 = 'test2.txt'
f3 = 'test3.txt'
fp1 = os.path.join(p, f1)
fp2 = os.path.join(p, f2)
fp3 = os.path.join(p, f3)

# 创建文件
if not os.path.exists(fp1):
    f = open(fp1, 'w+')
    f.close()

if not os.path.exists(fp2):
    f = open(fp2, 'w+')
    f.close()

if not os.path.exists(fp3):
    f = open(fp3, 'w+')
    f.close()

# 压缩文件
files = [fp1, fp2, fp3]
with zipfile.ZipFile('./python/new.zip', 'w') as zip:
    for f in files:
        zip.write(f)

# 解压文件
zf = zipfile.ZipFile('./python/new.zip', 'r')
zf.extractall(path='./python/last/')
zf.close()
print(os.listdir('./python/last/python/next/'))

# 解压加密文件
with zipfile.ZipFile('./python/new.zip', 'r') as zip:
    zip.extractall(path='./python/last/', pwd='')
    # 获取文件列表
    print(zip.namelist())
```
```
['test1.txt', 'test2.txt', 'test3.txt']
['python/next/test1.txt', 'python/next/test2.txt', 'python/next/test3.txt']
```

> #### TAR 格式

``` python
import os
import tarfile


p = './python/next'
f1 = 'test1.txt'
f2 = 'test2.txt'
f3 = 'test3.txt'
fp1 = os.path.join(p, f1)
fp2 = os.path.join(p, f2)
fp3 = os.path.join(p, f3)

# 创建文件
if not os.path.exists(fp1):
    f = open(fp1, 'w+')
    f.close()

if not os.path.exists(fp2):
    f = open(fp2, 'w+')
    f.close()

if not os.path.exists(fp3):
    f = open(fp3, 'w+')
    f.close()
    
# 压缩文件
with tarfile.open('./python/new1.zip', 'w') as tar:
     for f in files:
        tar.add(f)
        
# 解压文件
tar = tarfile.open('./python/new1.zip', 'r')
tar.extractall(path='./python/last/')
print(os.listdir('./python/last/python/next/'))
# 获取文件列表
print(tar.getnames())
```
```
['test1.txt', 'test2.txt', 'test3.txt']
['./python/next/test1.txt', './python/next/test2.txt', './python/next/test3.txt']
```


> ### 6. 文件属性

``` python
from pathlib import Path
import time

# 格式化更新时间
def timeOut(times):
    dt = time.strftime("%Y-%m-%d %H:%M:%S", time.localtime(times))
    return dt

# os.stat_result(st_mode=33206, st_ino=562949953421659, st_dev=810779109, st_nlink=1, st_uid=0, st_gid=0, st_size=2932, st_atime=1708140037, st_mtime=1702144044, st_ctime=1701961348)

path = Path('python')

# 获取路径下的所有文件信息
# 打印文件名、更新时间、创建时间、文件大小
for f in path.iterdir():
    if f.is_file():
        info = f.stat()
        # print(info)
        print(f'{f.name} 更新时间 {timeOut(info.st_mtime)} 创建时间 {timeOut(info.st_mtime)} 文件大小 ' + '%.3f' % (info.st_size/1024/1024) + 'M')
```

```
basic.py 更新时间 2023-12-10 01:47:24 创建时间 2023-12-10 01:47:24 文件大小 0.003M
pyCmd.py 更新时间 2024-02-17 10:28:00 创建时间 2024-02-17 10:28:00 文件大小 0.002M
```