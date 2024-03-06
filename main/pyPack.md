<!-- PyInstaller -->

> ### 1. 应用场景

?> PyInstaller 打包测试小工具，生成执行文件，分享给他人使用（无需配置环境），且兼容多个平台（win、linux、mac）。


> ### 2. 示例参考

?> 示例：为了测试导入文件的性能效率，打包一个生成指定数据量 CSV 的测试小工具 <br>
<br>
-F 生成一个执行文件 <br>
-i 指定文件图标 <br>
-n 重命名 <br>
-p 引入依赖包 <br>
<br>
pyinstaller -i .\favlove.ico -F .\batchData.py -n easyData <br>


``` python
# pip install pandas -i https://pypi.tuna.tsinghua.edu.cn/simple
# pip install pyinstaller -i https://pypi.tuna.tsinghua.edu.cn/simple

import os
import random
import sys
import pandas


th = ['name', 'age', 'address']
address = ['ShenZhen']
path = './media/batchImport.csv'


def batch(num):
    num = int(num)
    os.makedirs('media', exist_ok=True)
    names = []
    ages = []
    for i in range(0, num):
        names.append("".join(random.sample(
            'abcdefghijklmnopqrstuvwxyz0123456789', 8)))
        ages.append(random.randint(18, 100))
    addr = address * num
    dict = {th[0]: names, th[1]: ages, th[2]: addr}
    # print(names, ages, dict)
    # 保存 csv
    pandas.DataFrame(dict).to_csv(path)
    print(f"批量文件生成成功：{path}")
    return 'Success!'


# batch()

if __name__ == '__main__':
    print(f'Start.')
    try:
        r = batch(sys.argv[1])
        print(f'输入数据量：{sys.argv[1]} \n运行结果：', str(r))
    except:
        print('请输入参数--文件生成的数据量')
    print('End.')
```
```
pyinstaller -i .\favlove.ico -F .\batchData.py -n easyData

20483 INFO: Copying bootloader EXE to E:\Code\pyMore\dist\easyData.exe
20669 INFO: Copying icon to EXE
20866 INFO: Copying 0 resources to EXE
20866 INFO: Embedding manifest in EXE
21051 INFO: Appending PKG archive to EXE
21082 INFO: Fixing EXE headers
22009 INFO: Building EXE from EXE-00.toc completed successfully.
```

> ### 3. 运行结果

?> 生成 easyData.spec、build 目录、dist 目录

![easy](../_media/resource/easy.png ':size=40%')

?> 传入命令行参数，生成 CSV 文件 <br>

CSV 文件：<a href='./_media/resource/batchImport.csv' target='blank'>Click Here</a> 

```
.\easyData.exe 100

Start.
批量文件生成成功：./media/batchImport.csv
输入数据量：100
运行结果： Success!
End.

.\easyData.exe   

Start.
请输入参数--文件生成的数据量
End.
```
