# sql_manage_platform-MySQL
## 基于django和inception，带权限控制的mysql语句运行系统
### 开发环境：
#### django:1.8.14
#### python:2.7.12
### python依赖组件：
#### django-crontab
#### django-simple-captcha
#### django-sslserver
#### django-ratelimit
#### MySQL-python
### 权限功能简述：
用户的系统使用权限大致可以分为可以看到的页面，以及能够看到的DB两个维度

这两个维度的权限都可以通过设置组来达到后期快速添加用户的需求

对于前者：

所有页面都可以根据需要分配给不同权限的用户

对于DB维度的权限：

一个DB可以配置role为read和write两个ip-port实例，用以区分查询和变更语句执行的实例，（也可以将role配置成all不进行区分）

对于数据库账户，一个DB可以配置多个，并分配给不同的用户，用以实现不同用户在同一db下区分权限的功能。（也可以保持默认设置，即分配给public用户，不进行区分）

如果要使用任务管理功能，需要为DB添加一个role为admin的数据库账号。。未完待续
### 启动配置
#### config.ini配置文件如下：
[settings]

host="127.0.0.1" 

port=3306

user="xx"

passwd="xxx"

dbname="xxx"

wrong_msg="select '请检查输入语句'"

select_limit=200

export_limit=200

incp_host="x.x.x.x"

incp_port=6669

incp_user=""

incp_passwd=""

public_user="public"
##### 说明:
host-dbname为django库的连接配置，和settings.py中的一致即可

select_limit 和 export_limit为系统默认查询和导出条数限制

incp_XX系列配置文件为inception的连接配置
#### 定时任务配置
修改seheduled.py中第18行路径地址为正确地址

config.readfp(open('/root/PycharmProjects/mypro/myapp/etc/config.ini','r'))

然后运行 python manage.py crontab add 添加 crontab任务

### 启动：
#### python manage.py runsslserver 0.0.0.0:8000

# 页面展示如下:
## 1.登录界面
![image](https://github.com/speedocjx/myfile/blob/master/sql-manage-platform/login.jpg)
## 2.主页
![image](https://github.com/speedocjx/myfile/blob/master/sql-manage-platform/main.jpg)
## 3.表结构查询界面
支持表名模糊搜索
![image](https://github.com/speedocjx/myfile/blob/master/sql-manage-platform/meta_query.jpg)
### 3.2查询结果:
![image](https://github.com/speedocjx/myfile/blob/master/sql-manage-platform/meta_info.jpg)
## 4.查询界面
### 4.1表查询:
支持单条sql的查询和查询结果的导出，导出条数限制默认为config.ini中配置的值，也可以通过后台myapp_profile表对特定用户进行调整
![image](https://github.com/speedocjx/myfile/blob/master/sql-manage-platform/mysql_query.jpg)
## 5.执行界面
支持单条sql语句的执行，用户能够执行的语句类型可以通过权限限制。
![image](https://github.com/speedocjx/myfile/blob/master/sql-manage-platform/mysql_exec.jpg)
## 6.任务提交界面
![image](https://github.com/speedocjx/myfile/blob/master/sql-manage-platform/task_upload.jpg)
## 7.任务管理界面
可以审核、查看、修改、执行、预约任务执行时间，通过调用inception接口来实现

不同用户能够看到的页面按钮可以通过权限控制

通过inception调用pt-osc执行的任务可以被终止，停止后需要到库中人工清理触发器
![image](https://github.com/speedocjx/myfile/blob/master/sql-manage-platform/task_manage.jpg)
### 7.1任务执行结果示例
![image](https://github.com/speedocjx/myfile/blob/master/sql-manage-platform/resul_of_task.jpg)
### 7.2任务编辑界面
可以通过权限设置来限制用户是否能够编辑此页面的内容

![image](https://github.com/speedocjx/myfile/blob/master/sql-manage-platform/task_edit.jpg)
## 8.日志查询界面
本平台记录所有用户在mysql_query,mysql_exec以及任务管理页面中执行的语句

这些语句可以通过日志查询页面进行搜索

![image](https://github.com/speedocjx/myfile/blob/master/sql-manage-platform/oper_log.jpg)
## 9.权限查询页面示例
### 9.1按db查询
![image](https://github.com/speedocjx/myfile/blob/master/sql-manage-platform/pre_query_db.jpg)
### 9.2按db组查询
![image](https://github.com/speedocjx/myfile/blob/master/sql-manage-platform/pre_query_dbgroup.jpg)
### 9.3按用户账号查询
![image](https://github.com/speedocjx/myfile/blob/master/sql-manage-platform/pre_query_user.jpg)
### 9.4按实例查询
![image](https://github.com/speedocjx/myfile/blob/master/sql-manage-platform/pre_query_ins.jpg)
## 10.用户账户设置界面
![image](https://github.com/speedocjx/myfile/blob/master/sql-manage-platform/user_set.jpg)
## 11.DB快速创建界面
![image](https://github.com/speedocjx/myfile/blob/master/sql-manage-platform/fast_dbset.jpg)
## 12.DB详细设置界面
![image](https://github.com/speedocjx/myfile/blob/master/sql-manage-platform/db_detailset.jpg)
## 13.用户页面权限设置界面
![image](https://github.com/speedocjx/myfile/blob/master/sql-manage-platform/group_set.jpg)
## 14.DB组设置界面
![image](https://github.com/speedocjx/myfile/blob/master/sql-manage-platform/dbgroup_set.jpg)
