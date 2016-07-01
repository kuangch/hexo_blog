---
title: python 导入模块详解
date: 2016-06-29 21:50:45
tags:
- 模块导入
-
categories: Python
---

- #### 当我们在要导入的模块和当前python在文件同一目录下
```
app
|\__ python1.py
|___ python2.py
```
    要在python2中导入python1模块只需：
``` python
from python1 import *
```

- #### 当我们在要导入的模块在当前文件的同一目录下的目录中
```
app
|\__ module1
|   |__ python1.py
|___ python2.py
```
    要在python2中导入python1模块需要：在module1目录下创建一个 `__init__.py` 文件
```
app
|\__module1
|   |__ __init__.py
|   |__ python1.py
|___ python2.py
```
    然后导入
``` python
from module1.python1 import *
```

- #### 当我们在要导入的模块在当前文件父目录的同一级目录下的目录中
```
app
|\__ module1
|   |__ python1.py
|\__ module2
|   |__ python2.py
```
    要在python2中导入python1模块需要：在module1目录下创建一个 `__init__.py` 文件
```
app
|\__module1
|   |__ __init__.py
|   |__ python1.py
|\__ module2
|   |__ python2.py
```
    然后将父级目录添加到pthon模块搜索路径，在导入
``` python
import sys
sys.path.append("..")
from module1.python1 import *
```

- #### <font color=red>`__init__.py`的作用</font>
    - python的每个模块的包中必须有一个`__init__.py`文件，才能导入这个目录下的module
    - 在`__init__.py` 里面还是可以有内容的，我们在导入一个包时，实际上导入了它的`__init__.py`文件
    ```
    app
    |\__module1
    |   |__ __init__.py
    |   |__ python1.py
    |   |__ python2.py
    |___ python2.py
    ```

    在`__init__.py`文件中写
    ``` python
        import python1
        import python2
        # or
        from python1 import *
        from python2 import *
    ```
    这样，当我们导入`module1` 这个包的时候`__init__.py`文件自动运行。帮我们导入了这么多个模块，我们就不需要将所有的import语句写在一个文件里了，也可以减少代码量。不需要一个个去导入module了
    ``` python
        from module1 import *
        ```
    - 文件`__init__.py` 中还有一个重要的变量，叫做<font color="red">`__all__`</font>。我们有时会使出一招“全部导入”，也就是这样：
    ``` python
    from module1 import *
    ```
    这时 import 就会把注册在包`__init__.py` 文件中`__all__` 列表中的子模块和子包导入到当前作用域中来。比如：

    ``` python
    #文件 __init__.py
    __all__ = ["module1", "python1", "python2"]
    ```

- #### <font color=red>`__all__` 的作用</font>
    - 列出模块中要导出的类，函数，变量等成员变量，避免命名冲突
    > 在模块(*.py)中使用意为导出__all__列表里的类、函数、变量等成员，
    否则将导出modualA中所有不以下划线开头（私有）的成员，
    在模块中使用__all__属性可避免在相互引用时的命名冲突

    ``` python
    # modualA.py

    import os
    import sys

    def fun1():
        pass

    def fun2():
        pass

    def class1:
        pass

    __all__=["fun1","class1"]

    # 使用：
    from modualA import *

    # 这时只能使用fun1不能使用fun2,并且排除了 os, sys
    ```

    - 列出包里面导出的模块
        > 在包(假设pkgA，pkgA是一个文件夹)的 `__init__.py` 中意为导出包里的模块

        ``` python
        # pkgA/__init__.py

        from modualA import class1,class2
        from modualB import fun1,class3

        __all__=["modualA","modualB"]


        # 使用：
        from pkgA import *
        ```
        以上语句即执行了pkgA下的 `__init__.py` ，导入两个模块，和这两模块下的函数和类
        按照 PEP8 建议的风格，`__all__` 应该写在所有 import 语句下面，和函数、常量等模块成员定义的上面。