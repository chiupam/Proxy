## 第一步: 安装 Docker

1. 如果之前安装过 Docker 需要先卸载

`sudo yum remove docker docker-common docker-selinux docker-engine`

2. 安装依赖

`sudo yum install -y yum-utils device-mapper-persistent-data lvm2`

3. 下载 repo 文件(CentOS)

`wget -O /etc/yum.repos.d/docker-ce.repo https://download.docker.com/linux/centos/docker-ce.repo`

4. 把软件仓库地址替换为 TUNA

`sudo sed -i 's+download.docker.com+mirrors.bfsu.edu.cn/docker-ce+' /etc/yum.repos.d/docker-ce.repo`

5. 最后安装

```
sudo yum makecache fast
sudo yum install docker-ce
```

6. 顺带

`npm install got`

## 第二步: 创建容器

在此笔记中 Docker 的安装位置为 /usr/docker/ (可改)

```
docker run -dit \
-v /usr/docker/jd/scripts:/jd/scripts \
-v /usr/docker/jd/config:/jd/config \
-v /usr/docker/jd/log:/jd/log \
-p 5678:5678 \
--name jd \
--hostname jd \
--restart always \
evinedeng/jd:github
```

## 第三步: 查看创建日志

`docker logs -f jd`

> 直到出现容器启动成功...字样才代表启动成功, 按 Ctrl+C 退出查看日志

## 第四步: 编辑文件

```
cd /usr/docker/jd/config
```

`vim auth.json`
> auth.json 文件是用户账号和密码, 修改完按 ESC 输入 :wq 保存并退出

`vim congif.sh`
> config.sh 文件是脚本变量设置, 按文件内说明即可, 修改完按 ESC 输入 :wq 保存并退出

`crontab.list`
> crontab.list 文件是脚本运行时间, 按文件内格式编写修改完按 ESC 输入 :wq 保存并退出
