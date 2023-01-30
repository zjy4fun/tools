## docker

### ali-image

```bash
sudo mkdir -p /etc/docker

sudo tee /etc/docker/daemon.json <<-'EOF'
{
"registry-mirrors": ["https://e3vnt17r.mirror.aliyuncs.com"]
}
EOF

sudo systemctl daemon-reload

sudo systemctl restart docker
```

### mysql

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

### web server

> 快速启动一个web server用于调试前端页面

```shell
#!/usr/bin/env bash

# for access: http://localhost:8000
LOCAL_PORT=8000

cd "$(dirname "$0")"
WWW_DIR=`pwd`
docker run --rm -p $LOCAL_PORT:80 -v $WWW_DIR:/usr/share/nginx/html nginx:latest
```

