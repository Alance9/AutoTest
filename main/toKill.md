<!-- 清理进程 -->

?> 清理某端口的进程及子进程 <br>
查询端口：netstat -ano | findstr port <br>
清理进程：taskkill /pid pid /t /f

``` python
#  kill.py

import sys  
import os  


port = sys.argv[1]
r = os.popen("netstat -ano | findstr " + port)
ls = r.read().split("\n")
print("进程个数为：", len(ls)-1)

for i in ls:
    j = i.split(" ")
    if len(j) > 1:
        pid = j[len(j)-1]
        print('端口：' + pid)
        os.system("taskkill /pid "+pid+" /t /f")
r.close()
```

```
python kill.py 8080

进程个数为： 13
端口：105528
成功: 已终止 PID 37136 (属于 PID 105528 子进程)的进程。
成功: 已终止 PID 105528 (属于 PID 111620 子进程)的进程。
......
```