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

- #### <font color=red>__init__.py作用</font>
    - python的每个模块的包中必须有一个__init__.py文件，才能导入这个目录下的module
    - 在__init__.py 里面还是可以有内容的，我们在导入一个包时，实际上导入了它的__init__.py文件
    ```
    app
    |\__module1
    |   |__ __init__.py
    |   |__ python1.py
    |   |__ python2.py
    |___ python2.py
    ```

    在__init__.py文件中写
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