### 相关命令
#### 创建项目

`django-admin startproject mysite`

两个问题：
- 名称必须是小写、数字、下划线组成
- 文件名必须不存在

解决：
使用 "." 通配

#### 运行服务

`python manage.py runserver`

后面可以跟指定端口和ip

#### 创建应用

`python manage.py startapp app_name`

#### 数据库配置

```
# 数据库配置， 默认使用 SQLite
DATABASES = {
    'default': {
        # 默认的数据库，还可以是postgresql、mysql、oracle
        'ENGINE': 'django.db.backends.sqlite3',
        # 数据库名称 如果是文件数据库（SQLite），则是文件完整的绝对路径，包含文件名
        'NAME': BASE_DIR / 'db.sqlite3',
        # 额外的设置，USER、PASSWARD、HOST
    }
}
```

#### 项目包管理

```
# 项目启动所需要的所有 Django 应用
# 默认开启的某些应用需要至少一个数据表，使用时需要创建这些表
INSTALLED_APPS = [
    'view.apps.ViewConfig',  # 自己创建的服务
    'django.contrib.admin',  # 管理员站点
    'django.contrib.auth',   # 权限认证
    'django.contrib.contenttypes',  # 内容类型框架
    'django.contrib.sessions',  # 会话框架
    'django.contrib.messages',  # 消息框架
    'django.contrib.staticfiles',  # 静态文件管理
]
```

对于用户自己创建的应用（创建应用命令），每个应用都有属于自己的应用名称（在apps.py中），若要
放在项目包中管理，需要放到这个列表中

#### 创建项目应用所需要的数据表

`python manage.py migrate`

通常是第一次创建项目时使用，新加入包时使用

#### 执行项目db初始化

`python manage.py makemigrations app_name`

`makemigrations` 会检测对模型的修改，并把修改存储为一次 *迁移*
其实就是一个构建一个迁移文件（xxx_initial.py），文件描述了新的model设计结构等

#### model 迁移命令

SQL迁移内容查看命令

`python manage.py sqlmigrate app_name 000x`

执行迁移
`python manage.py migrate`

会找出还未迁移的的项目，然后同步到数据结构上

#### 检查命令

`python manage.py check`

#### 命令行

`python manage.py shell`

#### json 返回

- simplejson

- Serialization