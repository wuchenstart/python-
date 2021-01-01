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
    ``` 
 **settings.py**指定Django如何与系统交互以及如何管理项目。修改、添加设置；添加我的应用程序  
 **urls.py**告诉Django应创建哪些网页来响应浏览器请求  
 **wsgi.py**帮助Django提供它创建的文件，文件名是Web服务器网关接口（不懂？？？）

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
```  
**models.py**-定义应用程序中管理的数据
**views.py**-用于存放视图函数
**admin.py**-用于注册模型
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

4.  定义模型Entry  
添加条目定义模型，每个条目与特定主题相关联，**多对一关系**。即多个条目关联到同一主题  
**外键**，引用了数据库中的另一条记录；将每个条目关联到特定主题。
models.py
```python
from django.db import models

class Topic(models.Model):
    --snip--

class Entry(models.Model):
    """学到的有关某个主题的具体知识"""
    # 每个主题创建时，分配一个键（ID）
    # 需要在两项数据之间建立联系时，Django使用与每项信息相关联的键。
    topic = models.ForeignKey(Topic, on_delete=models.CASCADE)
    text = models.TextField()
    date_added = models.DateTimeField(auto_now_add=True)
    
    # Meta存储用于管理管理模型的额外信息。设置一个特殊属性，Entries来表示多个条目。
    # 若无这个类，用Entrys表示多个条目
    class Meta:
        verbose_name_plural = 'entries'

    def __str__(self):
        """返回模型的字符串表示"""
        return self.text[:50] + "..."
```

5. 迁移模型Entry  
**添加了一个新模型，需要再次迁移数据库**
   - (1) 修改models.py
   - (2) 执行命令：
   ```python
   python manage.py makemigrations app_name
   ```
   - (3) 在执行命令
   ```python
   python manage.py migrate
   ```
   
6. 向管理网站注册Entry  
admin.py
```python
from django.contrib import admin

from learning_logs.models import Topic, Entry

admin.site.register(Topic)
admin.site.register(Entry)
```

7. Django shell:交互式编程
```python
python manage.py shell
```

# 三、创建网页：学习笔记主页
用Djan创建网页的过程三阶段：**定义URL、编写视图、编写模板**  
**URL模式**描述URL如何设计，让Django知道如何将浏览器请求与网站URL匹配，以确定返回哪个网页。  
每个URL映射到特定的视图，**视图函数**获取并处理网页所需要的数据，视图函数通常调用一个模板，**模板**生成浏览器能够理解的网页。  
1. 映射URL
项目主文件夹learning_log的urls.py
```python
# 导入为项目和管理网站管理URL的函数和模块
from django.conf.urls import include, path
from django.contrib import admin

# urlpatterns包含项目中的应用程序的URL
urlpatterns = [
    # admin.site.urls定义了在管理网站中请求的所有URL
    path('admin/', include(admin.site.urls)),
    # namespace让learning_logs的URL与项目中的其他URL区分
    path('', include('learning_logs.urls', namespace='learning_logs')),
    ]
```  
learning_logs中的urls.py
```python
"""定义learning_logs的URL模式"""
# 用path将URL映射到视图
from django.conf.urls import path
# 从当前的urls.py所在文件夹导入views
from . import views

app_name = 'learning_logs'
# 包含可在应用程序learning_ logs中请求的网页
urlpatterns = [
    # path包含三个参数，第一个参数，定义了Django可查找的模式。python忽略项目的基础URL（http://localhost:8000）
    # 第二个参数,指定要调用的视图函数
    # 第三个参数，将URL模式的名称指定为index，在代码其他地方能引用它
    # 主页
    path('', views.index, name='index'),
]
```

2. 编写视图
learning_logs下的views.py
```python
from django.shortcuts import render

def index(request):
    """学习笔记主页"""
    # render()根据视图提供的数据渲染响应
    return render(request, 'learning_logs/index.html')
```
**当URL请求与定义的URL模式匹配时**，Django将在文件views.py查找index(),再将请求对象传递给这个视图函数。
3. 编写模板
**模板**定义了网页的结构。  
   - (1) 文件夹learning_logs中新建一个文件夹，并将其命名为templates。  
   - (2) 在templates中新建learning_logs文件夹，
   - (3) 在learning_logs中新建index.html。
index.html
```html
<p>Learning Log</p>

<p>Learning Log helps ...</p>
```
# 四、创建其他网页
  
