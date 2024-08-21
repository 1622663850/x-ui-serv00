# x-ui for FreeBSD

支持多协议多用户的 xray 面板, 本版本支持FreeBSD非root安装。

# 功能介绍

- 系统状态监控
- 支持多用户多协议，网页可视化操作
- 支持的协议：vmess、vless、trojan、shadowsocks、dokodemo-door、socks、http
- 支持配置更多传输配置
- 流量统计，限制流量，限制到期时间
- 可自定义 xray 配置模板
- 支持 https 访问面板（自备域名 + ssl 证书）
- 更多高级配置项，详见面板

# 安装&升级
在安装前，请先准备好用户名，密码和两个端口（面板访问端口和流量监控端口）！
```
wget -O x-ui.sh -N --no-check-certificate https://raw.githubusercontent.com/parentalclash/x-ui-freebsd/main/x-ui.sh && chmod +x x-ui.sh && ./x-ui.sh install
```

## 手动安装&升级

1. 首先从 https://github.com/parentalclash/x-ui-freebsd/releases 下载最新的压缩包，一般选择 `amd64`架构
2. 然后将这个压缩包上传到服务器的 `/home/[username]`目录下，

> 如果你的服务器 cpu 架构不是 `amd64`，自行将命令中的 `amd64`替换为其他架构

```
cd ~
rm -rf ./x-ui
tar zxvf x-ui-freebsd-amd64.tar.gz
chmod +x x-ui/x-ui x-ui/bin/xray-freebsd-* x-ui/x-ui.sh
cp x-ui/x-ui.sh ./x-ui.sh
cd x-ui
crontab -l > x-ui.cron
echo "0 0 * * * cd $cur_dir/x-ui && cat /dev/null > x-ui.log" >> x-ui.cron
echo "@reboot cd $cur_dir/x-ui && nohup ./x-ui run > ./x-ui.log 2>&1 &" >> x-ui.cron
crontab x-ui.cron
rm x-ui.cron
nohup ./x-ui run > ./x-ui.log 2>&1 &
```

## SSL证书申请

建议使用Cloudflare 15年证书

## Tg机器人使用（开发中，暂不可使用）

此功能未经测试！

## 建议系统

- FreeBSD 14+

# 常见问题

## issue 关闭

各种小白问题看得血压很高

# 特别感谢
https://github.com/vaxilu/x-ui

# Serv00教程

Serv00.com 是一家提供免费虚拟主机服务的平台，使用 FreeBSD 的系统，提供 512MB 内存、3G 磁盘和最大 20 个进程。Serv00允许使用最多三个端口, 切无法获得root权限。X-UI面板是一个非常好用的翻墙工具，但是原版面板（https://github.com/vaxilu/x-ui）不支持FreeBSD切必须要root权限。今天介绍一个开源的改版X-UI， 可以支持FreeBSD和非root安装。

本文将使用 Serv00 提供的虚拟主机，通过安装改版的X-UI（https://github.com/parentalclash/x-ui-freebsd）面板来实现科学上网。

注意！因为Serv00限制一个账号最多能申请3个端口，X-UI面板需要两个端口(一个用于访问面板，另一个用于面板流量监测)， 所以只能再申请最多一个节点用于科学上网。

安装步骤
1. 注册 Serv00 账号
在网上有已经有很多教程和视频了， 请自行查找教程并注册开通Serv00账号

2. 下载SSH工具并成功登录到Serv00服务器
请自行查找网上教程完成

3. 开通应用运行权限
有两种方式。

方法一，SSH登录到服务器后，在命令行输入以下命令

```
devil binexec on
```


方法二，登录进管理后台（DevilWEB webpanel）

点击左边的菜单中的‘Additional services’，


在右侧窗口顶部的菜单中，点击‘Run your own applications’


在右侧下面点击绿色的‘Enable’按钮


4. 开通端口
可以一次性把三个端口(全部TCP)都开通，也有两种方式。

方法一，SSH登录到服务器后，在命令行输入以下命令三次，每次申请一个端口

```
devil port add tcp random
```
执行下面的命令来查看获得的端口号
```
devil port list
```
请记下这些端口，一会安装X-UI面板的时候会用到其中的两个，另一个将来申请节点用。

方法二，登录进管理后台（DevilWEB webpanel）

点击左边的菜单中的‘Port reservation’，


在右侧窗口顶部的菜单中，点击‘Add port’


然后请点击‘Random’按钮，点击后红叉会变成绿勾。


然后点击‘Add’按钮


重复以上步骤申请另外两个端口。

查看申请到的端口，左边的菜单中，点击‘Port reservation’，在右侧窗口顶部的菜单中，点击‘Port list’

请记下这些端口，一会安装X-UI面板的时候会用到其中的两个，另一个将来申请节点用。

5. 安装X-UI面板
SSH登录到服务器后，执行以下命令
```
cd ~ && wget -O x-ui.sh -N --no-check-certificate https://raw.githubusercontent.com/parentalclash/x-ui-freebsd/main/x-ui.sh && chmod +x x-ui.sh && ./x-ui.sh install
```
在安装过程中会提示‘出于安全考虑，安装/更新完成后需要强制修改端口与账户密码’，建议选择Y来指定用户名密码和两个端口。虽然后面也能修改，但是会相对麻烦些。

安装完成后，X-UI会自动运行。也会加入开机自启动脚本，不过Ser00有时候好像不会调用用户定义的Cronjob。

6. 通过Serv00的IP来访问你的面板。
比如IP是 192.168.0.1， 你安装面板时指定的访问端口是1234. 那你可以通过‘http://192.168.0.1:1234’来访问面板。

如何获得你的IP？

可以输入以下命令获得，结果中的第一个IP就是服务器的IP。
```
devil vhost list
```
7. 访问面板后台
SSH登录到服务器后，执行以下命令
```
cd ~ && ./x-ui.sh
```
8. 创建节点
请根据自己的需要在X-UI面板中创建节点。创建节点时，需要使用上面开通的第三个端口。
