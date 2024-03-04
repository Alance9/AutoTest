<!-- pytest -->

> ### 1. pytest 入门

?> 基于 unittest 升级拓展，pytest 提供了更多自定义，适用于更复杂的测试场景。 <br>
<br>
<b> pytest 特性 </b><br>
支持匹配、标记测试用例，按需分组运行 <br>
支持跳过占位用例，自定义失败次数终止运行 <br>
支持并行运行，存在大规模用例时，减少执行时间 <br>
支持定义前置，共享不同范围下的前置条件，减少代码量 <br>
支持全局变量，获取可变参数，快速切换测试与生产环境 <br>
支持参数化用例，同一用例执行多种场景，减少代码量，提高运行效率 <br>
<br>
<b> 自动检测 </b><br>
测试文件：test_*.py 或 *_test.py <br>
测试类：以 Test_ 开头，不包含初始化 _init_ <br>
测试方法：以 test 开头


> ### 2. 断言与运行

| 断言 | | |
| -- | -- | -- |
| assert a == b | assert a != b |
| assert a<=b | assert a>=b |
| assert a in b | assert a not in b |
| assert a / assert a is True | assert not a / assert a is not True / assert a is Flase|

| 运行 | | |
| -- | -- | -- |
| -s | 打印 print 信息 |
| -v | 打印信息更详细，常使用 -vs |
| -k | 匹配运行
| -m | 按标记，分组运行 |
| -n | 多线程运行，适用于大规模用例 |

| 组合运行 | |
| -- | -- |
| pytest dir | 运行某个路径 |
| pytest *.py | 运行某个文件 |
| pytest \*.py::test_\* | 运行某个用例 |
| pytest -k key -m mark -vs | 匹配 + 标记，筛选运行，详细打印 |

***

> #### 直接运行

``` python
# 运行当前目录下的所有测试用例

import pytest


if __name__ == '__main__':
    pytest.main(['-vs'])
```

***

> #### 匹配运行

``` python
# pytest -k key -vs

import pytest
a = True


def test_login_suc():
    assert a


def test_login_fail():
    assert a


def test_logout_suc():
    assert a


def test_logout_suc():
    assert a
```
```
pytest -k login -vs

===================================================================================== test session starts ======================================================================================
platform win32 -- Python 3.9.13, pytest-7.4.3, pluggy-1.3.0 -- D:\python\python.exe
cachedir: .pytest_cache
rootdir: E:\Code\pyApi\pytest
configfile: pytest.ini
plugins: xdist-3.5.0
collected 3017 items / 3015 deselected / 2 selected

test_c.py::test_login_suc PASSED
test_c.py::test_login_fail PASSED

============================================================================== 2 passed, 3015 deselected in 0.21s ============================================================================== 
```

***

> #### 标记运行

?> 标记：先在配置文件里，注册标记，再标记用例 <br>
选择其一，注册标记：pytest.ini、conftest.py

``` python
# 选择其一，注册标记

# pytest.ini
[pytest]
markers = 
    a:a
    b:b


# conftest.py
def pytest_configure(config):
    config.addinivalue_line("markers", "a")
    config.addinivalue_line("markers", "b")
```

``` python
# 标记用例
# pytest -m mark -vs

import pytest
xx = True


@pytest.mark.a
def test1():
    assert xx


@pytest.mark.a
def test11():
    assert xx


@pytest.mark.b
def test2():
    assert xx


@pytest.mark.b
def test22():
    assert xx
```
```
pytest -m a -vs

===================================================================================== test session starts ======================================================================================
platform win32 -- Python 3.9.13, pytest-7.4.3, pluggy-1.3.0 -- D:\python\python.exe
cachedir: .pytest_cache
rootdir: E:\Code\pyApi\pytest
configfile: pytest.ini
plugins: xdist-3.5.0
collected 3019 items / 3017 deselected / 2 selected

test_c.py::test1 PASSED
test_c.py::test11 PASSED

============================================================================== 2 passed, 3017 deselected in 0.19s ============================================================================== 
```

***

> #### 并行运行

?> 并行场景：构造 3000 个测试用例，分为 3 个 worker 运行，对比单个 worker 的运行时间差 <br>
<br>
pip install pytest-xdist

``` python
# pytest .\test_3000.py -n num
# 构造 3000 个测试用例，生成 test_3000.py

import os

s = 'import pytest\na = True\n'
s1, s2 = 'def test', '(): assert a'

for i in range(1000, 4000):
    s = s + s1 + str(i) + s2 + '\n'
print(s)

with open('test_3000.py', 'w') as f:
    f.writelines(s)
```
```
pytest .\test_3000.py
collected 3000 items
3000 passed in 1.88s

pytest .\test_3000.py -n 3
3 workers [3000 items]
3000 passed in 1.37s
```


> ### 3. 跳过执行

?> 应用场景：部分功能未实现时，打桩占位，不计入 PASSED 或 FAILED，后续再完善用例 <br>
<br>
<b> skip 与 xfail 区别： </b><br>
正常执行用例，最终状态为 PASSED 或 FAILED <br>
skip 完全跳过用例执行，最终状态为 SKIPPED <br>
xfail 执行用例，最终状态为 XPASS 或 XFAIL <br>


| 运行状态 | | 描述 |
| -- | -- | -- |
| PASSED | | 通过 |
| FAILED | | 失败 |
| | @pytest.mark.xfail(raises=RuntimeError) | 抛出非断言异常 |
| SKIPPED (unconditional skip) | @pytest.mark.skip | 跳过用例不执行 |
| XFAIL ([NOTRUN] ) | @pytest.mark.xfail(run=False) | 跳过用例不执行 |
| XPASS | @pytest.mark.xfail | 执行通过 |
| XFAIL | @pytest.mark.xfail | 执行失败 |
| | @pytest.mark.xfail(raises=AssertionError) | 抛出断言异常 |


``` python
# test_cc.py

import pytest
a = True


def test1():
    print('1')
    assert a

def test2():
    print('2')
    assert not a

@pytest.mark.skip
def test3():
    print('3')
    assert a

@pytest.mark.xfail
def test4():
    print('4')
    assert a

@pytest.mark.xfail
def test5():
    print('5')
    assert not a
    
@pytest.mark.xfail(run=False)
def test6():
    print('6')
    assert not a

@pytest.mark.xfail(raises=AssertionError)
def test7():
    print('7')
    assert not a

@pytest.mark.xfail(raises=RuntimeError)
def test8():
    print('8')
    assert not a
```
```
pytest .\test_cc.py -vs 

collected 8 items                                        

test_cc.py::test1 1
PASSED
test_cc.py::test2 2
FAILED
test_cc.py::test3 SKIPPED (unconditional skip)
test_cc.py::test4 4
XPASS
test_cc.py::test5 5
XFAIL
test_cc.py::test6 XFAIL ([NOTRUN] )
test_cc.py::test7 7
XFAIL
test_cc.py::test8 8
FAILED

======================= FAILURES ======================= 
________________________ test2 _________________________ 

    def test2():
        print('2')
>       assert not a
E       assert not True

test_cc.py:19: AssertionError
________________________ test8 _________________________ 

    @pytest.mark.xfail(raises=RuntimeError)
    def test8():
        print('8')
>       assert not a
E       assert not True

test_cc.py:49: AssertionError
=============== short test summary info ================ 
FAILED test_cc.py::test2 - assert not True
FAILED test_cc.py::test8 - assert not True
= 2 failed, 1 passed, 1 skipped, 3 xfailed, 1 xpassed in 0.07s =
```

***

> ### 4. 重试终止

?> 应用场景：跑流水线时，可行性测试跑完所有用例，再反馈成功率，可能占用时间过久 <br>
可以判定只要失败一次，默认可行性验证不通过 <br>
或者，提供几次重试失败用例的机会，避免环境因素的影响 <br>
<br>
手动重试 <br>
失败终止：pytest \*.py --maxfail num -vs <br>
失败重试 - 只跑失败用例（--last-failed）：pytest \*.py --lf --vs <br>
失败重试 - 先跑失败用例，后跑通过用例（--failed-first ）：pytest \*.py --ff --vs <br>
<br>
自动重试 <br>
pip install pytest-rerunfailures <br>
pytest --reruns num --reruns-delay num <br>
reruns 重试次数，reruns-delay 间隔时间
``` 
# test_cc.py

# 第一次失败，终止执行
pytest .\test_cc.py --maxfail 1 -vs

collected 8 items                                        

test_cc.py::test1 1
PASSED
test_cc.py::test2 2
FAILED

!!!!!!!!!!!!!! stopping after 1 failures !!!!!!!!!!!!!!! 
============= 1 failed, 1 passed in 0.04s ==============


# 失败重试，只跑失败用例
pytest .\test_cc.py --lf -vs    

collected 8 items / 6 deselected / 2 selected
run-last-failure: rerun previous 2 failures

test_cc.py::test2 2
FAILED
test_cc.py::test8 8
FAILED

FAILED test_cc.py::test2 - assert not True
FAILED test_cc.py::test8 - assert not True
=========== 2 failed, 6 deselected in 0.04s ============ 


# 失败重试，先跑失败用例，后跑通过用例
pytest .\test_cc.py --ff -vs

collected 8 items
run-last-failure: rerun previous 2 failures first        

test_cc.py::test2 2
FAILED
test_cc.py::test8 8
FAILED
test_cc.py::test1 1
PASSED
test_cc.py::test3 SKIPPED (unconditional skip)
test_cc.py::test4 4
XPASS
test_cc.py::test5 5
XFAIL
test_cc.py::test6 XFAIL ([NOTRUN] )
test_cc.py::test7 7
XFAIL

FAILED test_cc.py::test2 - assert not True
FAILED test_cc.py::test8 - assert not True
= 2 failed, 1 passed, 1 skipped, 3 xfailed, 1 xpassed in 0.07s =
```


> ### 4. 参数化

?> 应用场景：同一个用例下，定义参数集合列表，含入参、对应的期望结果，测试同一功能的多种场景，减少代码量，提高运行效率。 <br>
<br>
示例：登录成功、用户名为空、密码为空、用户名不存在、密码错误，5 个测试用例参数化，合为一个用例

``` python
# 源代码

import pytest
import requests

base_url = 'http://127.0.0.1:8080'

# 期望结果
ex = [{
    'transok': 0,
    'code': 200,
    'msg': '登录成功'
}, {
    'transok': 1,
    'code': 202,
    'msg': '用户密码错误'
}, {
    'transok': 1,
    'code': 201,
    'msg': '用户密码不能为空'
}
]


""" 登录成功 """


def test_login_suc():
    data = {'user': 'test', 'pwd': '123'}
    resp = requests.post(url=base_url+'/login', data=data)
    assert resp.status_code == 200

    dc = ex[0]
    resp = resp.json()
    for key in dc.keys():
        assert dc[key] == resp[key]


""" 登录失败，用户为空 """


def test_login_failed1():
    data = {'user': '', 'pwd': '123'}
    resp = requests.post(url=base_url+'/login', data=data)
    assert resp.status_code == 200

    dc = ex[2]
    resp = resp.json()
    for key in dc.keys():
        assert dc[key] == resp[key]


""" 登录失败，密码为空 """


def test_login_failed2():
    data = {'user': 'test', 'pwd': ''}
    resp = requests.post(url=base_url+'/login', data=data)
    assert resp.status_code == 200

    dc = ex[2]
    resp = resp.json()
    for key in dc.keys():
        assert dc[key] == resp[key]


""" 登录失败，用户不存在 """


def test_login_failed3():
    data = {'user': 'admin', 'pwd': '123'}
    resp = requests.post(url=base_url+'/login', data=data)
    assert resp.status_code == 200

    dc = ex[1]
    resp = resp.json()
    for key in dc.keys():
        assert dc[key] == resp[key]


""" 登录失败，密码错误 """


def test_login_failed4():
    data = {'user': 'test', 'pwd': '456'}
    resp = requests.post(url=base_url+'/login', data=data)
    assert resp.status_code == 200

    dc = ex[1]
    resp = resp.json()
    for key in dc.keys():
        assert dc[key] == resp[key]
```
```
collected 5 items

test_apiD.py::test_login_suc PASSED
test_apiD.py::test_login_failed1 PASSED
test_apiD.py::test_login_failed2 PASSED
test_apiD.py::test_login_failed3 PASSED
test_apiD.py::test_login_failed4 PASSED

==================================================================== 5 passed in 0.24s ===================================================================== 
```

***

``` python
# 参数化，减少代码量，提高运行效率

import pytest
import requests

base_url = 'http://127.0.0.1:8080'

# 入参
data = [{'user': 'test', 'pwd': '123'}, {'user': '', 'pwd': '123'}, {
    'user': 'test', 'pwd': ''}, {'user': 'admin', 'pwd': '123'}, {'user': 'test', 'pwd': '456'}]

# 期望结果
ex = [{
    'transok': 0,
    'code': 200,
    'msg': '登录成功'
}, {
    'transok': 1,
    'code': 202,
    'msg': '用户密码错误'
}, {
    'transok': 1,
    'code': 201,
    'msg': '用户密码不能为空'
}
]

""" 登录参数化 """

@pytest.mark.parametrize("data, ex", [(data[0], ex[0]), (data[1], ex[2]), (data[2], ex[2]), (data[3], ex[1]), (data[4], ex[1])])
def test_login(data, ex):
    resp = requests.post(url=base_url+'/login', data=data)
    assert resp.status_code == 200

    resp = resp.json()
    for key in ex.keys():
        assert ex[key] == resp[key]
```
```
collected 5 items

test_apiD.py::test_login[data0-ex0] PASSED
test_apiD.py::test_login[data1-ex1] PASSED
test_apiD.py::test_login[data2-ex2] PASSED
test_apiD.py::test_login[data3-ex3] PASSED
test_apiD.py::test_login[data4-ex4] PASSED

==================================================================== 5 passed in 0.08s =====================================================================
```


> ### 5. 前置夹具

?> 夹具是一个抽象的名词，可以简单认为是，不同范围下的前置方法，满足某些条件，获取某些变量，在 conftest.py 里配置。 <br>
<br>
autouse=False，默认手动传参执行，True 自动执行 <br>
@pytest.fixture(scope="session", autouse=True)


| 夹具 | |  |
| -- | -- | -- |
| 无参数 | @pytest.fixture | 定义可变参数，供测试用例调用 |
| session | @pytest.fixture(scope=session) | 整个项目只运行一次 |
| package | @pytest.fixture(scope=package) | 每个 python 包运行一次，不常用 |
| module | @pytest.fixture(scope=module) | 每个 py 文件运行一次 |
| class | @pytest.fixture(scope=class) | 每个测试类运行一次 |
| function | @pytest.fixture(scope=function) | 每个测试用例运行一次 |
| | |
|<b> 前后置 </b> | | |
| setup_class | | 类前置 |
| teardown_class | | 类后置 |
| setup | | 函数前置 |
| teardown | | 函数后置 |



``` python
# conftest.py

import pytest


# 定义全局变量 status
@pytest.fixture
def status():
    code = 404
    print(f'\n-------- getCode: {code} --------')
    return code


# 定义全局变量 order，项目运行时，只生成一次
@pytest.fixture(scope="session")
def order():
    print('\n-------- order --------')
    return []


""" 作用域，由大到小 """


@pytest.fixture(scope="session")
def sess(order):
    order.append("session")
    print('-------- session --------')


@pytest.fixture(scope="package")
def pack(order):
    order.append("package")
    print('-------- package --------')


@pytest.fixture(scope="class")
def cls(order):
    order.append("class")
    print('-------- class --------')


@pytest.fixture(scope="module")
def mod(order):
    order.append("module")
    print('-------- module --------')


@pytest.fixture
def func(order):
    order.append("function")
    print('-------- function --------')

```

``` python
# test_aa.py

def test_data(status):
    assert status == 404
    
def test_order(sess, pack, mod, cls, func, order):
    assert order == ["session", "package", "module", "class", "function"]
```
``` 
pytest .\test_aa.py -vs

collected 2 items                                        

test_aa.py::test_data
--------getCode: 404--------
PASSED
test_aa.py::test_order
-------- order --------
-------- session --------
-------- package --------
-------- module --------
-------- class --------
-------- function --------
PASSED

================== 2 passed in 0.02s =================== 
```

***

``` python
# test_bb.py 

class Test_bb:

    def setup_class(self):
        print('\n-------- 类前置 --------')

    def teardown_class(self):
        print('-------- 类后置 --------')

    def setup(self):
        print('-------- 函数前置 --------')

    def teardown(self):
        print('\n-------- 函数后置 --------')
    
    def test_qhz(self):
        print('-------- 执行函数 --------')
```
```
pytest .\test_bb.py --disable-warnings -vs

test_bb.py::Test_bb::test_qhz
-------- 类前置 --------
-------- 函数前置 --------
-------- 执行函数 --------
PASSED
-------- 函数后置 --------
-------- 类后置 --------
```

> ### 6. conftest.py

?> 作用：注册标记、定义全局变量、设置前置条件、添加自定义命令行参数 <br>
<br>
示例：全局切换测试与生产环境的 API-URL <br>
<br>
存在 3 种环境： <br>
mock 模拟环境，即代码尚未完成时，测试 mock 的 API <br>
test 测试环境，指代码完成后，正式测试的 API <br>
pro 生产环境，代码部署后，实际用户使用的 API <br>
<br>
自定义命令参数 env，切换到模拟环境，全局调用 mock-url：pytest --env=mock \*.py -vs <br>


| 自定义命令行参数 | |
| -- | -- |
| name | 参数名 |
| action | 默认 store |
| default | 参数默认值 |
| help | 参数说明 |


``` python
# conftest.py

import subprocess
import pytest


def pytest_addoption(parser):
    # 自定义环境参数，添加到配置对象，默认测试环境 test
    parser.addoption('--env', action='store', default='test',
                     help='环境参数：mock-模拟环境、test-测试环境、pro-生产环境')


@pytest.fixture(scope="session")
def env(request):
    # 从配置对象，读取自定义参数的值
    # pytest --env=mcok 运行时，获取 env 的参数值 mock
    return request.config.getoption('--env')


@pytest.fixture(scope="session")
def base_url(env):

    # 按 env 切换到不同环境的 url
    if env == 'mock':
        # 模拟环境，启动本地 falsk 接口，正常
        subprocess.Popen('python api.py')
        return 'http://127.0.0.1:8080/'
    elif env == 'pro':
        # 生产环境，不存在
        return 'http://127.0.0.1:6060/'
    else:
        # 测试环境，不存在
        return 'http://127.0.0.1:7070/'


""" 以下部分无关主题，忽略 """


# 注册标记
def pytest_configure(config):
    config.addinivalue_line("markers", "a")
    config.addinivalue_line("markers", "b")


# 定义全局变量 status
@pytest.fixture
def status():
    code = 404
    print(f'\n-------- getCode: {code} --------')
    return code

# 定义全局变量 order，项目运行时，只生成一次
@pytest.fixture(scope="session")
def order():
    print('\n-------- order --------')
    return []


""" 作用域，由大到小 """


@pytest.fixture(scope="session")
def sess(order):
    order.append("session")
    print('-------- session --------')


@pytest.fixture(scope="package")
def pack(order):
    order.append("package")
    print('-------- package --------')


@pytest.fixture(scope="class")
def cls(order):
    order.append("class")
    print('-------- class --------')


@pytest.fixture(scope="module")
def mod(order):
    order.append("module")
    print('-------- module --------')


@pytest.fixture
def func(order):
    order.append("function")
    print('-------- function --------')
```

***

``` python
# test_login.py

import pytest
import requests

# base_url = 'http://127.0.0.1:8080'

# 期望结果
dc = [{
    'transok': 0,
    'code': 200,
    'msg': '登录成功'
}]


""" 登录成功 """


def test_login_suc(base_url):
    data = {'user': 'test', 'pwd': '123'}
    resp = requests.post(url= base_url + '/login', data=data)
    assert resp.status_code == 200

    resp = resp.json()
    for key in dc.keys():
        assert dc[key] == resp[key]
```
``` 
# 模拟环境，正常
pytest ./cases/test_login.py --env=mock -vs

collected 1 item

cases/test_login.py::test_login_suc test
127.0.0.1 - - [25/Feb/2024 05:46:19] "POST //login HTTP/1.1" 200 -
PASSED

==================================================================== 1 passed in 0.08s ===================================================================== 


# 测试环境、生产环境不存在，报错
pytest ./cases/test_login.py --env=test -vs
pytest ./cases/test_login.py --env=pro -vs

collected 1 item

cases/test_login.py::test_login_suc FAILED

E requests.exceptions.ConnectionError: HTTPConnectionPool(host='127.0.0.1', port=7070): Max retries exceeded with url: //login (Caused by NewConnectionError('<urllib3.connection.HTTPConnection object at 0x000001B9CE0B8400>: Failed to establish a new connection: [WinError 10061] 由于目标计算机积极拒绝， 无法连接。'))

==================================================================== 1 failed in 2.27s ===================================================================== 
```


> ### 7. pytest.ini

``` ini
[pytest]

; 注册标记
markers = 
    a : a
    b : b
    login : run login mark case

; 命令行参数
; pytest 运行，自动带以下参数
addopts = --env=mock --maxfail=3 --color=yes --tb=no --disable-warnings -vs --html=./report/result.html --self-contained-html --log-cli-level=INFO
; --capture=sys 
; --log-cli-level=ERROR 
; --show-capture=no 不显示 live log call，'no', 'stdout', 'stderr', 'log', 'all'
; --junitxml=./result.xml

generate_report_on_test = True

; 折叠 passed,skipped,failed,error,xfailed,xpassed,rerun
render_collapsed = passed,skipped,xfailed,xpassed,rerun

; 忽略某些目录，不做遍历
norecursedirs = src report util log

; 测试路径
testpaths = ./cases

; 测试文件
python_files = test*.py *test.py

; 测试类
python_classes = Test*

; 测试方法
python_functions = test_*

; 更详细的控制台日志
log_cli = true

; 设置日志级别
log_cli_level = INFO

; 设置日志日期格式
log_cli_date_format = %Y-%m-%d %H:%M:%S

; 设置日志消息格式
log_cli_format = %(asctime)s %(levelname)s %(message)s

; 开启，xpassed 归到 failed
xfail_strict = false
```


> ### 8. 测试报告

?> 测试报告：xml、pytest-html、allure <br>
<br>
pytest-html: [Click Here](main/apPytestRe) <br>
allure: [Click Here](main/uiAllure) <br>
<br>
XML: pytest --junitxml=./result.xml <br>



``` xml
<!-- report.xml -->

<?xml version="1.0" encoding="utf-8"?>
<testsuites>
    <testsuite name="pytest" errors="0" failures="0" skipped="0" tests="1" time="0.076"
        timestamp="2024-02-25T07:27:42.256509" hostname="MSI">
        <testcase classname="cases.test_login" name="test_login_suc" time="0.004" />
    </testsuite>
</testsuites>
```
