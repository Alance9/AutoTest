<!-- 日志处理 -->

> ### 1. 应用场景

?> 日志作用：记录关键信息，排查报错的快速手段，支持格式化和备份。

> ### 2. 日志语法


| 日志等级 | |
| -- | -- |
| DEBUG | 调试信息 |
| INFO | 关键信息 |
| WARNING | 警告信息，不影响运行 |
| ERROR | 错误信息，影响部分功能 |
| CRITICAL | 严重错误，影响整个程序 |


| 日志组件 | |
| -- | -- |
| Logger | 日志基本功能 |
| Formatter | 格式化，决定日志的输出格式 |
| Handler | 保存日志，定义路径、大小、数量 |
| Filter | 日志过滤器 |


| Logger | |
| -- | -- |
| addHandler() / removeHandler() | 添加、移除 Handler |
| addFilter() / removeFilter() | 添加、移除 Filter |
| setLevel() | 设置日志处理级别 |
| logging.debug() | 打印调试信息 |
| logging.info() | 打印关键信息 |
| logging.warning() | 打印警告信息 |
| logging.error() | 打印错误信息 |
| logging.critical() | 打印严重错误信息 |


| Handler | |
| -- | -- |
| Handler.setLevel() | 设置日志处理最低级别 |
| RotatingFileHandler() | 按大小切割，保存日志 |
| TimeRotatingFileHandler() | 按时间切割，保存日志 |
| logging.StreamHandler() | 控制台打印 |
| logging.handlers.HTTPHandler() | 日志发送到服务器 |
| logging.handlers.SMTPHandler() | 日志发送到 email |


| Formatter | |
| -- | -- |
| %(levelno)s / %(levelname)s | 日志级别 |
| %(pathname)s | 路径 |
| %(filename)s | 文件 |
| %(funcName)s | 方法 |
| %(lineno)d | 日志行号 |
| %(asctime)s | 日志时间 |
| %(thread)d / %(threadName)s | 线程 |
| %(process)d | 进程 |
| %(message)s | 日志信息 |


> ### 3. 示例参考

``` python
# myLogger.py

import os
import time
import logging
from logging.handlers import RotatingFileHandler


class MyLogger(logging.Logger):

    """ 重写日志初始化 """

    def __init__(self, level='DEBUG'):

        # 继承原 logger 初始化
        super().__init__('mylog')

        # 格式化
        fmt = '%(asctime)s %(filename)s %(funcName)s [line- %(lineno)d] %(levelname)s %(message)s'
        format = logging.Formatter(fmt)

        # 控制台打印
        console = logging.StreamHandler()
        # 设置处理日志等级
        console.setLevel(level)
        # 设置日志格式
        console.setFormatter(format)
        # 添加 Handler 处理器
        self.addHandler(console)

        # 定义日志
        if not os.path.exists('log'):
            os.makedirs('log')
        date = time.strftime(
            'Log_%Y-%m-%d %H%M%S', time.localtime(time.time()))
        file = f'log/{date}.log'

        # 日志打印
        # 定义路径、编码、最大（10M）、切割保留数量（1）
        log = RotatingFileHandler(
            file, encoding='UTF-8', maxBytes=1024*1024*10, backupCount=1)
        # 设置处理日志等级
        log.setLevel(level)
        # 设置日志格式
        log.setFormatter(format)
        # 添加 Handler 处理器
        self.addHandler(log)


logger = MyLogger()         
```

***

``` python
# 引用 logger

from common.myLogger import logger


logger.debug('访问首页成功')
logger.info('访问首页成功')
logger.warning('访问首页成功')
logger.error('访问首页成功')
logger.critical('访问首页成功')
```
```
# 2024-02-26 223520.log

2024-02-26 22:35:20,822 test_api.py test_index1 [line- 90] DEBUG 访问首页成功
2024-02-26 22:35:20,822 test_api.py test_index1 [line- 91] INFO 访问首页成功
2024-02-26 22:35:20,822 test_api.py test_index1 [line- 92] WARNING 访问首页成功
2024-02-26 22:35:20,823 test_api.py test_index1 [line- 93] ERROR 访问首页成功
2024-02-26 22:35:20,823 test_api.py test_index1 [line- 94] CRITICAL 访问首页成功
```