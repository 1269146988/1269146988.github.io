---
title: Python开发与编码规范
date: 2023-04-19 18:00:53
description: 为了让不同编码习惯的开发者更好的协作配合，并且形成良好的基础编码规范与风格，我以PEP8为基础，整理了工作中常见的不规范操作，最终形成了此Python编码规范与风格文档
categories:
- Python
---

# Python编码风格和编码规范

## 前言

​		为了让不同编码习惯的开发者更好的协作配合，并且形成良好的基础编码规范与风格，我以 [PEP8](https://www.python.org/dev/peps/pep-0008/) 为基础，整理了工作中常见的不规范操作，最终形成了此Python编码规范与风格文档

- <span style="color: red">**必须**</span>：开发人员必须采用
- <span style="color: orange">**建议**</span>：建议开发人员采用
- <span style="color: blue">**自愿**</span>：开发人员自行决定是否采用

注意：如未明确标识指明，则默认为 <span style="color: red">**必须**</span>

## 一、编码风格

### 1.  命名

> 在python中的标识符的命名规则，包括：文件夹、变量、函数、类、模块以及其他对象的名称

   - 【<span style="color: red">**必须**</span>】标识符只能包含字母、数字和下划线（_），不能包含其他特殊字符。
   - 【<span style="color: red">**必须**</span>】标识符的第一个字符必须是字母或下划线，不能是数字。
   - 【<span style="color: red">**必须**</span>】标识符的名称大小写敏感，即`name`和`Name`是不同的标识符。
   - 【<span style="color: red">**必须**</span>】如果标识符由多个单词组成，可以使用下划线将其分开，例如`first_name`，也被称为小驼峰命名法.
   - 【<span style="color: orange">**建议**</span>】标识符的长度没有限制，但建议不要超过79个字符。
     - <span class="good_example_span" style="color:#228b22">***正确示范***</span>
     ```python
     name
     age
     _student
     student_name
     sum1
     hello_world
     ```
       - <span class="bad_example_span" style="color:#dc143c">***错误示范***</span>
     ```python
     2student  # 标识符不能以数字开头
     hello-world  # 标识符不能包含连字符
     print  # 内置函数不能用作标识符
     ```



### 2.  字符编码

   - 【<span style="color: orange">**建议**</span>】文件全部使用 `UTF-8` 编码

     - 文件头格式如下

     ```python
     # -*- coding: UTF-8 -*-
     或者
     # coding=utf-8
     ```

   - 【<span style="color: orange">**建议**</span>】为避免不同操作系统对文件换行处理的方式不同，一律使用`LF`

### 3. 每行最大长度

   - 【<span style="color: red">**必须**</span>】每行最多不超过`120`个字符。每行代码最大长度限制的根本原因是过长的行会导致阅读障碍，使得缩进失效。
   - 【<span style="color: blue">**自愿**</span>】以下两种情况可以超过120字符
     
     - 导入模块语句
     - 注释中包含了URL
   - 【<span style="color: orange">**建议**</span>】如果确实需要一个长字符串，可以用括号实现隐形连接。

     <span class="good_example_span" style="color:#228b22">***正确示范***</span>

     ```python
     x = ('This will build a very long long '
          'long long long long long long string')
     ```

   

### 4. 空格

   - 【<span style="color: red">**必须**</span>】在表达式的赋值符号、操作符左右至少有一个空格

     - <span class="good_example_span" style="color:#228b22">***正确示范***</span>

     ```python
     x = z + 1
     ```

     - <span class="bad_example_span" style="color:#dc143c">***错误示范***</span>

     ```python
     x=z+1  # 符号左右没有空格
     x=z +1
     x=z + 1
     x= z + 1
     ```

   

### 5. 注释

   - 【<span style="color: orange">**建议**</span>】注释应该清晰、简洁明了，方便其他人理解你的代码。
   - 【<span style="color: orange">**建议**</span>】注释应该在代码之前或之后，而不是在代码中间，这样不会影响代码的可读性。
   - 【<span style="color: blue">**自愿**</span>】在注释代码时，应该避免使用无意义的注释，例如`这是一个变量`或`这里进行了一个XX操作`。
   - 【<span style="color: orange">**建议**</span>】在编写函数时，应该在函数定义的下方使用`docstring`来提供函数的描述、参数和返回值等信息。
   - 【<span style="color: orange">**建议**</span>】对于长的注释，应该使用段落来组织注释，以便更容易阅读。
   - 【<span style="color: blue">**自愿**</span>】对于未完成开发代码、代办事项或需要进一步修改的代码，应当使用`TODO`来进行注释
   - 【<span style="color: blue">**自愿**</span>】对于需要即将修复或已经修复的bug的代码或错误，可以使用`FIX`或`FIXED`来进行注释

   一些注释示例

   ```python
   # 这是一行单行注释
   
   """
   这是一段多行注释
   可以用来解释一段较长的代码
   """
   
   result = 3 + 4  # 这里是一个加法操作
   
   def calculate_sum(a: int, b: int) -> int:
       """
       计算两个整数的和并返回结果
   
       Args:
           a: 整数a
           b: 整数b
   
       Returns:
           两个整数的和
       """
       return a + b
   
   # TODO: 这里需要增加异常处理代码
   def some_function():
       pass
   
   # TODO: 这里需要修改变量名
   my_variable = 10
   
   # TODO: 这里需要优化算法性能
   def some_algorithm():
       pass
     
   # FIX: 这里需要修复一个数组越界的错误
   def some_function():
       pass
   
   # FIXED: 这里已经修复了一个数组越界的错误
   def some_function():
       pass
   
   ```

   

### 6. 空行

   - 【<span style="color: red">**必须**</span>】在顶层函数或类定义之间应该有两个空行。这有助于将不同函数或类之间的代码清晰地分离开来。

     ```python
     def function1():
         # 函数1的代码
     
         
         
     def function2():
         # 函数2的代码
     ```

   - 【<span style="color: red">**必须**</span>】在函数或类定义内部，应该使用一个空行来分隔函数或类的不同部分。例如，在函数内部，可以使用一个空行来分隔函数头和函数主体，或者使用一个空行来分隔不同的逻辑段。

     ```python
     def my_function():
         # 函数头
         x = 1

         # 使用一个空行分隔函数头和函数主体
         if x == 1:

             # 使用一个空行分隔不同的逻辑段
             print("x is 1")

         else:
             print("x is not 1")

     ```

   - 【<span style="color: red">**必须**</span>】在模块的顶部和底部，应该使用一个空行来分隔模块的代码和其他模块或文件的代码。

     ```python
     # 模块的代码

     # 一个空行用来分隔模块的代码和其他模块或文件的代码

     ```

   - 【<span style="color: red">**必须**</span>】每个语句应该独占一行

     - <span class="good_example_span" style="color:#228b22">***正确示范***</span>

     ```python
     if foo:
         bar(foo)
     else:
         baz(foo)
     
     try:
         bar(foo)
     except ValueError:
         baz(foo)
     ```

     - <span class="bad_example_span" style="color:#dc143c">***错误示范***</span>

     ```python
     if foo: bar(foo)
     else:   baz(foo)
     
     try:               bar(foo)
     except ValueError: baz(foo)
     
     try:
       bar(foo)
     except ValueError: baz(foo)
     ```



 ### 7. 字符串

   - 【<span style="color: orange">**建议**</span>】使用`单引号`或`双引号`表示字符串，但在同一代码中应该保持一致

       - <span class="good_example_span" style="color:#228b22">***正确示范***</span>

       ```python
       name = "张三"
       sex = "男"
       ```
       - <span class="bad_example_span" style="color:#dc143c">***错误示范***</span>

       ```python
       name = "张三"
       sex = '男'
       ```

   - 【<span style="color: red">**必须**</span>】对于较长的字符串，可以使用字符串连接符 <kbd>+</kbd>将其拆成多行。请注意：字符串连接符应该在前面一行的末尾而不是下一行的开头。

     - <span class="good_example_span" style="color:#228b22">***正确示范***</span>

     ```python
     # 使用字符串连接符
     long_string = '这是一个较长的字符串，可以用字符串连接符' + \
                   '拆分成多行以提高可读性'
     ```

     - <span class="bad_example_span" style="color:#dc143c">***错误示范***</span>

     ```python
     # 使用字符串连接符
     long_string = '这是一个较长的字符串，可以用字符串连接符' \
                   + '拆分成多行以提高可读性'
     ```

   - 【<span style="color: red">**必须**</span>】对于需要包含引号或换行符等特殊字符的字符串，应该使用转义字符来表示这些字符。<br>Python中常见的转义字符包括` \n`（换行符）,`\t`（制表符）,` "`（双引号）和 `'`（单引号）等。

     ```python
     # 使用转义字符
     my_string = '这是一个包含\'引号\'的字符串，可以使用转义字符表示'
     my_string = '这是一个包含\\n的字符串，可以使用转义字符表示'
     my_string = '这是一个包含\\t的字符串，可以使用转义字符表示'
     ```

   - 【<span style="color: orange">**建议**</span>】关于格式化字符串。即使参数都是字符串，也需要使用`%`操作符或者格式化方法格式化字符串。<br>但是也不是定死，需要在 <kbd>+</kbd>和格式化之间做权衡。

       - <span class="good_example_span" style="color:#228b22">***正确示范***</span>

       ```python
       x = f'name: {name}; score: {n}'  # Python3.6+ 以上支持
       x = 'name: {name}; score: {n}'.format(name=name, n=n)
       x = 'name: {name}; score: {n}'.format(**{"name": name, "n": n})
       x = 'name: %(name)s; score: %(n)d' % {"name": name, "n": n}
       ```

       - <span class="bad_example_span" style="color:#dc143c">***错误示范***</span>

       ```python
       x = '%s%s' % (a, b)  # 这里应该使用 + 连接
       x = '{}{}'.format(a, b)  # 这里应该使用 + 连接
       x = imperative + ', ' + expletive + '!'  # 不建议这样，可读写太差，应该使用格式化方法
       x = 'name: ' + name + '; score: ' + str(n)  # 不建议这样，可读写太差，应该使用格式化方法
       ```

   - 【<span style="color: orange">**建议**</span>】在拼接字符串时，不推荐使用<kbd>+</kbd>运算符，而应该使用join()方法。join()方法更高效，并且在连接大量字符串时可以节省内存。

       - <span class="good_example_span" style="color:#228b22">***正确示范***</span>

       ```python
       foo = []
       name_list = ["张三", "李四", "王五"]
       for name in name_list:
           foo.append(name)
       
       print("".join(foo))
       ```

       - <span class="bad_example_span" style="color:#dc143c">***错误示范***</span>

       ```python
       foo = ""
       name_list = ["张三", "李四", "王五"]
       for name in name_list:
           foo += name
        
       print(foo)
       ```

   - 【<span style="color: orange">**建议**</span>】检查前缀后缀时，使用 `.startswith()` 和 `.endswith()` 函数检查，而不是使用切片 <kbd>[]</kbd>

     - <span class="good_example_span" style="color:#228b22">***正确示范***</span>

     ```python
     if foo.startswith('bar'):
     ```
     
     - <span class="bad_example_span" style="color:#dc143c">***错误示范***</span>
     
     ```python
     if foo[:3] == 'bar':
     ```

### 8. 括号

   - 【<span style="color: red">**必须**</span>】`Tuple`元组不允许逗号结尾，显式增加括号规避。即使一个元素也加上括号。行尾的逗号可能导致本来要定义一个简单变量，结果变成 tuple 变量。
     
     - <span class="good_example_span" style="color:#228b22">***正确示范***</span>
     
     ```python
     foo = (1,)  # 定义变量时，即使只有一个元素，也要带上括号
     
     return (1,)  # return时，即使只有一个元素，也要带上括号
     ```
     
     - <span class="bad_example_span" style="color:#dc143c">***错误示范***</span>
     
     ```python
     foo = 1,  # 定义变量时，结尾带上了逗号，导致本意是foo = 1 int类型 变成了foo = (1,) tuple, 给其他人读代码造成不便
     
     return 1,  # return时，结尾带上了逗号，很容易忽略这个逗号导致理解代码错误
     ```

### 9. 文档字符串

   > 文档字符串（docstring）是Python中一种用于文档化代码的约定，通常用于描述函数、类、模块等的功能和使用方法。<br>使用文档字符串可以使代码更具可读性、可维护性和可重用性。<br>[PEP 257](https://www.python.org/dev/peps/pep-0257/) 全面描述了文档字符串的风格。

   - 【<span style="color: orange">**建议**</span>】文档字符串应该在函数、类、模块的定义之后，使用三个双引号或三个单引号括起来。<br>文档字符串的第一行应该是一句简单的概述，可以用一句话描述该函数、类或模块的功能。

     ```python
     def my_function(arg1, arg2):
         """
         这是一个用于演示文档字符串的函数。
     
         Args:
             arg1: 第一个参数的描述
             arg2: 第二个参数的描述
     
         Returns:
             返回值的描述
         """
         return None
     
     ```

   - 【<span style="color: orange">**建议**</span>】在文档字符串中应该详细描述函数、类或模块的功能和使用方法。这些描述应该尽可能清晰、简洁，并包括必要的参数、返回值、异常等信息。

     ```python
     def my_function(arg1, arg2):
         """
         这是一个用于演示文档字符串的函数。
     
         :param arg1: 第一个参数的描述
         :type arg1: str
         :param arg2: 第二个参数的描述
         :type arg2: str
         :return: 返回值的描述
         :rtype: None
         :raises ValueError: 如果参数arg1不满足某些条件，则会引发ValueError。
     
         Example::
             示例用法：
             >>> my_function('hello', 'world')
             None
         """
         return None
     
     ```

### 10. 文件和Sockets

  - 【<span style="color: red">**必须**</span>】在Python中，打开文件和Socket后，必须显式地关闭它们，以释放资源和避免泄漏。<br>推荐使用`with`上下文管理器来操作文件或socket
  
       - <span class="good_example_span" style="color:#228b22">***正确示范***</span>
         
       ```python
       with open('file.txt', 'r') as f:
           # 文件操作
       
       with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
           # Socket 操作
       
       # 对于不支持使用with语句的类似文件的对象，可以使用contextlib.closing()
       import contextlib
       
       with contextlib.closing(urllib.urlopen("http://www.python.org/")) as front_page:
           for line in front_page:
               pass
       # Legacy AppEngine 中Python 2.5的代码如使用`with`语句, 需要添加 `from __future__ import with_statement`.
       
       ```
  
       - <span class="bad_example_span" style="color:#dc143c">***错误示范***</span>
         
       ```python
       f = open("file.txt")
       print(f.read())
       
       ```
  
  - 除文件外， sockets或其他类似文件的对象在没有必要的情况下打开，会造成很多副作用，包括但不限于以下几点：
    - 它们可能会消耗有限的系统资源，如文件描述符。 如果这些资源在使用后没有及时归还系统，那么用于处理这些对象的代码会将资源消耗殆尽。
    - 持有文件将会阻止对于文件的其他诸如移动、删除之类的操作。
    - 仅仅是从逻辑上关闭文件和sockets，那么它们仍然可能会被其共享的程序在无意中进行读或者写操作。只有当它们真正被关闭后，对于它们尝试进行读或者写操作将会抛出异常, 并使得问题快速显现出来。

 ### 11. Main

> 就算随便写一个脚本，也应该可以被别的模块导入且不会因为被导入造成脚本主函数被执行，除非特殊要求导入时执行
>
> 在Python中，`__name__`是一个内置的特殊变量，用于表示当前模块的名称。<br>当一个Python文件被执行时，`__name__`会被设置为`'__main__'`。<br>因此，可以使用`__name__ == '__main__'`来判断当前文件是否被作为脚本直接执行。

- 【<span style="color: orange">**建议**</span>】python中的所有文件都应该可以被导入，对不需要作为程序入口地方应该添加 `if __name__ == '__main__' `

    - <span class="good_example_span" style="color:#228b22">***正确示范***</span>

    ```python
    # my_module.py
    
    def add(a, b):
        return a + b
    
    def substract(a, b):
        return a - b
    
    if __name__ == '__main__':
        print(add(1, 2))
        print(substract(3, 4))
    
    ```

    - <span class="bad_example_span" style="color:#dc143c">***错误示范***</span>

    ```python
    # my_module.py 改脚本会在被别的模块导入时直接被执行
    
    def add(a, b):
        return a + b
    
    def substract(a, b):
        return a - b
    
    print(add(1, 2))
    print(substract(3, 4))
    ```


​      

### 12. SheBang

> 指的是 Unix系统中文件头的 `#!` 
>
> 这个符号的名称，叫做`Shebang`或者`Sha-bang`。长期以来，Shebang都没有正式的中文名称。Linux中国翻译组的GOLinux将其翻译为“释伴”，即“解释伴随行”的简称，同时又是Shebang的音译

- 【<span style="color: orange">**建议**</span>】程序的main文件应该以 `#!/usr/bin/env python2` 或者 `#!/usr/bin/env python3` 开始， 可以同时支持Python2、Python3的`#!/usr/bin/env python`<br>非程序入口的文件不应该出现Shebang

    - <span class="good_example_span" style="color:#228b22">***正确示范***</span>

    ```python
    #!/usr/bin/env python3
    # -*- coding: utf-8 -*-
    
    ```


### 13. 导包

- 【<span style="color: red">**必须**</span>】使用import语句导入包或模块时，位于模块注释和文档字符串之后，模块全局变量和常量之前，并且每个导入应该独占一行

- 【<span style="color: orange">**建议**</span>】导入应该按照从最通用到最不通用的顺序分组, 每个分组之间，需要空一行

    * 标准库导入
    * 第三方库导入
    * 本地导入

- 【<span style="color: red">**必须**</span>】避免使用通配符导入，如`from module import *`，因为它使得代码难以理解和维护，并且可能会污染命名空间

    > 污染命名空间（Namespace Pollution）指的是在程序中定义了过多的变量，从而导致命名空间变得混乱不清，变量名之间发生冲突或者变量被意外地覆盖等问题。这些问题可能会导致程序出现错误或者产生不可预测的行为。因此，在编写Python代码时，我们应该尽量避免污染命名空间，使用合适的变量名和作用域规则来确保程序的可读性、可维护性和正确性。

- 【<span style="color: red">**必须**</span>】避免导入了模块却不使用它

- 【<span style="color: blue">**自愿**</span>】对于常用的第三方库，可以将其导入并重命名为一个简短的名称，以节省代码中的空间和打字时间，例如：`import numpy as np`。

- 【<span style="color: red">**必须**</span>】如果只需要导入模块中的某个函数或变量，则可以使用`from module import function`语句，而不是导入整个模块。

- 【<span style="color: red">**必须**</span>】对于模块和包中的子模块，可以使用`import module.submodule`语句来导入它们，而不是使用多个导入语句。

- 【<span style="color: red">**必须**</span>】在Python中，导入包时可以使用相对导入或绝对导入。建议使用绝对导入，因为它更具可读性和稳定性，且在Python 3中是默认的导入方式。

    示例

    ```python
    import os
    import numpy as np
    from my_module import my_function
    from my_package import my_submodule
    
    def main():
        # 使用导入的库和模块
        pass
    
    if __name__ == "__main__":
        main()
    
    ```

### 14. 类型注解

> Python类型注解是Python3.5引入的一种语法，它允许在函数或方法的参数和返回值上标注类型信息。<br>
> 类型注解是一种静态检查类型的方式，可以提高代码的可读性和可维护性<br>
> 并且可以帮助IDE或其他工具进行自动补全和类型检查等操作。<br>
> 在使用Python3.5+时，推荐在项目中使用该特性<br>
> 更多使用可以参考 [类型标注支持]( https://docs.python.org/zh-cn/3/library/typing.html)。

- 【<span style="color: red">**必须**</span>】Python类型注解使用的语法是在函数或方法的参数或返回值后面加上`:`和类型名称

    - <span class="good_example_span" style="color:#228b22">***正确示范***</span>

    ```python
    def greeting(name: str) -> str:
        return "Hello, " + name
    # 上面的代码中，greeting函数的参数name被标注为str类型，返回值也被标注为str类型。这些类型注解并不会影响函数的实际行为，仅仅是提供了额外的类型信息。当使用该函数时，可以根据类型注解来推断参数和返回值的类型，例如：
    
    print(greeting("Alice"))  # 输出：Hello, Alice
    print(greeting(123))  # 抛出类型错误 TypeError: can only concatenate str (not "int") to str
    # 第一个调用使用了一个字符串作为参数调用了greeting函数，因此没有发生任何错误。
    # 而第二个调用使用了一个整数作为参数调用了greeting函数，因为类型不匹配，所以抛出了一个TypeError类型的错误。
    ```

    - <span class="bad_example_span" style="color:#dc143c">***错误示范***</span>

    ```python
    def greeting(name : str) -> str:  # 冒号前面不应有空格
        return "Hello, " + name
    ```

- 【<span style="color: red">**必须**</span>】模块级变量，类和实例变量以及局部变量的注释应在冒号后面有一个空格

-  【<span style="color: red">**必须**</span>】如果有赋值符，则等号在两边应恰好有一个空格

    - <span class="good_example_span" style="color:#228b22">***正确示范***</span>

    ```python
    code: int = 10
    
    class Point(object):
        coords: Tuple[int, int] = (0, 0)
        label: str = '<unknown>'
    
    def broadcast(servers: Sequence[Server],
                    message: str = 'spaces around equality sign') -> None:
        pass
    ```

    - <span class="bad_example_span" style="color:#dc143c">***错误示范***</span>

    ```python
    code:int  # 冒号后没空格
    code : int  # 冒号前多了个空格
    
    class Test(object):
        result: int=0  # 等号左右没有空格
    ```

      

## 二、编码规范

### 1. `None`条件判断

> ​	在Python中，`None`是一个特殊的对象，用于表示“空”或“不存在”。

- 【<span style="color: red">**必须**</span>】使用`is`操作符来判断变量或表达式是否为`None`，而不是使用`==`操作符。

    - <span class="good_example_span" style="color:#228b22">***正确示范***</span>

    ```python
    if foo is None:
    ```

    - <span class="bad_example_span" style="color:#dc143c">***错误示范***</span>

    ```python
    if foo == None:
    ```


- 【<span style="color: red">**必须**</span>】为提升可读性，在判断条件中应使用 `is not`，而不使用 `not ... is`

    - <span class="good_example_span" style="color:#228b22">***正确示范***</span>

    ```python
    if foo is not None:
    ```

    - <span class="bad_example_span" style="color:#dc143c">***错误示范***</span>

    ```python
    if not foo is None:
    ```

    ​      

### 2. `lambda`匿名函数

> 建议：项目中使用 def 定义简短函数而不是使用 lambda，因为使用def的方式有助于在trackbacks中打印有效的信息

- 【<span style="color: red">**必须**</span>】不要在lambda函数中使用复杂的表达式或语句，以保持代码的简洁和易于理解。

    - <span class="good_example_span" style="color:#228b22">***正确示范***</span>

    ```python
    # 对列表进行排序，使用lambda函数定义排序规则
    my_list = [(1, "a"), (4, "d"), (3, "c"), (2, "b")]
    my_list.sort(key=lambda x: x[0])
    print(my_list)
    ```

    - <span class="bad_example_span" style="color:#dc143c">***错误示范***</span>

    ```python
    # 不推荐在lambda函数中使用复杂的语句和表达式
    my_list = [1, 2, 3, 4, 5, 6]
    new_list = list(map(lambda x: x ** 2 if x % 2 == 0 else x, my_list))
    print(new_list)
    ```

- 【<span style="color: red">**必须**</span>】不要在lambda函数中使用循环语句或其他控制流语句，因为这可能会使代码更加难以理解。

    - <span class="good_example_span" style="color:#228b22">***正确示范***</span>

    ```python
    # 对列表进行筛选，使用lambda函数定义筛选规则
    my_list = [1, 2, 3, 4, 5, 6]
    new_list = list(filter(lambda x: x % 2 == 0, my_list))
    print(new_list)
    ```

    - <span class="bad_example_span" style="color:#dc143c">***错误示范***</span>

    ```python
    # 不推荐在lambda函数中使用循环语句
    my_list = [[1, 2], [3, 4], [5, 6]]
    new_list = list(map(lambda x: [i ** 2 for i in x], my_list))
    print(new_list)
    ```

### 3. 列表推导式

> 列表推导式的语法格式为`[expression for item in iterable if condition]`，其中expression为需要执行的表达式，item为可迭代对象中的元素，iterable为可迭代对象，condition为一个可选的判断条件。

- 【<span style="color: red">**必须**</span>】在使用列表推导式时，应该尽量避免使用嵌套的列表推导式，因为嵌套的推导式可能会使代码难以理解和维护。如果超过一个for语句或过滤表达式，应当使用传统for循环替代而不是使用列表推导式

    - <span class="good_example_span" style="color:#228b22">***正确示范***</span>

    ```python
    number_list = [1, 2, 3, 10, 20, 55]
    odd = [i for i in number_list if i % 2 == 1]
    
    result = []
    for x in range(10):
        for y in range(5):
            if x * y > 10:
                result.append((x, y))
    ```

    - <span class="bad_example_span" style="color:#dc143c">***错误示范***</span>

    ```python
    result = [(x, y) for x in range(10) for y in range(5) if x * y > 10]  # 多重for导致代码理解困难
    ```

- 【<span style="color: red">**必须**</span>】列表推导式适用于简单场景。 如果语句过长，每个部分应该单独置于一行： 映射表达式， for语句， 过滤器表达式

    - <span class="good_example_span" style="color:#228b22">***正确示范***</span>

    ```python
    fizzbuzz = []
    for n in range(100):
        if n % 3 == 0 and n % 5 == 0:
            fizzbuzz.append(f'fizzbuzz {n}')
        elif n % 3 == 0:
            fizzbuzz.append(f'fizz {n}')
        elif n % 5 == 0:
            fizzbuzz.append(f'buzz {n}')
        else:
            fizzbuzz.append(n)
    
    
    for n in range(1, 11):
        print(n)
    ```

    - <span class="bad_example_span" style="color:#dc143c">***错误示范***</span>

    ```python
    # 条件过于复杂
    fizzbuzz = [
        f'fizzbuzz {n}' if n % 3 == 0 and n % 5 == 0
        else f'fizz {n}' if n % 3 == 0
        else f'buzz {n}' if n % 5 == 0
        else n
        for n in range(100)
    ]
    
    
    [print(n) for n in range(1, 11)] 
    ```

     

### 4. 函数

- 【<span style="color: red">**必须**</span>】定义重复函数声明，会导致覆盖较早定义的函数

    - <span class="good_example_span" style="color:#228b22">***正确示范***</span>

    ```python
    def get_x(x):
        return x
    
    def get_y(y):
        return y
    ```

    - <span class="bad_example_span" style="color:#dc143c">***错误示范***</span>

    ```python
    def get_x(x):
        return x
    
    def get_x(x):  # 模块内重复定义
        return x
    ```

- 【<span style="color: orange">**建议**</span>】函数名应该清晰、简短、准确，并且要符合Python标识符的命名规范

- 【<span style="color: red">**必须**</span>】函数的参数和返回值应该尽可能的清晰和简洁，并且要避免使用全局变量。

- 【<span style="color: red">**必须**</span>】函数应该尽量避免修改函数入参，否则会对代码理解造成人为的阻碍，可能会误导其他开发人员

    - <span class="good_example_span" style="color:#228b22">***正确示范***</span>

    ```python
    # 定义一个函数，用于对列表中的元素进行平方计算，并返回新的列表
    def square_list(lst):
        """
        对列表中的元素进行平方计算，并返回新的列表
        :param lst: 原始列表
        :return: 新的列表
        """
        return [x**2 for x in lst]
    ```

    - <span class="bad_example_span" style="color:#dc143c">***错误示范***</span>

    ```python
    # 函数中对传入的列表进行修改，不应该对传入的参数进行修改
    def square_list(lst):
        for i in range(len(lst)):
            lst[i] = lst[i] ** 2
    ```

-  【<span style="color: red">**必须**</span>】函数参数中，禁止使用可变类型变量作为默认值

    - <span class="good_example_span" style="color:#228b22">***正确示范***</span>

    ```python
    def f(x=0, y=None, z=None):
        if y is None:
            y = []
        if z is None:
            z = {}
    ```

    - <span class="bad_example_span" style="color:#dc143c">***错误示范***</span>

    ```python
    def f(x=0, y=[], z={}):
        pass
    
    def f(a, b=time.time()):
        pass
    ```

    
    

### 5. 类

- 【<span style="color: red">**必须**</span>】类的命名应该遵循大驼峰命名法（每个单词的首字母大写），以便与变量和函数等其他命名区分开来。

- 【<span style="color: red">**必须**</span>】属性和方法的命名应该遵循小写字母和下划线的命名法，以便于阅读和理解。

- 【<span style="color: orange">**建议**</span>】Python没有私有属性和方法的概念，但是可以使用单下划线来表示属性和方法是私有的。

  - <span class="good_example_span" style="color:#228b22">***正确示范***</span>

  ```python
  class MyClassName:
      def __init__(self, arg1, arg2):
          self._arg1 = arg1
          self._arg2 = arg2
  
      def _my_private_method(self):
          pass
  ```

 - 【<span style="color: orange">**建议**</span>】类方法的第一个参数名使用`cls`而不是其他名字


 - 【<span style="color: orange">**建议**</span>】实例方法的第一个参数名使用`self`而不是其他名字
 - 【<span style="color: blue">**自愿**</span>】在类中定义的成员函数，如果未使用`self`中的属性，应当加上`@staticmethod`以表示为静态方法

### 6. 异常

- 【<span style="color: red">**必须**</span>】异常类继承自 `Exception`，而不是 `BaseException`

- 【<span style="color: orange">**建议**</span>】捕获异常时，需要指明具体异常，而不是捕获所有异常

  - <span class="good_example_span" style="color:#228b22">***正确示范***</span>

  ```python
  try:
      with open('myfile.txt', 'r') as f:
          content = f.read()
          # do some processing on the content
  except (FileNotFoundError, IOError) as e:
      print(f'Error opening or reading file: {e}')
  except ValueError as e:
      print(f'Error processing file contents: {e}')
  else:
      print(content)
  
  ```

- <span class="bad_example_span" style="color:#dc143c">***错误示范***</span>

    ```python
    try:
        # some code that may raise an exception
    except Exception:
        # handle the exception
        print('An error occurred.')
    
    ```


- 【<span style="color: orange">**建议**</span>】在代码中用异常替代函数的错误返回码。

    - <span class="good_example_span" style="color:#228b22">***正确示范***</span>

    ```python
    def write_data():
        if check_file_exist():
            do_something()
        else:
            raise FileNotExist()
    ```

    - <span class="bad_example_span" style="color:#dc143c">***错误示范***</span>

    ```python
    def write_data():
        if check_file_exist():
            do_something()
            return 0
        else:
            return FILE_NOT_EXIST
    ```



- 【<span style="color: red">**必须**</span>】如需在 `except` 子句中重新抛出原有异常时，不能用 `raise ex`，而是用 `raise`。

    > `raise ex` 会创建一个新的异常对象，其中 `ex` 是原有异常的引用。<br>新的异常对象会丢失原有异常的堆栈跟踪信息，并且可能会导致调试问题。

    - <span class="good_example_span" style="color:#228b22">***正确示范***</span>

    ```python
    try:
        raise MyException()
    except MyException as ex:
        try_handle_exception()
        raise  # 可以保留原始的 traceback
    ```

    - <span class="bad_example_span" style="color:#dc143c">***错误示范***</span>

    ```python
    try:
        raise MyException()
    except MyException as ex:
        try_handle_exception()
        raise ex  
    ```



- 【<span style="color: orange">**建议**</span>】所有 `try`/`except` 子句的代码要尽可的少，以免屏蔽其他的错误


  - <span class="good_example_span" style="color:#228b22">***正确示范***</span>

  ```python
  try:
      value = collection[key]
  except KeyError:
      return key_not_found(key)
  else:
      return handle_value(value)
  ```

  - <span class="bad_example_span" style="color:#dc143c">***错误示范***</span>

  ```python
  try:
      # 范围太广
      return handle_value(collection[key])
  except KeyError:
      # 会捕捉到 handle_value() 中的 KeyError
      return key_not_found(key)
  ```

- 【<span style="color: red">**必须**</span>】记录错误日志时，使用`logger.exception` 而不是`logger.error`

  > logger.error 无法打印错误堆栈的跟踪信息<br>logger.exception会自动记录错误堆栈的跟踪信息

  - <span class="good_example_span" style="color:#228b22">***正确示范***</span>

  ```python
  try:
      ret = my_funtion()
  except Exception as e:
      logger.exception(e)
  
  # 或者 使用traceback记录
  
  try:
      ret = my_funtion()
  except Exception as e:
      logger.error(traceback.format_exc()) 
  
  ```

  - <span class="bad_example_span" style="color:#dc143c">***错误示范***</span>

  ```python
  try:
      ret = my_funtion()
  except Exception as e:
      logger.error(e)
  
  ```

### 7. 条件表达式

- 【<span style="color: blue">**自愿**</span>】函数或者方法在没有返回时要显示的返回`None`，而不是让Python隐式的返回

-  【<span style="color: orange">**建议**</span>】对于未知的条件分支或者不应该进入的分支，建议抛出异常，而不是返回一个值（比如说`None` 或`False`）

  - <span class="good_example_span" style="color:#228b22">***正确示范***</span>

    ```python
    def f(x):
        if x in ('SUCCESS',):
            return True
        else:
            raise MyException()  # 如果一定不会走到的条件，可以增加异常，防止将来未知的语句执行。
    ```

  - <span class="bad_example_span" style="color:#dc143c">***错误示范***</span>

    ```python
    def f(x):
        if x in ('SUCCESS',):
            return True
        return None
    ```

- 【<span style="color: orange">**建议**</span>】`if` 与 `else` 尽量一起出现，而不是全部都是`if`子句。

  > 由于Python全靠缩进代码块来判断代码作用域，当多个if连在一起的时候，不能直观的判断是否是手误elif还是真的if

  - <span class="good_example_span" style="color:#228b22">***正确示范***</span>

  ```python
  if condition:
      do_something()
  else:
      # 增加说明注释
      pass
  
  if condition:
      do_something()
  # 这里增加注释说明为什么不用写else子句
  # else:
  #     pass
  ```

  - <span class="bad_example_span" style="color:#dc143c">***错误示范***</span>

  ```python
  if condition:
      do_something()
  if another_condition: 
      do_another_something()
  else:
      do_else_something()
  ```

  

### 8. `*args`和`**kwargs`

 - 【<span style="color: orange">**建议**</span>】通常情况下，我们将可变数量的位置参数命名为`*args`，<br>将可变数量的关键字参数命名为`**kwargs `而不是其他名字如 *a, *k
 - 【<span style="color: red">**必须**</span>】对于只接受固定数量参数的函数，不应该使用 `*args`。
 - 【<span style="color: orange">**建议**</span>】应该为 `*args`和`**kwargs`提供一个描述性的参数名称，以便在阅读代码时能够更好地理解函数的目的。
 - 【<span style="color: red">**必须**</span>】使用 `**kwargs` 时，应该把它放在函数参数列表的最后一个参数位置。
 - 【<span style="color: blue">**自愿**</span>】代码中尽量少使用或不使用 `*args`和`**kwargs`，如非使用，请注意写清楚数据结构

### 9. 闭包

> 闭包是指在一个函数内部定义了另一个函数，并且内部函数可以引用外部函数的变量或参数，即使外部函数已经执行完毕，内部函数仍然可以访问外部函数中的变量或参数。这样的内部函数就形成了一个闭包。也称之为`装饰器`，可以配合 <kbd>@</kbd>符号使用

- 【<span style="color: orange">**建议**</span>】在定义闭包函数时，应该确保该函数具有清晰的目的和明确的输入输出。

- 【<span style="color: red">**必须**</span>】除非你自己很清楚你在干什么，否则不要使用闭包。可能会造成内存泄露

- 【<span style="color: red">**必须**</span>】在定义闭包函数时，应该使用 `nonlocal` 关键字声明所有需要在闭包函数内部修改的变量。

    - <span class="good_example_span" style="color:#228b22">***正确示范***</span>

    ```python
    def outer():
        x = 0
        def inner():
            nonlocal x
            x += 1
            return x
        return inner
    
    func = outer()
    print(func())  # 1
    print(func())  # 2
    
    ```

    - <span class="bad_example_span" style="color:#dc143c">***错误示范***</span>

    ```python
    def outer():
        x = 0
        def inner():
            x += 1
            return x
        return inner
    
    func = outer()
    print(func())  # UnboundLocalError: local variable 'x' referenced before assignment
    
    ```

     

 ### 10. 逻辑运算符

> 逻辑运算符有三个：`and`、`or` 和 `not`

- 【<span style="color: red">**必须**</span>】当有多个逻辑运算符同时出现时，使用括号显示得声明运算优先级，避免代码理解困难

    - <span class="good_example_span" style="color:#228b22">***正确示范***</span>

    ```python
    (a and b) or c 和  a and (b or c)  # 结果不同
    ```


- 【<span style="color: orange">**建议**</span>】在使用 `and` 运算符时，应该将最有可能为 `False` 的表达式放在前面，这样可以最大程度地避免计算后面的表达式。



- 【<span style="color: orange">**建议**</span>】在使用逻辑运算符时，应该尽可能地简化表达式，避免出现不必要的复杂性。

    - <span class="good_example_span" style="color:#228b22">***正确示范***</span>

    ```python
    if x is not None and x != 0:
        # do something
    
    if x > 0 or y > 0:
        # do something
    
    if x in my_list or y in my_list:
        # do something
    ```

    - <span class="bad_example_span" style="color:#dc143c">***错误示范***</span>

    ```python
    if (x is not None) and (x != 0):
        # 不需要使用括号
    
    if not (x == 0):
        # 应该使用 x != 0
    
    if x or y:
        # 不够明确，应该使用 x != None and x != 0 or y != None and y != 0
    
    ```


### 11. 循环

  - 【<span style="color: red">**必须**</span>】不要在循环内部重复执行相同的计算或操作，应该尽可能地提前计算或缓存结果。

    - <span class="good_example_span" style="color:#228b22">***正确示范***</span>

    ```python
    # 正确示例：在循环外计算列表长度，避免重复计算
    my_list = [1, 2, 3, 4, 5]
    length = len(my_list)
    for i in range(length):
        print(my_list[i])
    ```

    - <span class="bad_example_span" style="color:#dc143c">***错误示范***</span>

    ```python
    # 错误示例：在循环内重复调用 len() 函数
    my_list = [1, 2, 3, 4, 5]
    for i in range(len(my_list)):
        print(my_list[i])
    ```

  - 【<span style="color: red">**必须**</span>】不要在循环内部频繁地打开和关闭文件或数据库连接等资源，应该尽可能地复用已经打开的资源。

    - <span class="good_example_span" style="color:#228b22">***正确示范***</span>

    ```python
    # 正确示例：在循环外打开文件，避免频繁打开和关闭
    with open('data.txt', 'w') as f:
        for i in range(10):
            f.write(str(i))
    ```

    - <span class="bad_example_span" style="color:#dc143c">***错误示范***</span>

    ```python
    # 错误示例：在循环内部频繁地打开和关闭文件
    for i in range(10):
        with open('data.txt', 'w') as f:
            f.write(str(i))
    ```

  - 【<span style="color: red">**必须**</span>】不要在循环内部做过多的 I/O 操作，应该尽可能地减少 I/O 操作的次数和数据量。

    - <span class="good_example_span" style="color:#228b22">***正确示范***</span>

    ```python
    # 正确示例：在循环外读写文件，减少 I/O 操作的次数
    with open('data.txt', 'w') as f:
        data = ''
        for i in range(10):
            data += str(i)
        f.write(data)
    ```

    - <span class="bad_example_span" style="color:#dc143c">***错误示范***</span>

    ```python
    # 错误示例：在循环内部频繁地读写文件
    with open('data.txt', 'w') as f:
        for i in range(10):
            f.write(str(i))
            f.flush()  # 每次写完后手动刷新缓冲区
    ```

  - 【<span style="color: orange">**建议**</span>】循环变量的命名应该具有描述性，能够清晰地表达循环变量的作用。

    - <span class="good_example_span" style="color:#228b22">***正确示范***</span>

    ```python
    name_list = ["张三", "李四", "王五"]
    for name in name_list:
        pass
    ```

    - <span class="bad_example_span" style="color:#dc143c">***错误示范***</span>

    ```python
    name_list = ["张三", "李四", "王五"]
    for i in name_list:
        pass
    ```

  - 【<span style="color: red">**必须**</span>】不要在循环中修改循环变量，可能会导致不可预期的结果，并且当出现之后通常很难排查

    - <span class="good_example_span" style="color:#228b22">***正确示范***</span>

    ```python
    name_list = ["张三", "李四", "王五"]
    for i in name_list[:]:
        name_list.remove(i)
    print(name_list)  # []
    ```

    - <span class="bad_example_span" style="color:#dc143c">***错误示范***</span>

    ```python
    name_list = ["张三", "李四", "王五"]
    for i in name_list:  
        name_list.remove(i)
    print(name_list)  # ['李四']  
    
    
    # 循环中删除字典中的元素会导致程序报错
    name_dict = {
        "name": "张三",
        "age": 18,
        "height": 180,
    }
    for i in name_dict:  # RuntimeError: dictionary changed size during iteration
        name_dict.pop(i)  
    print(name_dict)
    
    ```

  - 【<span style="color: orange">**建议**</span>】如果需要遍历一个序列并同时跟踪序列中元素的索引，可以使用 `enumerate()` 函数。

    - <span class="good_example_span" style="color:#228b22">***正确示范***</span>

    ```python
    # 使用 enumerate() 函数遍历序列并跟踪索引
    my_list = ['a', 'b', 'c']
    for index, item in enumerate(my_list):
        print(index, item)
    
    ```

  - 【<span style="color: orange">**建议**</span>】如果需要反向遍历一个序列，可以使用 `reversed()` 函数。

    - <span class="good_example_span" style="color:#228b22">***正确示范***</span>

    ```python
    # 使用 reversed() 函数反向遍历序列
    my_list = ['a', 'b', 'c']
    for item in reversed(my_list):
        print(item)
    ```

  - 【<span style="color: red">**必须**</span>】使用while循环时，确保循环条件在某个时刻会变为 False，否则会导致死循环。（除非有特定需求）
  - 【<span style="color: orange">**建议**</span>】避免在 while 循环中使用复杂的条件表达式，应该将它们封装为函数或变量。

### 12. 可变和不可变

  - 【<span style="color: orange">**建议**</span>】如果变量明确知道不会被修改，应当使用不可变类型。如：`tulpe`、`str`
  - 【<span style="color: red">**必须**</span>】在使用可变数据类型时，要注意该变量是否只有自己用，否则不要轻易修改变量中的数据
  - 【<span style="color: red">**必须**</span>】在多线程程序中，修改任何变量都应该使用锁来确保并发安全

 ### 13.变量

  - 【<span style="color: red">**必须**</span>】禁止定义了变量却不使用它

    > 在代码里到处定义变量却没有使用它，不完整的代码结构看起来像是个代码错误。 即使没有使用，但是定义变量仍然需要消耗资源，并且对阅读代码的人也会造成困惑，不知道这些变量是要做什么的。

    - <span class="good_example_span" style="color:#228b22">***正确示范***</span>

    ```python
    def get_x_plus_y(x, y):
        return x + y
    ```

    - <span class="bad_example_span" style="color:#dc143c">***错误示范***</span>

    ```python
    some_unused_var = 42
    
    # 定义了变量不意味着就是使用了
    y = 10
    y = 5
    
    # 对自身的操作并不意味着使用了
    z = 0
    z = z + 1
    
    # 未使用的函数参数
    def get_x(x, y):
        return x
    ```

  - 【<span style="color: blue">**自愿**</span>】当某函数返回多个值而你又不需要用到那么多的时候，建议使用双下划线 `__`来表示不需要使用的变量

    - <span class="good_example_span" style="color:#228b22">***正确示范***</span>

    ```python
    path = '/tmp/python/foobar.txt'
    dir_name, __ = os.path.split(path)
    ```

### 14. 内置关键字
> 常见内置关键字：
> ```python
> and       as        assert    async     await
> break     class     continue  def       del
> elif      else      except    False     finally
> for       from      global    if        import
> in        is        lambda    None      nonlocal
> not       or        pass      raise     return
> True      try       while     with      yield
> ```

- 【<span style="color: red">**必须**</span>】不要使用内置关键字作为变量名、函数名、类名等标识符，以避免出现语法错误或者意外的行为。
- 【<span style="color: red">**必须**</span>】如果需要使用与内置关键字同名的标识符，可以在标识符前面添加一个下划线，以区分开来。

### 15. eval和exec

> `eval()` 和 `exec()` 是 Python 内置函数，可以用于在程序运行时执行字符串表示的 Python 代码。<br>但是，它们也带来了一些安全风险，因为它们可以执行任意的 Python 代码，包括恶意代码。

- 【<span style="color: red">**必须**</span>】禁止将用户的输入传入`eval`或`exec`，这可能造成非常严重的后果

  - <span class="bad_example_span" style="color:#dc143c">***错误示范***</span>

    ```python
    s = input('please input:')
    print eval(s)
    
    # 用户输入 __import__('os').system('del delete.py /q')  # 通过这些字符串，用户可以执行任意他想执行的命令
    
    
    def my_view_funtion(request):
        task_list = eval(request.POST.get('task_list'))  # 非常危险，禁止这样做
        ...
    ```

- 【<span style="color: red">**必须**</span>】所有项目中应该严禁使用`eval`或`exec`

## 三、工具和配置

### 1.flake8

[`flake8`](https://flake8.pycqa.org/) 是一个结合了 pycodestyle，pyflakes，mccabe 检查 Python 代码规范的工具。


使用方法：

```bash
flake8 {source_file_or_directory}
```

在项目中创建 `setup.cfg` 或者 `tox.ini` 或者 `.flake8` 文件，添加 `[flake8]` 部分。

推荐的配置文件如下：

```ini
[flake8]
ignore =
    ;W503 line break before binary operator
    W503,
    ;E203 whitespace before ':'
    E203,

; exclude file
exclude =
    .tox,
    .git,
    __pycache__,
    build,
    dist,
    *.pyc,
    *.egg-info,
    .cache,
    .eggs

max-line-length = 120
```

如果需要屏蔽告警可以增加行内注释 `# noqa`，例如：

```python
example = lambda: 'example'  # noqa: E731
```

### 2.pylint

[`pylint`](https://www.pylint.org/) 是一个能够检查编码质量、编码规范的工具。

配置项较多，单独一个配置文件配置，详情可查阅：[.pylintrc](https://git.code.oa.com/standards/python/blob/master/.pylintrc)

使用方法：

```bash
pylint {source_file_or_directory}
```

如果遇到一些实际情况与代码冲突的，可以在行内禁用相关检查，例如 ：

```python
try:
    do_something()
except Exception as ex:  # pylint: disable=broad-except
    pass
```

如果需要对多行的进行禁用规则，可以配套使用 `# pylint: disable=具体错误码`/`# pylint: enable=具体错误码`。

```python
# pylint: disable=invalid-name
这里的代码块会被忽略相关的告警
app = Flask(__name__)
# pylint: enable=invalid-name
```

### 3.EditorConfig

EditorConfig 可以帮助开发同一项目下的跨多 IDE 的开发人员保持一致编码风格。

在项目的根目录下放置
[`.editorconfig`](https://git.code.oa.com/standards/python/blob/master/.editorconfig)
文件，可以让编辑器规范文件对格式。参考配置如下：

```ini
# https://editorconfig.org

root = true

[*]
indent_style = space
indent_size = 4
trim_trailing_whitespace = true
insert_final_newline = true
charset = utf-8
end_of_line = lf

[*.py]
max_line_length = 120

[*.bat]
indent_style = tab
end_of_line = crlf

[LICENSE]
insert_final_newline = false

[Makefile]
indent_style = tab
```

支持常见的 IDE ，配置说明及 IDE 的支持情况可参考官网 [editorconfig.org](https://editorconfig.org/)。


