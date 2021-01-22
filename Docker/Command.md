## 基本命令

此笔记中的容器名为 jd 或者 jd2

### 启动 Docker 服务

`service docker start`

### 产看创建日志

`docker logs -f <容器名>`

即

`docker logs -f jd`

### 查看所有容器情况

`docker ps -a`

### 启动、停止、杀死、删除容器

```
docker start <容器名>
docker stop <容器名>
docker kill <容器名>
docker rm -f <容器名>
```

即

```
docker start jd
docker stop jd
docker kill jd
docker rm -f jd
```

### 重置控制面板用户名和密码

`docker exec -it jd2 bash jd resetpwd`

### 手动拉取脚本

`docker exec -it <容器名> bash git_pull`

即

`docker exec -it jd bash git_pull`

### 手动执行脚本

- 延迟 RandomDelay 秒后执行脚本, RandomDelay 在 config.sh 中设置

`docker exec -it <容器名> bash jd.sh <脚本名(不带.js)>`

即

`docker exec -it jd bash jd.sh jd_fruit`

- 立即执行脚本

`docker exec -it <容器名> bash jd.sh <脚本名(不带.js)> now`

即

`docker exec -it jd bash jd.sh jd_fruit now`

#### 拉取失败时

`npm install got`

### 进入容器查看挂机日志

`docker exec -it <容器名> /bin/bash`

即

`docker exec -it jd /bin/bash`

然后

`pm2 monit`

退出

`exit`

