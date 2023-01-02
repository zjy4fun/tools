## linux mint 装机
### 卸载无用软件

```
sudo apt purge rhythmbox hexchat libreoffice-* thunderbird transmission -y
```

### 换源&&升级

图形化界面换源即可
```
sudo apt update && sudo apt upgrade -y
```
### 安装常用软件

```
sudo apt install git vim tree curl fcitx fcitx-sunpinyin htop proxychains4 flameshot telegram-desktop -y
```

### 配置 Git
```
git config --global user.name "zjy4fun"
git config --global user.email "zjy4fun@gmail.com"
```

### 生成密钥
```
ssh-keygen -t rsa -C "zjy4fun@gmail.com"
```

## 安装 docker
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

## nvm设置镜像 
### linux
```
export NVM_NODEJS_ORG_MIRROR=https://npmmirror.com/mirrors/node/
export NODEJS_ORG_MIRROR=https://npmmirror.com/mirrors/node/
```

### Win
管理员模式
```
nvm npm_mirror https://npmmirror.com/mirrors/npm/  
nvm node_mirror https://npmmirror.com/mirrors/node/ 
```


## npm 设置镜像
```
npm config set registry http://mirrors.cloud.tencent.com/npm/

npm config set registry https://mirrors.huaweicloud.com/repository/npm/

npm config set registry https://registry.npmmirror.com（不推荐）

```
## npm 特殊包镜像
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

## 解决 Jetbrains 中文输入法不跟随问题

https://github.com/RikudouPatrickstar/JetBrainsRuntime-for-Linux-x64

直接替换 jbr 文件
