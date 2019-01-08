# Learning Log

## Django 入门

通过Pycharm的 *New Project* 菜单. 已经做好了如下工作:

1. 建立并激活虚拟环境;
2. 安装Django
3. 在Django中创建项目: `django-admin startproject learning_log .`
4. 创建应用程序 **learning_logs** : `python manage.py startapp learning_logs`
5. 把`learning_logs.apps.LearningLogsConfig` 加入到`setting.py`的`INSTALLED_APPS`以激活程序.

### 创建项目

#### 创建数据库

`python manage.py migrate`

#### 查看项目

`python manage.py runserver`

### 创建应用程序

#### 定义(数据)模型

定义`Topic` 模型
1. `text`
2. `data_added` 
3. `__str__`

#### 激活模型

再一次迁移数据库:
```shell
python manage.py makemigrations learning_logs
python manage.py migrate
```

#### 迁移总结

**每当需要修改数据时, 都采取如下三个步骤:**
1. **修改`models.py`**
2. **对修改的django 应用程序调用`makemigrations`**
3. **迁移数据: `python manage.py migrate`**

#### Django 管理网站

1. 创建超级用户: `python manage.py createsuperuser` (密码: 大吉大利)
2. 向管理网站注册模型
3. 添加主题

#### 定义模型Entry

#### 迁移模型Entry

**每当需要修改数据时, 都采取如下三个步骤:**
1. **修改`models.py`**
2. **对修改的django 应用程序调用`makemigrations`**
3. **迁移数据: `python manage.py migrate`**

#### 向管理网站注册Entry

#### Django shell

`python manage.py shell`

备注: pycharm的 *Python Console* 默认就是可以使用Django Shell的.

```python
from learning_logs.models import Topic
Topic.objects.all()

topics = Topic.objects.all()
for topic in topics:
    print(topic.id, topic)

t = Topic.objects.get(id=1)
t.text
t.date_added
# 要通过外键关系获取数据, 可以使用相关模型的**小写名称、下划线、和单词set**
t.entry_set.all()

```

### 创建网页：学习笔记主页

通过Django创建网页的3步骤:

1. 定义URL
2. 编写视图(view)
3. 编写模版

#### 映射URL

#### 编写视图

#### 编写模板

### 创建其他网页

#### 模板继承

1. 父模板 `base.html`
2. 子模板 `index.html` (重新编写)

#### 显示所有主题的页面

1. URL模式: `topics/`
2. 视图
3. 模板

#### 显示特定主题的页面

1. URL模式: `^topics/(?P<topic_id>\d+/$`
2. 视图
3. 模板
4. 将显示所有主题的页面中的每个主题都设置为链接

## 用户账户

### 让用户能够输入数据

> Django 表单创建工具 
> 需要导入包含表单的模块`forms.py`

#### 添加新主题

1. 用于添加主题的表单
2. URL模式 `new_topic`
3. 视图函数 `new_topic()`
4. GET请求还是POST请求
5. 模板new_topic

#### 添加新条目

1. 用于添加新条目的表单
2. URL模式`new_entry`
3. 视图函数`new_entry()`
4. 模板`new_entry`
5. 链接到页面`new_entry`

#### 编辑条目

1. URL模式`edit_entry`
2. 视图函数`edit_entry()`
3. 模板`edit_entry`
4. 链接到页面`edit_entry`

至此, "学习笔记" 已具备了需要的大部分功能. 用户可以添加主题和条目, 还可以根据需要查看任何一组条目.
下面, 会实现一个用户注册系统.

### 创建用户账户

#### 应用程序`users`

创建`users`应用程序
`python manage.py startapp users`

1. 将应用程序`users`添加到`settings.py`中
2. 包含应用程序`users`的URL

#### 登录页面

创建`users`目录的`urls.py`

1. 模板`login.html`
2. 链接到登陆页面
3. 使用登陆界面

#### 注销

不创建注销的页面, 用户只需要单击一个链接就能注销并返回到主页.

1. 注销URL
2. 视图函数`logout_view()`
3. 链接到注销视图

#### 注册页面

1. 注册页面的URL模式
2. 视图函数`register()`
3. 注册模板
4. 链接到注册页面

### 让用户拥有自己的数据

#### 使用`@login_required`限制访问

1. 限制对`topics`页面的访问
2. 全面限制对项目"学习笔记"的访问

#### 将数据关联到用户

只需要将最高层的数据关联到用户, 这样更低层的数据将自动关联到用户. 

1. 修改模型`Topic`
2. 确定当前有哪些用户 - 最简单的办法, 将既有主题都关联到一个用户, 如超级用户. 
    ```python
    # django shell
    from django.contrib.auth.models import User
    User.objects.all()
    # <QuerySet [<User: ll_admin>, <User: test>]>
    for user in User.objects.all():
        print(user.username, user.id)
    # ll_admin 1
    # test 2
    ```
3. 迁移数据库
    ```
    (learning_log)  C:\Users\east4ming\PycharmProjects\learning_log>python manage.py makemigrations learning_logs
    You are trying to add a non-nullable field 'owner' to topic without a default; we can't do that (the database needs something to populate existing rows)
    .
    Please select a fix:
     1) Provide a one-off default now (will be set on all existing rows with a null value for this column)
     2) Quit, and let me add a default in models.py
    Select an option: 1
    Please enter the default value now, as valid Python
    The datetime and django.utils.timezone modules are available, so you can do e.g. timezone.now
    Type 'exit' to exit this prompt
    >>> 1
    Migrations for 'learning_logs':
      learning_logs\migrations\0003_topic_owner.py
        - Add field owner to topic

    (learning_log)  C:\Users\Cui Kaidong\PycharmProjects\learning_log>python manage.py migrate
    Operations to perform:
      Apply all migrations: admin, auth, contenttypes, learning_logs, sessions
    Running migrations:
      Applying learning_logs.0003_topic_owner... OK 
    ```
    验证: (django shell)
    ```python
    from learning_logs.models import Topic
    for topic in Topic.objects.all():
        print(topic, topic.owner)
        
    # Chess ll_admin
    # Rock Climbing ll_admin
    # work ll_admin
    ```

> 重建数据库
> `python manage.py flush`

#### 只允许用户访问自己的主题

#### 保护用户的主题

目前有bug, 用户可以直接输入url: `/topics/1` 来查看其他人的数据.
为修复bug, 在视图函数`topic()`获取请求的条目前执行检查. 不是对应的用户, 返回 403(Forbidden)页面.

#### 保护页面`edit_entry`

同上

#### 将新主题关联到当前用户

## 设置应用程序的样式并对其进行部署

目标:

- 设置样式
- 部署到远程服务器上

### 设置项目"学习笔记"的样式

`django-bootstrap3`

#### 应用程序`django-bootstrap3`

> 这个应用程序下载必要的Bootstrap文件, 将它们放到项目的合适位置, 让你能够在项目的模板中使用样式设置指令.

安装`django-bootstrap3`
```bash
pipenv lock
# Success!
# Updated Pipfile.lock (1adabf)!
pipenv install django-bootstrap3
```

#### 使用 Bootstrap 来设置"学习笔记"的样式

### 部署"学习笔记"



