


# crontab 设置DDNS自动任务
项目地址: [https://github.com/YaoYinYing/HZAU_DDNS][1]

### 1. 新建cron定时任务脚本 ddns_task.sh

``` bash
vim ddns_task.sh

```

写入以下信息，注意更改变量name的值

``` vim
#!/bin/bash

# Define a sub domain name here
name=test

# default setting for DDNS, do not edit it.
PATH=/bin:/sbin:/usr/bin:/usr/sbin:/usr/local/bin:/usr/local/sbin:~/bin
export PATH
curl https://your.domain.host/HZAU_DDNS/api/index.py -X POST -d "token=token&name="$name"&domain_name=domain"

```

### 2. 赋予执行权限：

``` bash
chmod +x ./ddns_task.sh
```
尝试运行，正确将返回以下信息：

``` html_response
{'200.1 OK': 'records created'}
```
或者
``` html_response
{'200.2 OK': 'records unchanged'}
```
其他信息参照项目文档。

### 3. 写入crontab任务计划

``` bash
crontab -e
```

此时应进入编辑器编辑任务列表；若初次编辑，需要按照提示设置编辑器，这里用vim-basic；写入以下内容

``` vim
# DDNS TASK 
* * * * *  /etc/profile;/bin/bash /path-to-task-script/ddns_task.sh>/dev/null 2>&1
```

### 4. 检查是否写入完成

``` bash
crontab -l
```

### 5. 任务运行情况：
http://your.domain.host/HZAU_DDNS/api/test_tool/update_count_show.py

若在10分钟之内可观测到DDNS运行计数增加，则配置无误。

[1]: https://github.com/YaoYinYing/HZAU_DDNS
