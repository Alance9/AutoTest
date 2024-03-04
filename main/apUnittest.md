<!-- unittest -->

> ### 1. unittest 入门

?> 简单介绍一下 unittest，后面主要使用 pytest <br>
unittest 为 python 内置库，pytest 是 plus 版本 <br>
<br>
unittest 特性 <br>
测试类继承 unittest.TestCase <br>
测试方法命名 test 开头 (test*.py) <br>
<br>
测试断言 <br> 
assertEqual、assertNotEqual、、assertTrue、assertFalse、assertIn、assertNotIn <br>
<br>
测试前后置 <br>
setUpClass、setUp、tearDown、tearDownClass <br>
setUpClass、tearDownClass 前，需加 @classmethod <br>

``` python
# 加载多个测试用例文件（测试用例路径）
suite = unittest.defaultTestLoader.discover(dir, "test*.py")

# 生成测试报告：TextTestRunner
with open(file.txt, "w") as f:
    runer = unittest.TextTestRunner(stream=f, verbosity=2)  
    runer.run(suite)

# 生成测试报告：HTMLTestRunner
with open(file.html, "wb") as f:
    runner = HTMLTestRunner(stream=f, verbosity=2, title="", description="")
    runner.run(suite)
```

> ### 2. 示例参考 - 简单计算器测试

?> 简单计算器应用：加减乘除 <br>
了解前后置、断言，加载用例，生成报告 <br>
<br>
项目结构： <br>
pc.py   -- 计算机类 <br>
pc_test.py  -- 测试用例 <br>
pc_main.py  -- 入口文件 <br>

![cul](../_media/resource/cul.png ':size=20%')

***

``` python
# pc.py

class pc(object):
    def add(self, x, y):
        return x+y

    def sub(self, x, y):
        return x-y

    def mul(self, x, y):
        return x*y

    def div(self, x, y):
        try:
            return float(x) / float(y)
        except:
            return None


if __name__ == '__main__':
    pc = pc()
    print(pc.add(100, 4))
    print(pc.sub(100, 4))
    print(pc.mul(100, 4))
    print(pc.div(100, 4))
```
```
104
96
400
25.0
```


> ### 3. 示例参考 - 测试用例


``` python
# pc_test.py

import unittest
from pc import pc


class pc_unit(unittest.TestCase):

    @classmethod
    def setUpClass(cls):
        print('-- 测试类 start --')

    @classmethod
    def tearDownClass(cls):
        print('\n-- 测试类 end --')

    def setUp(self):
        print('\n-- 测试用例 start --')

    def tearDown(self):
        print('-- 测试用例 end --')

    def test_pc1(self):
        print('test1')
        r = pc.add(self, 1, 2)
        self.assertEqual(3, r, '1 + 2')

    def test_pc2(self):
        print('test2')
        r = pc.add(self, 1, 2)
        self.assertEqual(1, r, '1 + 2')

    def test_pc3(self):
        print('test3')
        r = pc.div(self, 1, 0)
        self.assertEqual(1, r, '1 / 2')


if __name__ == '__main__':
    unittest.main()
```
```
-- 测试类 start --

-- 测试用例 start --
test1
-- 测试用例 end --
.
-- 测试用例 start --
test2
-- 测试用例 end --
F
-- 测试用例 start --
test3
-- 测试用例 end --
F
-- 测试类 end --

======================================================================
FAIL: test_pc2 (__main__.pc_unit)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "e:\Code\pyApi\unittest\pc_test.py", line 29, in test_pc2
    self.assertEqual(1, r, '1 + 2')
AssertionError: 1 != 3 : 1 + 2

======================================================================
FAIL: test_pc3 (__main__.pc_unit)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "e:\Code\pyApi\unittest\pc_test.py", line 34, in test_pc3
    self.assertEqual(1, r, '1 / 2')
AssertionError: 1 != None : 1 / 2

----------------------------------------------------------------------
Ran 3 tests in 0.001s
```


> ### 4. 示例参考 - 入口文件


``` python
# pc_main.py

import unittest

# 加载用例，测试用例：路径 unittest，以 test 结尾的 py 文件
suite = unittest.defaultTestLoader.discover("unittest", "*test.py")

# 生成报告
with open("unittest/report.txt", "w") as f:
    # verbosity=2 输出详细日志
    runer = unittest.TextTestRunner(
        stream=f, verbosity=2)
    runer.run(suite)
```

***

> 生成报告 report.txt

``` txt
test_pc1 (pc_test.pc_unit) ... ok
test_pc2 (pc_test.pc_unit) ... FAIL
test_pc3 (pc_test.pc_unit) ... FAIL

======================================================================
FAIL: test_pc2 (pc_test.pc_unit)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "E:\Code\pyApi\unittest\pc_test.py", line 29, in test_pc2
    self.assertEqual(1, r, '1 + 2')
AssertionError: 1 != 3 : 1 + 2

======================================================================
FAIL: test_pc3 (pc_test.pc_unit)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "E:\Code\pyApi\unittest\pc_test.py", line 34, in test_pc3
    self.assertEqual(1, r, '1 / 2')
AssertionError: 1 != None : 1 / 2

----------------------------------------------------------------------
Ran 3 tests in 0.003s

FAILED (failures=2)
```