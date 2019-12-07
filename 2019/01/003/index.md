# 使用Aria2+Aria2Ng+OneIndex+OneDrive建立不限流量/离线BT下载/在线观看网盘


- 宝塔

```shell
#Centos系统
yum install -y wget && wget -O install.sh http://download.bt.cn/install/install.sh && sh install.sh

#Ubuntu系统
wget -O install.sh http://download.bt.cn/install/install-ubuntu.sh && sudo bash install.sh

#Debian系统
wget -O install.sh http://download.bt.cn/install/install-ubuntu.sh && bash install.sh
```

这里只需要安装 Nginx 和 php5.6 就可以了，其他 FTP，Mysql 不需要

- 安装 Aria2

```shell
wget -N --no-check-certificate https://raw.githubusercontent.com/ToyoDAdoubiBackup/doubi/master/aria2.sh && chmod +x aria2.sh && bash aria2.sh
#备用地址
wget -N --no-check-certificate https://www.moerats.com/usr/shell/Aria2/aria2.sh && chmod +x aria2.sh && bash aria2.sh
```

选择 1，安装 aria2 后一路 yes 就可以了。最后会出来一个密钥，记下来

- 安装 ArinanNG 和 oneindex

```shell
# NG
https://github.com/mayswind/AriaNg/releases/download/1.0.0/AriaNg-1.0.0.zip

# oneIndex
https://github.com/donwa/oneindex/archive/master.zip
```

- 编辑配置文件

```shell
dir=/Download
on-download-complete=/root/upload2one.sh
```

自动上传 OneDrive

```shell
#!/bin/bash
path=$3
downloadpath='/Download'
if [[ $2 -eq 0 ]]
       then
               exit 0
fi
while true; do  #提取下载文件根路径，如把/root/downloads/a/b/c/d.jpg变成/root/downloads/a
filepath=$path
path=${path%/*};
if [ "$path" = "$downloadpath" ] && [ $2 -eq 1 ]  #如果下载的是单个文件
   then
   /www/server/php/72/bin/php /www/wwwroot/你的ip或者域名/one/one.php upload:file "$filepath" /upload/
   rm -rf "$filepath"
   exit 0
elif [ "$path" = "$downloadpath" ]   #文件夹
   then
   /www/server/php/72/bin/php /www/wwwroot/你的ip或者域名/one/one.php upload:folder "$filepath"/ /upload/"${filepath##*/}"/
   rm -rf "$filepath"/
   exit 0
fi
done
```

赋予权限,重启 aria2NG

```shell
chmod +x /root/upload2one.sh

./aria2.sh
```
