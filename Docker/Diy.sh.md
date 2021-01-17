## 添加 shylocks 脚本

### 编辑 config.sh 文件

```
cd /usr/docker/jd/config
vim config.sh
```

> 按 e 进入编辑模式

把 `EnableExtraShell=""` 改成 `EnableExtraShell="true"`

按 ESC 输入 :wq 保存并退出

### 新建一个 diy.sh 文件并编写

先新建一个 diy.sh 文件, 或者直接下面那行代码

`touch diy.sh`

(或者直接新建并)进入 diy.sh 编辑

`vim diy.sh`

输入以下代码(注意查看并修改第三行代码)

```
#!/usr/bin/env bash

my_scripts_list="jd_gyec.js jd_mh.js jd_ms.js jd_super_coupon.js jd_live_redrain2.js "  # 你想下载的js文件清单
scripts_base_url=https://raw.fastgit.org/shylocks/Loon/main/   # raw文件的基础网址(国内镜像源)

cd $ScriptsDir   # 在 git_pull.sh 中已经定义 ScriptsDir 此变量，diy.sh 由 git_pull.sh 调用，因此可以直接使用此变量
for js in $my_scripts_list
do
  # 下载my_scripts_list中的每个js文件，重命名增加前缀"shylocks_"，增加后缀".new"
  echo -e "更新 $js ...\n"
  wget -q --no-check-certificate $scripts_base_url$js -O shylocks_$js.new

  # 如果上一步下载没问题，才去掉后缀".new"，如果上一步下载有问题，就保留之前正常下载的版本
  if [ $? -eq 0 ]; then
    mv -f shylocks_$js.new shylocks_$js
    echo -e "更新 $js 完成...\n"
  else
    [ -f shylocks_$js.new ] && rm -f shylocks_$js.new
    echo -e "更新 $js 失败，使用上一次正常的版本..."
  fi
done
```

按 ESC 输入 :wq 保存并退出

### 拉取文件

`docker exec -it jd bash git_pull`

### 测试运行脚本

这里以测试 jd_gyec 脚本为例子(**shylocks_很重要**)

`docker exec -it jd bash jd.sh shylocks_jd_gyec now`

看到输出正常日志即可

### 编写 cronbat.list 文件

```
cd /usr/docker/jd/config/
vim crontab.list
```

按 E 进入编辑模式, 例如(**shylocks_很重要**)
> `30 * * * * bash jd shylocks_jd_gyec`
