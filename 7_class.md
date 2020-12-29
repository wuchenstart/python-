1. 实例化:根据类来创建对象
```python
class Dog():
    """文档字符串"""
    def __init__(self, name, age):
        """初始化属性"""
        self.name = name
        self.age = age
```  

2. 方法：类中的函数
   - (1) __init__()--每当程序根据类创建新实例时，python会自动运行它。  
   未显示包括return语句，但返回一个类的实例
   - (2) 为何在方法定义中包含形参self？
      + Python调用__init()__方法创建实例时，自动传入self参数
      + 每个与类相关联的方法调用都自动传递参数**self，它指向实例本身的引用**，让实例访问类中的属性和方法 
      
3. 使用类和属性  
   - (1) 给属性指定默认值---在__init__()方法中指定初始值；若这样，在__init__()定义中就不需要包含为他提供初始值的形参
   - (2) 修改属性的值
      + 直接经实例修改
      + 经方法改
      + 经方法对属性递增
      
4. 继承--子类继承父类所有属性和方法，还定义自己的属性和方法。
```python
class 子类名(父类名):
    def __init__(self, 子类属性1, 子类属性2):
        """初始化父类属性"""
        super().__init__(父类属性1, 父类属性2)
        self.子类属性1 = 子类属性1
        self.子类属性2 = 子类属性2
```  
   - 重写父类方法：与父类方法同名
   - 将实例用作属性  
   将类的一部分作为独立类提取出来  
   self.battery = Battery()
   - 续航里程是电瓶属性，还是汽车属性
      + 只描述一辆车，get_range()放在Battery类中合适
      + 描述以汽车制造商的整个生产线，将get_range()移到ElectriCar类中，方法依然根据电池容量确定续航，但报告的一款车的续航  
      或者get_range()根据电瓶容量和车型号报告续航，仍然放在Batterty类中
    
5. 导入类
   - (1) 导入多个类
    ```python
    from 模块名 import 类名1, 类名2
    类名1(实参)
    类名2(实参)
    ```
   - (2) 导入整个模块
    ```python
    import 模块名
    模块名.类名(实参)
    ```
   - (3) 模块中导入所有类(不推荐)
    ```python
    from 模块名 import *
    ```

6. Python标准库是一组模块。使用模块collections中的OrderedDict类  
字典将信息联系起来，但不记录添加键-值对的顺序
```python
from collections import OrderedDict
# OrderedDict实例几乎与字典相同，但记录了键-值对的添加顺序
favorite_languages = OrderedDict()

favorite_languages['jen'] = 'python'
favorite_languages['sarah'] = 'c'
favorite_languages['edward'] = 'ruby'

for name, language in favorite_languages.items():
    print(name.title() + "'s favorite language is " + language.title() + ".")
```

7. 类编写指南
   - (1) **类名**--驼峰命名，每个单词首字母大写，不用下划线；**实例名和模块名**--小写，且单词之间加上下划线
   - (2) 每个模块和类名定义后--文档字符串
   - (3) **类中**，一个空行分隔方法；**模块中**，两个空行分隔类
   - (4) 先导入**标准库模块的import语句**，在添加**一空行**，再导入**自己编写的模块的import**
 
