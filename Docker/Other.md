### 重置控制面板用户名和密码

`docker exec -it jd bash jd resetpwd`

### 添加除 lxk 以外的脚本

**以下代码中 Docker 安装位置为 /usr/docker/**

如果脚本可以在 node.js 上执行, 将此脚本(.js)放在 /usr/docker/jd/scripts 下即可. 

比如文件名为 test.js, 编辑 crontab.list 文件添加定时任务：

```
15 10 * * * bash jd test     # 如果不需要准时运行或RandemDelay未设置
15 10 * * * bash jd test now # 如果设置了RandemDelay但又需要它准时运行
```

*注意：额外添加的脚本不能以 “jd_”、“jr_”、“jx_” 开头，以 “jd_”、“jr_”、“jx_” 开头的任务如果不在 lxk0301 仓库中会被删除！*

### 如果额外脚本需要使用 lxk 仓库文件

如果此脚本需要使用 LXK 仓库中的 sendNotify.js 来发送通知，或者用到 jdCookie.js 来处理Cookie

将此脚本上传至 /usr/docker/jd/script/ 文件夹下, 然后执行以下代码

`docker cp /usr/docker/test.js jd:/jd/scripts`

### 进入容器并查看挂机日志

先进容器

`docker exec -it jd /bin/bash`

后看日志

`pm2 monit`

最后退出

`exit`

### 手动 git pull 更新脚本

`docker exec -it jd bash git_pull`

> 顺便也执行以下这段代码吧 `npm install got`

### 手动删除指定时间以前的旧日志

`docker exec -it jd bash rm_log`

### 手动导出所有互助码

`docker exec -it jd bash export_sharecodes`

### 手动执行一次某个脚本

以执行名为 jd_fruit.js 的文件为例 

`docker exec -it jd bash jd.sh jd_fruit now`

其中 `exec` 后面的 `jd` 为容器名, `bash` 后面的 `jd` 为命令名, `xxx` 为 `lxk` 的脚本名

> 报错的话可能需要用到这两段代码
>> `docker exec -it jd bash git_pull`
>> `npm install got`
