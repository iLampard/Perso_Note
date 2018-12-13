# Scheduler tools snippets



### Airflow
#### Install
[Incubator-Airflow](https://github.com/apache/incubator-airflow) 只能在linux平台上安装运行。

建议先创建虚拟环境, 然后pip安装或者clone最新开发版本安装。

如果要改变airflow的默认路径，可以如下操作
```bash
# 如果在ubuntu系统中
gedit ~/.bashrc
#airflow
export AIRFLOW_HOME=/home/user/workarea/airflow
source ~/.bashrc
```


然后在终端中输入
```python
# initialize the database
airflow initdb

# start the web server, default port is 8080
airflow webserver -p 8080
```

在另一个终端中输入
```python
# start the scheduler
airflow scheduler
```

#### config 设置

##### timezone
```bash
default_timezone = Asia/Shanghai
```

##### email

```bash
[smtp]
# If you want airflow to send emails on retries, failure, and you want to use
# the airflow.utils.email.send_email_smtp function, you have to configure an
# smtp server here
smtp_host = mail.company.com
smtp_starttls = True
smtp_ssl = False
# Uncomment and set the user/pass settings if you want to use SMTP AUTH
smtp_user = emailuser
smtp_password = pass
smtp_port = 25
smtp_mail_from = xx@company.com
```



