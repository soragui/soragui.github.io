---
layout: post
image: assets/images/ssser.jpg
comments: true
author: sal
categories: shadowsocks
---

>虽然API接口已经安装成功，但我还是不知道如何使用，不过还是要记录一下安装配置的过程，这也算是小白的一点成长。

## 序言
说一说我为什么要在服务器上安装这个API，在服务器上成功安装shadowsocks之后，当然这都是一年前的事情了，我就想能不能用一个网页来配置用户信息之类的东西，必须承认我对后端、JS什么的一点也不懂所以就在Github上面找啊找，终于找到了一个[ss manager](https://github.com/shadowsocks/shadowsocks-manager),很开心以为找到了解决办法，不过我还是高估了我自己的能力，看了好几遍有说明、有例子但还是一头雾水，所以这个愿望一直都没能实现。直到最近，才发现另外一个大牛用JS写的[API](https://github.com/shadowsocks/shadowsocks-hub-api)才重新燃起了我的希望，没想到居然成功了，所以今天就记录一下抒发一下内心激动的心情。

## 开始
不得不说,这个API的文档写的很完整，而且请求格式很清楚明白，反正我一看就懂，安装过程也是很清晰。这里我就在复习一遍，说明一下我服务器正好是Ubuntu 16.04,用户为root，其中Node和MySQL已经配置完整，所以信心满满。

```bash
//下载 API 接口
cd ~
git clone https://github.com/shadowsocks/shadowsocks-hub-api.git
cd shadowsocks-hub-api
npm i

//新建数据库
mysql -u root -p
CREATE DATABASE sshub;

//新建 .env 配置文件
vim .env

//然后就开始安装了
knex migrate:latest --env production

//发现没有 knex 命令 安装一下
npm i -g knex

//配置 certificate
openssl req -nodes -x509 -newkey rsa:4096 -keyout server.key -out server.cert -days 365

//一切就绪 然后就是 启动测试
node api.js

```

发现一切都很正常，可以使用API了，不过我才发现这个只是一个API并没有什么网页界面，唉我决定了写一个APP获取...这个也是下一步目标了。

## 完善
刚刚启动的时候是一个问题，因为没有配置成服务程序，也就是开机自启动的脚本，于是就Google了一下方法，配置了如下启动文件：

```bash
//修改可执行权限
chmod +x api.js

//编写服务文件
vim /etc/systemd/system/sshubapi.service

//内容如下
[Unit]
Description=shadowsocks hub API

[Service]
PIDFile=/tmp/sshubapi.pid
User=root
Group=
Restart=always
KillSignal=SIGQUIT
WorkingDirectory=/root/shadowsocks-hub-api
ExecStart=/usr/bin/node /root/shadowsocks-hub-api/api.js

[Install]
WantedBy=multi-user.target

//保持退出 尝试启动
systemctl start sshubapi.service

//查看状态
systemctl status sshubapi.service
● sshubapi.service - shadowsocks hub API
   Loaded: loaded (/etc/systemd/system/sshubapi.service; enabled; vendor preset: enabled)
   Active: active (running) since Fri 2018-07-27 19:43:16 CST; 29min ago
 Main PID: 17890 (node)
    Tasks: 10 (limit: 1133)
   Memory: 56.5M
   CGroup: /system.slice/sshubapi.service
           └─17890 /usr/bin/node /root/shadowsocks-hub-api/api.js

Jul 27 19:43:16 kinezhou.cn systemd[1]: Started shadowsocks hub API.
Jul 27 19:43:19 kinezhou.cn node[17890]: listening on port 8000

```

一切就绪了，终于配置好API，不过这似乎只是一个开始。下一步就是看文档然后写一个移动端APP来管理shadowsocks用户了--加油。

## 参考链接

[node js Service Ubuntu](https://hackernoon.com/making-node-js-service-always-alive-on-ubuntu-server-e20c9c0808e4)
