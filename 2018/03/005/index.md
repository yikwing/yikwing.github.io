# ubuntu从零开始搭建爬虫框架


## 更换 ubuntu apt 源

- 查看 ubuntu 开发代号

```shell
sudo lsb_release -a
```

![此处输入图片的描述][1]

```shell
## linux路径
sudo vim  /etc/apt/sources.list

deb http://mirrors.aliyun.com/ubuntu/ artful main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ artful-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ artful-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ artful-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ artful-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ artful main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ artful-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ artful-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ artful-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ artful-backports main restricted universe multiverse
```

- 安装扩展插件

```shell
sudo apt-get install python3-dev
sudo apt-get install python3-pip
sudo apt-get install virtualenv

# pip3 安装插件报错
export LC_ALL=C

virtualenv -p /usr/bin/python3 py3env
source ./py3env/bin/activite

pip3 install gerapy
```

- 安装 scrapyd

```shell
pip3 install scrapyd
pip3 install scrapyd-client

sudo vim /etc/scrapyd/scrapyd.conf

eg:~~~~~~
[scrapyd]
eggs_dir    = eggs
logs_dir    = logs
items_dir   =
jobs_to_keep = 5
dbs_dir     = dbs
max_proc    = 0
max_proc_per_cpu = 4
finished_to_keep = 100
poll_interval = 5.0
bind_address = 127.0.0.1
http_port   = 6800
debug       = off
runner      = scrapyd.runner
application = scrapyd.app.application
launcher    = scrapyd.launcher.Launcher
webroot     = scrapyd.website.Root

[services]
schedule.json     = scrapyd.webservice.Schedule
cancel.json       = scrapyd.webservice.Cancel
addversion.json   = scrapyd.webservice.AddVersion
listprojects.json = scrapyd.webservice.ListProjects
listversions.json = scrapyd.webservice.ListVersions
listspiders.json  = scrapyd.webservice.ListSpiders
delproject.json   = scrapyd.webservice.DeleteProject
delversion.json   = scrapyd.webservice.DeleteVersion
listjobs.json     = scrapyd.webservice.ListJobs
daemonstatus.json = scrapyd.webservice.DaemonStatus
```

- 启动 gerapy

```shell
gerapy init

cd gerapy
gerapy migrate

gerapy runserver
gerapy runserver 0.0.0.0:8888
```

[1]: https://s13.postimg.org/fq3z4univ/Jietu20180124-171333.jpg
