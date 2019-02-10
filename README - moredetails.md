# LearningLog
A Django Study Program. 
Developing a "Learning Log" program, this is a online log system, which you can write your knowledge on the site.

[TOC]

我们将为这个项目:

1. 制定规范
2. 为应用程序使用的数据定义模型
3. 使用Django的管理系统来输入一些初始数据
4. 学习编写**视图**和**模板**, 让Django能够为我们的网站创建网页

分3章节:
1. Django是一个**Web框架** -- 一套用于帮助开发交互式网站的工具. 
    > Django能相应网页请求, 还能让我们更轻松地读写数据库/管理用户等.
2. 改进该项目, 增加用户账户等
3. 将该项目部署到活动的服务器.

## Django 入门

### 建立项目

先对项目进行描述, 再建立虚拟环境, 以便再其中创建项目.

#### 制定规范

完整规范包括:

- 项目目标
- 项目功能
- 项目的外观和用户界面

**规范如下**:

**编写一个名为"学习笔记"的Web应用程序, 让用户能够记录感兴趣的主题, 并在学习每个主题的过程中添加日志条目.**
**"学习笔记"的主页对这个网站进行描述, 并邀请用户注册或登陆.**
**用户登陆后, 就可以创建新主题/添加新条目以及阅读既有的条目.**

#### 建立虚拟环境

要使用Django, 首先需要建立一个虚拟工作环境. *虚拟环境*是系统的一个位置, 你可以在其中安装包, 并将其与其他Python隔离. 

为项目新建一个目录, 再在终端中切换到该目录, 并新建一个虚拟环境.
如果使用的是Python3, 可以如下命令创建:

```shell
python -m venv ll_env
```

这里运行了模块*venv*, 并使用它来创建一个名为*ll_env*的虚拟环境. 如果*venv*模块不可用. 参考下一节.

#### 安装 virtualenv

如果使用的是Python较早版本, 或系统没有正确设置, 不能使用模块venv, 可以安装virtualenv包.

`pip install --user virtualenv`

> 在Ubuntu中, 可以使用命令`sudo apt-get install python-virtualenv`

在终端中切换到项目目录, 并像下面这样来创建虚拟环境.

`virtualenv ll_env`

> 如果系统安装了多个Python版本, 需要制定virtualenv使用的版本. 
> 如: `virtualenv ll_env --python=python3` 创建Python3虚拟环境.

#### 激活虚拟环境

`source ll_env/bin/active`

> 如果是windows环境, 使用`ll_env\Scripts\activate`来激活虚拟环境. 如下:
> ```cmd
> E:\WorkSpace\LearningLog>ll_env\Scripts\activate
> (ll_env) E:\WorkSpace\LearningLog>
> ```

要停止使用虚拟环境, 可执行命令`deactivate`

#### 安装Django

```cmd
(ll_env) E:\WorkSpace\LearningLog>pip install django
Collecting django
  Downloading Django-2.0.3-py3-none-any.whl (7.1MB)
    100% |████████████████████████████████| 7.1MB 37kB/s
Collecting pytz (from django)
  Downloading pytz-2018.3-py2.py3-none-any.whl (509kB)
    100% |████████████████████████████████| 512kB 33kB/s
Installing collected packages: pytz, django
Successfully installed django-2.0.3 pytz-2018.3
```

#### 在Django中创建项目

在虚拟环境中执行如下命令来新建一个项目

```cmd
(ll_env) E:\WorkSpace\LearningLog>django-admin startproject learning_log .

(ll_env) E:\WorkSpace\LearningLog>dir
 E:\WorkSpace\LearningLog 的目录

2018/03/26 周一  20:52    <DIR>          .
2018/03/26 周一  20:52    <DIR>          ..
2018/03/26 周一  19:58             1,258 .gitignore
2018/03/26 周一  20:50    <DIR>          .idea
2018/03/26 周一  20:52    <DIR>          learning_log
2018/03/26 周一  19:58             1,087 LICENSE
2018/03/26 周一  20:24    <DIR>          ll_env
2018/03/26 周一  20:52               559 manage.py
2018/03/26 周一  20:50             3,232 README.md
               4 个文件          6,136 字节
               5 个目录 63,126,753,280 可用字节

(ll_env) E:\WorkSpace\LearningLog>dir learning_log
 E:\WorkSpace\LearningLog\learning_log 的目录

2018/03/26 周一  20:52    <DIR>          .
2018/03/26 周一  20:52    <DIR>          ..
2018/03/26 周一  20:52             3,226 settings.py
2018/03/26 周一  20:52               775 urls.py
2018/03/26 周一  20:52               417 wsgi.py
2018/03/26 周一  20:52                 0 __init__.py
               4 个文件          4,418 字节
               2 个目录 63,126,753,280 可用字节

```

创建一个名为*learning_log*的项目. 
有一个名为*manage.py*的文件, 这是一个简单的程序, 它接受命令并将其交给Django的相关部分去运行.

目录*learning_log*包含4个文件. 其中最重要的是*settings.py*, *urls.py*, 和*wsgi.py*.

- *settings.py* 制定Django如何与你的系统交互以及如何管理项目
- *urls.py*告诉Django应创建哪些网页来响应浏览器请求. 
- *wsgi.py*帮助Django提供它创建的文件, 它是*web server gateway interface*(Web服务器网关接口)的缩写.

#### 创建数据库

Django将大部分与项目相关的信息都存储在数据库中. 
创建数据库执行下面命令:

```cmd
(ll_env) E:\WorkSpace\LearningLog>python manage.py migrate
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, sessions
Running migrations:
  Applying contenttypes.0001_initial... OK
  Applying auth.0001_initial... OK
  Applying admin.0001_initial... OK
  Applying admin.0002_logentry_remove_auto_add... OK
  Applying contenttypes.0002_remove_content_type_name... OK
  Applying auth.0002_alter_permission_name_max_length... OK
  Applying auth.0003_alter_user_email_max_length... OK
  Applying auth.0004_alter_user_username_opts... OK
  Applying auth.0005_alter_user_last_login_null... OK
  Applying auth.0006_require_contenttypes_0002... OK
  Applying auth.0007_alter_validators_add_error_messages... OK
  Applying auth.0008_alter_user_username_max_length... OK
  Applying auth.0009_alter_user_last_name_max_length... OK
  Applying sessions.0001_initial... OK

(ll_env) E:\WorkSpace\LearningLog>dir .

 E:\WorkSpace\LearningLog 的目录
2018/03/26 周一  21:10    <DIR>          .
2018/03/26 周一  21:10    <DIR>          ..
2018/03/26 周一  20:56             1,267 .gitignore
2018/03/26 周一  21:00    <DIR>          .idea
2018/03/26 周一  21:10           131,072 db.sqlite3
2018/03/26 周一  21:10    <DIR>          learning_log
2018/03/26 周一  19:58             1,087 LICENSE
2018/03/26 周一  20:24    <DIR>          ll_env
2018/03/26 周一  20:52               559 manage.py
2018/03/26 周一  21:08             5,140 README.md
               5 个文件        139,125 字节
               5 个目录 63,126,597,632 可用字节
```

修改数据库称为*迁移数据库*. 首次执行命令`migrate`时, 让Django确保数据库与项目的当前状态匹配. 在使用SQLite的新项目中首次
执行该命令时, Django将新建一个数据库. Django指出它将创建必要的数据库表, 用于存储我们将在这个项目中的信息, 再确保
数据库结构与当前代码匹配.

Django会又创建一个文件 -- *db.sqlite3*. SQLite是一种使用单个文件的数据库, 是编写简单应用程序的理想选择, 因为它让你不用太
关注数据库管理的问题.

#### 查看项目

合适Django是否正确地创建了项目. 执行命令`runserver`, 如下:

```cmd
(ll_env) E:\WorkSpace\LearningLog>python manage.py runserver 8001
Performing system checks...

System check identified no issues (0 silenced).
March 26, 2018 - 21:24:48
Django version 2.0.3, using settings 'learning_log.settings'
Starting development server at http://127.0.0.1:8001/
Quit the server with CTRL-BREAK.

```

> 如果运行报错: `Error: [WinError 10013] 以一种访问权限不允许的方式做了一个访问套接字的尝试。`
> 可能是因为8000端口被占用. 酷狗就会占用8000端口, 而我正好在听歌...

Django的运行提示中说明了URL地址. 在Web浏览器中打开该URL. 会看到一个页面, 这是Django创建的, 让你知道一切运行正常.

### 创建应用程序

*Django*由一系列应用程序组成, 他们协同工作, 让项目成为一个整体.
我们暂时只创建一个应用程序, 它将完成项目的大部分工作. 在第二章, 将再添加一个管理用户账户的应用程序.

在另一个终端窗口, 切换到manage.py所在目录, 激活虚拟环境, 再执行命令*startapp*:

```cmd
E:\WorkSpace\LearningLog>ll_env\Scripts\activate
(ll_env) E:\WorkSpace\LearningLog>python manage.py startapp learning_logs

(ll_env) E:\WorkSpace\LearningLog>dir
 驱动器 E 中的卷是 文档
 卷的序列号是 0008-0DBA

 E:\WorkSpace\LearningLog 的目录

2018/03/26 周一  21:42    <DIR>          .
2018/03/26 周一  21:42    <DIR>          ..
2018/03/26 周一  21:35             1,285 .gitignore
2018/03/26 周一  21:40    <DIR>          .idea
2018/03/26 周一  21:10           131,072 db.sqlite3
2018/03/26 周一  21:10    <DIR>          learning_log
2018/03/26 周一  21:42    <DIR>          learning_logs
2018/03/26 周一  19:58             1,087 LICENSE
2018/03/26 周一  20:24    <DIR>          ll_env
2018/03/26 周一  20:52               559 manage.py
2018/03/26 周一  21:40             8,456 README.md
               5 个文件        142,459 字节
               6 个目录 63,126,577,152 可用字节

(ll_env) E:\WorkSpace\LearningLog>dir learning_logs
 驱动器 E 中的卷是 文档
 卷的序列号是 0008-0DBA

 E:\WorkSpace\LearningLog\learning_logs 的目录

2018/03/26 周一  21:42    <DIR>          .
2018/03/26 周一  21:42    <DIR>          ..
2018/03/26 周一  21:42                66 admin.py
2018/03/26 周一  21:42               105 apps.py
2018/03/26 周一  21:42    <DIR>          migrations
2018/03/26 周一  21:42                60 models.py
2018/03/26 周一  21:42                63 tests.py
2018/03/26 周一  21:42                66 views.py
2018/03/26 周一  21:42                 0 __init__.py
               6 个文件            360 字节
               3 个目录 63,126,577,152 可用字节

```

该命令让Django建立创建应用程序所需的基础设施. 查看项目目录, 其中新增了一个文件夹*learning_logs*. 
打开该文件夹, 创建的重要文件有*models.py* *admin.py* *views.py*. 
我们将使用*models.py*来定义我们要在应用程序中管理的数据.

#### 定义模型

