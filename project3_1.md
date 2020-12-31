## 第18章 Django入门（Win8系统下完成，Django版本-2.2.17，python版本-3.5）
# 一、 建立项目
1. 制定规范：项目的目标、功能，项目的外观和用户界面  
目标：编写一个名为“学习笔记”的Web应用程序，让用户**记录主题**，并在**每个主题下可以添加条目**。  
“学习笔记”的**主页**对这个网站描述，并邀请**注册或者登录**。  
用户**登录后**，可新创建新主题、添加新条目以及阅读现有条目。

2. **建立**虚拟环境
   - (1) 新建learning_log文件夹
   - (2) 建立
```python
# 虚拟环境名ll_env
python -m venv ll_env
```

3. **激活**虚拟环境：
```python
ll_env\Scripts\activate 
```
**停止**使用虚拟环境：
```python
deactivate
```

4. 安装Django：
```python
pip install Django
```

5. 在Django中创建项目：
   - (1) **创建项目**
    ```python
    # .要加上,使新项目使用合适的目录结构。项目名learning_log
    django-admin.py startproject learning_log .
    # 不管用换这个，看其他人说的：
    # django-admin startproject learning_log .
    ```  
   - (2) 查看目录
    ```python
    dir
    # 输出：
    # learning_log ll_env manage.py
    # manage.py它接受命令并将其交给Django的相关部分去运行
    ``` 
   - (3) 查看learning_log目录
    ```python
    dir learning_log
    # 输出：
    # __init__.py setting.py urls.py wsgi.py
    # settings.py指定Django如何与系统交互以及如何管理项目。修改、添加设置；添加我的应用程序
    # urls.py告诉Django应创建哪些网页来响应浏览器请求
    # wsgi.py帮助Django提供它创建的文件，文件名是Web服务器网关接口（不懂？？？）
    ``` 

6. 创建数据库  
   - (1) **修改**数据库称**迁移**数据库
    ```python
    # 首次执行命令migrate，确保数据库与项目匹配
    python manage.py migrate
    # 运行后，创建必要的数据库表，
    # 用于存储将在项目(同步未迁移的应用程序)中使用的信息，
    # 再确保数据库结构与当前代码(应用所有的迁移)匹配。
    ``` 
   - (2) 查看目录
    ```python
    dir
    # 结果:
    # db.sqlite3 learning_log ll_env manage.py
    # SQLite是一个使用单个文件的数据库
    ```

7. 查看项目
```python
python manage.py runserver
# 1.核实Django是否正确地创建了项目
# 2.指出Django版本以及当前使用的设置文件的名称
# 3.指出项目的url
```

# 二、 创建应用程序  
Django项目由一系列应用程序组成。  
   - (1) **终端窗口还运行着runserver**,**再打开一个终端窗口**，到manage.py所在目录，激活虚拟环境，执行**startapp**：
```python
# 命令startapp appname,建立创建应用程序的基础设施
python manage.py startapp learning_logs
``` 
   - (2) 查看learning_logs目录  
```python
dir learning_logs
# 结果：
# admin.py __init__.py migrations models.py tests.py views.py
# models.py定义应用程序中管理的数据
```  
# views.py
# admin.py
1. 定义模型：在代码层面，模型就是一个类  
models.py
```python
from django.db import models

# 在这里创建模型
# Model--在Django中一个定义了模型基本功能的类
class Topic(models.Model):
    """用户学习的主题"""
    # CharField字符数据
    text = models.CharField(max_length=200)
    # DateTimeFiled时间数据，
    # 参数auto_now_add=True，每当用户创建新主题时，这个属性自动设置为当前日期和时间
    date_added = models.DateTimeField(auto_now_add=True)
    
    # 告诉Django,默认使用哪个属性显示有关主题的信息。
    # __str__()显示模型的简单表示
    def __str__(self):
        """返回模型的字符串表示"""
        return self.text
```

2. 激活模型  
要使用模型，必须让Django将**应用程序包含到项目**中，即告诉Django哪些应用程序安装在项目中。  
   - (1) 目录learning_log/learning_log--settings.py
    ```python
    --snip--
    # 一个元组，告诉Django项目由哪些应用程序组成
    INSTALLED_APPS = (
        'django.contrib.admin',
        'django.contrib.auth',
        'django.contrib.contenttypes',
        'django.contrib.sessions',
        'django.contrib.messages',
        'django.contrib.staticfiles',

        # 我的应用程序
        'learning_logs',
    )
    --snip--
    ```    
   - (2) 修改数据库，使其能够存储与模型Topic相关的的信息
    ```python
    # makemigrations让Django确定如何修改数据库
    python manage.py makemigrations learning_logs
    # 结果：
    # 创建了一个名为0001_inital.py的**迁移文件**，这个文件将在数据库中为**模型Topic创建一个表**
    ```
   - (3) 应用迁移
    ```python
    python manage.py  migrate
    # 结果确认learnings_logs应用迁移正常
    ```
 **注**：每当修改“学习笔记”**管理的数据**时，三步：修改models.py;对learning_logs调用makemigrations;让Django迁移项目。  
 
3. Django管理网站-通过管理网站使用Topic模型添加主题  
Django提供的管理网站（admin site）,处理模型。网站管理员可使用管理网站，普通用户不能使用。  
(了解)最严格权限设置只允许用户阅读网站的公开信息；注册的用户可阅读自己私有数据，还可查看一些只有会员能查看的信息。
   - (1) 创建超级用户
   ```python
   python manage.py createsuperuser
   # 输入超级用户名、邮箱、两次密码
   ```
   - (2) 向管理网站注册模型    
   models.py所在目录的admin.py
   ```python
   from django.contrib import admin
   
   #  向管理网站注册Topic模型
   from learning_logs.models import Topic
   
   admin.site.register(Topic)
   ```
   激活虚拟环境，执行命令python manage.py runserver，访问http://localhost:8000/admin/ -输入超级用户名和密码
   - (3) 添加主题  
   单击Topics,点击Add，看到一个用于添加新主题的表单。save
# 三、创建网页：学习笔记主页
# 四、创建其他网页
  
