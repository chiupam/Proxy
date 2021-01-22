## 第一步: 创建第二个容器

在命令行依次复制粘贴以下代码, 此时 Docker 的安装位置为 /docker/, 容器名为 jd2, 如果没有配置加速, 最后一行推荐使用 `evinedeng/jd:gitee`, 貌似 `-p 5679:5679 \` 会导致控制面板无法打开, 反正也不用控制面板, 管他呢

```
docker run -dit \
-v /docker/jd2/scripts:/jd/scripts \
-v /docker/jd2/config:/jd/config \
-v /docker/jd2/log:/jd/log \
-p 5679:5679 \
--name jd2 \
--hostname jd2 \
--restart always \
evinedeng/jd:github
```

对比第一次创建容器的代码, 有些许不同, 第一次容器名为 jd

```
docker run -dit \
-v /docker/jd/scripts:/jd/scripts \
-v /docker/jd/config:/jd/config \
-v /docker/jd/log:/jd/log \
-p 5678:5678 \
--name jd \
--hostname jd \
--restart always \
evinedeng/jd:github
```

## 第二步: 启动 Docker 服务

`service docker start`

## 第三步: 查看创建日志

`docker logs -f jd2`

> 直到出现容器启动成功...字样才代表启动成功, 按 Ctrl+C 退出查看日志

## 第四步: 编辑文件

```
cd /docker/jd2/config
```

`vim auth.json`
> auth.json 文件是用户账号和密码, 修改完按 ESC 输入 :wq 保存并退出

`vim congif.sh`
> config.sh 文件是脚本变量设置, 按文件内说明即可, 修改完按 ESC 输入 :wq 保存并退出

`crontab.list`
> crontab.list 文件是脚本运行时间, 按文件内格式编写修改完按 ESC 输入 :wq 保存并退出

## 其他

### 添加除 lxk 以外的脚本

**以下代码中 Docker 安装位置为 /docker/**

如果脚本可以在 node.js 上执行, 将此脚本(.js)放在 /docker/jd2/scripts 下即可. 

比如文件名为 test.js, 编辑 crontab.list 文件添加定时任务：

```
15 10 * * * bash jd2 test     # 如果不需要准时运行或RandemDelay未设置
15 10 * * * bash jd2 test now # 如果设置了RandemDelay但又需要它准时运行
```

*注意：额外添加的脚本不能以 “jd_”、“jr_”、“jx_” 开头，以 “jd_”、“jr_”、“jx_” 开头的任务如果不在 lxk0301 仓库中会被删除！*

### 如果额外脚本需要使用 lxk 仓库文件(仍未琢磨出来)

~~如果此脚本需要使用 LXK 仓库中的 sendNotify.js 来发送通知，或者用到 jdCookie.js 来处理Cookie~~

~~将此脚本上传至 /docker/jd2/script/ 文件夹下, 然后执行以下代码~~

~~`docker cp /docker/test.js jd:/jd/scripts`~~

### 进入容器并查看挂机日志

先进容器

`docker exec -it jd2 /bin/bash`

后看日志

`pm2 monit`

最后退出

`exit`

### 手动 git pull 更新脚本

`docker exec -it jd2 bash git_pull`

> 顺便也执行以下这段代码吧 `npm install got`

### 手动删除指定时间以前的旧日志

`docker exec -it jd2 bash rm_log`

### 手动导出所有互助码

`docker exec -it jd2 bash export_sharecodes`

### 手动执行一次某个脚本

以执行名为 jd_fruit.js 的文件为例 

`docker exec -it jd2 bash jd.sh jd_fruit now`

其中 `exec` 后面的 `jd2` 为容器名, `bash` 后面的 `jd` 为命令名, `xxx` 为 `lxk` 的脚本名