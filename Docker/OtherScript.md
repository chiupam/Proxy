## 添加除 lxk 以外的脚本

以下代码中 Docker 安装位置为 /docker/

如果脚本可以在 node.js 上执行, 将此脚本(.js)放在 /docker/jd/scripts 下即可. 

比如文件名为 test.js, 编辑 crontab.list 文件添加定时任务：

```
15 10 * * * bash jd test     # 如果不需要准时运行或RandemDelay未设置
15 10 * * * bash jd test now # 如果设置了RandemDelay但又需要它准时运行
```

*注意：额外添加的脚本不能以 “jd_”、“jr_”、“jx_” 开头，以 “jd_”、“jr_”、“jx_” 开头的任务如果不在 lxk0301 仓库中会被删除！*

### 如果额外脚本需要使用 lxk 仓库文件

如果此脚本需要使用 LXK 仓库中的 sendNotify.js 来发送通知，或者用到 jdCookie.js 来处理Cookie

将此脚本上传至 /docker/jd/script/ 文件夹下, 然后执行以下代码

`docker cp /docker/test.js jd:/jd/scripts`
