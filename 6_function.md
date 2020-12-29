1. ```python 
    def 函数名:
        ...
   ```
       
2. 传递实参
   - (1) 位置实参：实参顺序与形参顺序相同
   - (2) 关键字实参：传递给函数的键-值对
   - (3) 形参使用默认参数：**注：在形参列表中，先列出没有默认值的形参，再列出有默认值的形参。**
   - (4) **混合使用**位置实参、关键字实参和形参使用默认值
   
3. 返回值-- **函数可以返回任何类型的值**
   - 形参使用默认值（空字符串""）,使实参变成可选的
    ```python
    def get_formatted_name(first_name, last_name, middle_name=''):
    """返回整洁的姓名"""
    if middle_name:
        full_name = first_name + ' ' + middle_name + " " + last_name
    else:
        full_name = first_name + " " + last_name
    return full_name.title()

    musician = get_formatted_name("jimi", "hendrix")
    print(musician)

    musician = get_formatted_name("john", 'hooker', 'lee')
    print(musician)
    ```

4. 形参传递列表
**注：列表传递给函数后，在函数中对该列表所做的任何修改都是永久性的**）
   - (1) 每个函数只负责一项具体工作
   - (2) 描述性的函数名
   - (3) 有时需要进制函数修改列表，解决：**向函数传递列表的副本[:]而不是原列表**
   ```python
    def show_magicians(magicians):
        """打印列表中的元素"""
        for magician in magicians:
            print(magician)

    def make_great(magicians):
        """
        修改列表中的每个元素
        注：在for循环中不该修改列表，否则python无法追踪其中元素；
            遍历列表的同时修改列表，用while循环
        """
        """
        # 方法二：while循环
        i = 0
        while i < len(magicians):
            magicians[i] = "the Great " + magicians[i]
            i += 1
        """
        # 方法一：for循环
        # 根据索引替换列表中的元素
        for i in range(len(magicians)):
            magicians[i] = "the great " + magicians[i]
        return magicians

    # 8_9:函数传递原列表
    magicians = ["a", 'b', 'v']
    print("原列表：")
    show_magicians(magicians)

    # 用原列表调用make_great函数
    make_great(magicians)

    print("\n传递原列表后，修改后的列表：")
    show_magicians(magicians)

    # 8_10:函数传递原列表副本
    magicians = ["a", 'b', 'v']
    # 用原列表副本[:]调用make_great函数
    changed_magicians = make_great(magicians[:])

    print("\n传递原列表副本后,原列表：")
    show_magicians(magicians)

    print("\n传递原列表副本后，修改的列表")
    show_magicians(changed_magicians)
   ```

5. 传递任意数量实参
   - (1) 传递任意数量同类型实参
   ```python
   def 函数名(*元组名)：
   ```  
   形参名"*元组名"中的星号，让python传建一个名为"元组名"的空元组，并将收到的所有值都装到元组中。  
   ```python
    """任意数量同类型实参尝试"""
    def add_toppings(*toppings):
        """概述添加的配料"""
        print("Add the following toppings:")
        for topping in toppings:
            print(topping)

    add_toppings('san', 'wi', 'ch')
    add_toppings('er')
    add_toppings('syi', '-wi')
   ```
   - (2) 结合使用位置实参和任意数量参数  
   **注：python先匹配位置实参和关键字实参，再将余下的实参都收集到最后一个参数中**
   - (3) 任意数量关键字实参--**（预先不知函数收到什么info时）**
   ```python
   def 函数名(**字典名):
   ```  
   形参名"**字典名"中的两个星号，让python传建一个名为"字典名"的空字典，并将收到的所有名称-值对都装到字典中。  
   ```python
    """位置参数和任意数量键-值对参数尝试"""
    def build_profile(first, last,  **user_info):
        profile = {}
        profile['first_name'] = first
        profile['last_name'] = last

        for key, value in user_info.items():
            profile[key] = value
        return profile

    profile = build_profile('chen', 'wu', location='nanjing', field='dianzi', money='15000')
    print(profile)
   ```

6. 函数存在模块(独立文件)中  
函数存在模块中作用：1.隐藏代码细节；2.重用函数；3.与程序员共享文件而不是整个程序。
   - (1) 导入整个模块
   ```python
   import 模块名
   
   模块名.函数名()
   ```
   - (2) 导入某几个特定函数
   ```python
   from 模块名 import 函数名1, 函数名2,...
   
   函数名1()
   函数名2()
   ```
   - (3) 别名
      + 函数
      ```python
      from 模块名 import 函数名 as 别名
      
      别名()
      ```
      + 模块
      ```python
      import 模块名 as 别名
      
      别名.函数名()
      ```
    - (4) 导入模块所有函数(基本不用)
    ```python
    from 模块名 import *
    ```
 7. 函数编写指南
    - (1) 函数名--描述性名称，小写字母和下划线(_)
    - (2) 注释--紧跟在函数定义之后，文档字符串格式
    - (3) 给形参指定默认值或者对于函数调用中的关键字实参时--等号两边不要空格
    - (4) 形参很多时--在函数定义中输入左括号后按回车，并在下一行按两次Tab键
    - (5) 相邻两个函数--两个空行
    - (6) 每个函数只负责一项具体工作
