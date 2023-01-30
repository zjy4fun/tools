<!-- vscode-markdown-toc -->
* 1. [linux mint 装机](#linuxmint)
	* 1.1. [卸载无用软件](#)
	* 1.2. [换源&&升级](#-1)
	* 1.3. [安装常用软件](#-1)
	* 1.4. [配置 Git](#Git)
	* 1.5. [生成密钥](#-1)
* 2. [安装 docker](#docker)
* 3. [nvm设置镜像](#nvm)
	* 3.1. [linux](#linux)
	* 3.2. [Win](#Win)
* 4. [npm 设置镜像](#npm)
* 5. [npm 特殊包镜像](#npm-1)
* 6. [解决 Jetbrains 中文输入法不跟随问题](#Jetbrains)
* 7. [Ubuntu 20.04 Gnome 禁用 tracker 避免 CPU 100%的问题](#Ubuntu20.04GnometrackerCPU100)
* 8. [解决搜狗输入法简体/繁体切换设置不生效的问题](#-1)
* 9. [解决Ubuntu中文显示异常的问题](#Ubuntu)
* 10. [解决 Jetbrains IDE 无法切换中文输入法的问题](#JetbrainsIDE)
* 11. [解决 Linux 下 unzip 解压文件名乱码的问题](#Linuxunzip)
* 12. [Linux zip 打包文件](#Linuxzip)
* 13. [常用 vim 快捷键](#vim)
* 14. [linux 批量修改文件后缀命令](#linux-1)
* 15. [WSL2 MySQL Client](#WSL2MySQLClient)

<!-- vscode-markdown-toc-config
	numbering=true
	autoSave=true
	/vscode-markdown-toc-config -->
<!-- /vscode-markdown-toc -->##  1. <a name='linuxmint'></a>linux mint 装机
###  1.1. <a name=''></a>卸载无用软件

```
sudo apt purge rhythmbox hexchat libreoffice-* thunderbird transmission -y
```

###  1.2. <a name='-1'></a>换源&&升级

图形化界面换源即可
```
sudo apt update && sudo apt upgrade -y
```
###  1.3. <a name='-1'></a>安装常用软件

```
sudo apt install git vim tree curl fcitx fcitx-sunpinyin htop proxychains4 flameshot telegram-desktop -y
```

###  1.4. <a name='Git'></a>配置 Git
```
git config --global user.name "zjy4fun"
git config --global user.email "zjy4fun@gmail.com"
```

###  1.5. <a name='-1'></a>生成密钥
```
ssh-keygen -t rsa -C "zjy4fun@gmail.com"
```

##  2. <a name='docker'></a>安装 docker
注意 Linux Mint 的版本最好和 Ubuntu 的 LTS 的版本对应

```
sudo apt update

sudo apt -y install apt-transport-https ca-certificates curl software-properties-common

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

# linux mint21 <==> ubuntu 22.04 jammy
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu jammy stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

cat /etc/apt/sources.list.d/docker.list

sudo apt update

sudo apt install docker-ce docker-ce-cli containerd.io docker-compose-plugin

sudo usermod -aG docker $USER

newgrp docker

docker version
```
配置开机运行 docker
```
sudo systemctl enable docker.service
sudo systemctl enable containerd.service
```
> 免 root 使用 docker 相关 https://docs.docker.com/engine/install/linux-postinstall/

记得重启

**配置阿里云镜像**
```
sudo mkdir -p /etc/docker

sudo tee /etc/docker/daemon.json <<-'EOF'
{
"registry-mirrors": ["https://e3vnt17r.mirror.aliyuncs.com"]
}
EOF

sudo systemctl daemon-reload

sudo systemctl restart docker
```

**启动一个mysql测试数据库**

1. 创建目录
```bash
mkdir -p ~/env/mysql/mydir ~/env/mysql/conf ~/env/mysql/datadir ~/env/mysql/source
```
2. 创建配置文件
```bash
vim ~/env/mysql/conf/my.cnf
```

```cnf
[mysqld]
user=mysql
default-storage-engine=INNODB
character-set-server=utf8
character-set-client-handshake=FALSE
collation-server=utf8_unicode_ci
init_connect='SET NAMES utf8'
[client]
default-character-set=utf8
[mysql]
default-character-set=utf8
```

3. 创建 docker-compose.yml
```bash
vim ~/env/mysql/docker-compose.yml
```

```yml
services:
  mysql:
    image: mysql:5.7
    ports:
      - "3306:3306"
    expose:
      - "3306"
    environment:
      - MYSQL_USER=test
      - MYSQL_PASSWORD=123456
      - MYSQL_DATABASE=test
      - MYSQL_ROOT_PASSWORD=root
    volumes:
      - /home/z/env/mysql/mydir:/mydir
      - /home/z/env/mysql/datadir:/var/lib/mysql
      - /home/z/env/mysql/conf/my.cnf:/etc/my.cnf
      - /home/z/env/mysql/source:/docker-entrypoint-initdb.d
```

4. 启动 docker-compose
```bash
cd ~/env/mysql
docker compose up -d
```

**快速启动一个web server用于调试前端页面**

```shell
#!/usr/bin/env bash

# for access: http://localhost:8000
LOCAL_PORT=8000

cd "$(dirname "$0")"
WWW_DIR=`pwd`
docker run --rm -p $LOCAL_PORT:80 -v $WWW_DIR:/usr/share/nginx/html nginx:latest
```

##  3. <a name='nvm'></a>nvm设置镜像 
###  3.1. <a name='linux'></a>linux
```
export NVM_NODEJS_ORG_MIRROR=https://npmmirror.com/mirrors/node/
export NODEJS_ORG_MIRROR=https://npmmirror.com/mirrors/node/
```

###  3.2. <a name='Win'></a>Win
管理员模式
```
nvm npm_mirror https://npmmirror.com/mirrors/npm/  
nvm node_mirror https://npmmirror.com/mirrors/node/ 
```

解决 Windows Powershell 无法执行 yarn/pnpm 的问题
打开 powershell 管理员模式，执行下面的命令
```
set-ExecutionPolicy RemoteSigned
```


##  4. <a name='npm'></a>npm 设置镜像
```
npm config set registry http://mirrors.cloud.tencent.com/npm/

npm config set registry https://mirrors.huaweicloud.com/repository/npm/

npm config set registry https://registry.npmmirror.com（不推荐）

```
##  5. <a name='npm-1'></a>npm 特殊包镜像
```
npm config set sass_binary_site https://npmmirror.com/mirrors/node-sass;
npm config set electron_mirror https://npmmirror.com/mirrors/electron/;
npm config set puppeteer_download_host https://npmmirror.com/mirrors;
npm config set chromedriver_cdnurl https://npmmirror.com/mirrors/chromedriver;
npm config set operadriver_cdnurl https://npmmirror.com/mirrors/operadriver;
npm config set phantomjs_cdnurl https://npmmirror.com/mirrors/phantomjs;
npm config set selenium_cdnurl https://npmmirror.com/mirrors/selenium;
npm config set node_inspector_cdnurl https://npmmirror.com/mirrors/node-inspector;
npm config set cypress_download_mirror https://npmmirror.com/mirrors/cypress;
```

##  6. <a name='Jetbrains'></a>解决 Jetbrains 中文输入法不跟随问题

https://github.com/RikudouPatrickstar/JetBrainsRuntime-for-Linux-x64

直接替换 jbr 文件


##  7. <a name='Ubuntu20.04GnometrackerCPU100'></a>Ubuntu 20.04 Gnome 禁用 tracker 避免 CPU 100%的问题

参考：https://www.cnblogs.com/hellxz/p/12321283.html

```
gsettings set org.freedesktop.Tracker.Miner.Files crawling-interval -2
gsettings set org.freedesktop.Tracker.Miner.Files enable-monitors false
```
删除索引数据
```
tracker reset --hard
```


##  8. <a name='-1'></a>解决搜狗输入法简体/繁体切换设置不生效的问题

需要在 fcitx 的全局设置里面关掉快捷键

##  9. <a name='Ubuntu'></a>解决Ubuntu中文显示异常的问题

修改
```
/etc/fonts/conf.avail/64-language-selector-prefer.conf
```
中的配置，把SC放到第一位。

##  10. <a name='JetbrainsIDE'></a>解决 Jetbrains IDE 无法切换中文输入法的问题

Help -> Edit Custom VM Options 追加下面的内容
```
-Drecreate.x11.input.method=true
```

##  11. <a name='Linuxunzip'></a>解决 Linux 下 unzip 解压文件名乱码的问题

```
unzip -O cp936
```

##  12. <a name='Linuxzip'></a>Linux zip 打包文件

```
zip -r outputfile folder1 folder2 folder3
```

##  13. <a name='vim'></a>常用 vim 快捷键

1. 移动光标

- h  左
- j  下
- k  上
- l  右

- w  移动到下一个单词的开头
- e  移动到下一个单词的末尾
- b  移动到上一个单词的开头（和e对应）
- ^  移动到行首 
- $  移动到行尾

- gg 移动到页首
- G  移动到页尾

2. 上下翻页

- ctrl + b  向上翻页
- ctrl + f  向下翻页

3. 复制/粘贴/删除

- Y/y  复制一行
- p    粘贴
- x    删除单个字符/选中内容
- dw   从光标位置向后删除直到删除单词结束
- daw  删除光标位置所在的单词
- dd   删除一行内容
- 5dd  删除 5 行内容（光标需要放在待删除的第一行上）
- :[start],[end]d 删除指定行内容
    - .（点）-当前行。
    - $-最后一行。
    - %-所有行。
- 删除所有行
    - 要删除所有行，您可以使用代表所有行的%符号或 1，$ 范围：
        - Esc 键进入正常模式
        - 键入 %d，然后按 Enter 键以删除所有行。
4. 搜索

- /                 输入字符，n 或 N 上下搜索
- shift + *         停在某个单词的某个字符上，shift + *，即可实现搜索

5. 撤销

- u           普通模式下，撤销上一个操作
- ctrl + r    反撤销（撤销后又后悔了可以执行该命令）

6. 移动某一行代码

- :3m4        将第3行代码移动到第四行(向上移动需要注意写目标行数的上一行)

##  14. <a name='linux-1'></a>linux 批量修改文件名命令

修改后缀：
```bash
rename 's/\.js/\.jsx/' *
```

修改前缀：
```bash
for i in `ls`; do mv -f $i `echo "text_"$i`; done
```

##  15. <a name='WSL2MySQLClient'></a>WSL2 MySQL Client

需要安装 mysql-client 访问 docker 中的 mysql-server，同时需要指定 ip 和端口号
```bash
mysql -h 127.0.0.1 -P 3306 -u root -p
```

否则会出现以下错误：
```
ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/var/run/mysqld/mysqld.sock' (2)
```

原因：MySQL 会默认访问本机安装的 mysql-server
