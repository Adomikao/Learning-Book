Nginx ("engine x") 是一个高性能的 HTTP 和反向代理服务器，也是一个 IMAP/POP3/SMTP 代理服务器。Nginx 是由 Igor Sysoev 为俄罗斯著名的 Rambler.ru 站点开发的，第一个公开版本 0.1.0 发布于 2004 年 10 月 4 日。其将源代码以类 BSD 许可证的形式发布，因它的稳定性、丰富的功能集、示例配置文件和低系统资源的消耗而闻名。<br>

由于 Nginx 使用基于事件驱动的架构，能够并发处理百万级别的 TCP 连接，高度模块化的设计和自由的许可证使得扩展 Nginx 功能的第三方模块层出不穷。因此其作为 Web 服务器被广泛应用到大流量的网站上，包括淘宝、腾讯、新浪、京东等访问量巨大的网站。<br>

## 1. nginx 的特点

- 处理响应请求很快: 在正常的情况下，单次请求会得到更快的响应。在高峰期，Nginx 可以比其它的 Web 服务器更快的响应请求。
- 高并发连接: 在互联网快速发展，互联网用户数量不断增加的今天，一些大公司、网站都需要面对高并发请求，如果有一个能够在峰值顶住 10 万以上并发请求的 Server，肯定会得到大家的青睐。理论上，Nginx 支持的并发连接上限取决于你的内存，10 万远未封顶。
- 低的内存消耗: 在一般的情况下，10000 个非活跃的 HTTP Keep-Alive 连接在 Nginx 中仅消耗 2.5MB 的内存，这也是 Nginx 支持高并发连接的基础。
- 具有很高的可靠性：Nginx 是一个高可靠性的 Web 服务器，这也是我们为什么选择 Nginx 的基本条件，现在很多的网站都在使用 Nginx，足以说明 Nginx 的可靠性。高可靠性来自其核心框架代码的优秀设计、模块设计的简单性，并且这些模块都非常的稳定。
- 高扩展性: Nginx 的设计极具扩展性，它完全是由多个不同功能、不同层次、不同类型且耦合度极低的模块组成。这种设计造就了 Nginx 庞大的第三方模块。
- 热部署: master 管理进程与 worker 工作进程的分离设计，使得 Nginx 具有热部署的功能，可以在 7 × 24 小时不间断服务的前提下，升级 Nginx 的可执行文件。也可以在不停止服务的情况下修改配置文件，更换日志文件等功能。
- 自由的 BSD 许可协议: BSD 许可协议不只是允许用户免费使用 Nginx，也允许用户修改 Nginx 源码，还允许用户用于商业用途。


## 2. nginx 的安装和使用

1> 使用 Homebrew 安装 nginx

```
sudo brew install nginx
```

2> 启动 nginx

```
sudo nginx
```

3> 重启 nginx

```
sudo nginx -s reload
```

4> Nginx的常用配置
- 核心配置文件nginx.conf
```
/usr/local/etc/nginx/nginx.conf （配置文件路径）
```
- nginx配置文件主要分为六个区域
    - main(全局设置)
    - events(nginx工作模式)
    - http(http设置)
    - sever(主机设置)
    - location(URL匹配)
    - upstream(负载均衡服务器设置)

- server模块
```
 server {
    listen       8080;             #端口配置
    server_name  localhost;        #域名配置

    #charset koi8-r;

    #access_log  logs/host.access.log  main;

    ······
}
```
- location模块
```
location / {    
    root   html;
    index  index.html index.htm;
}
```

location /表示匹配访问根目录。
root 指令用于指定访问根目录时，虚拟主机的 web 目录，这个目录可以是相对路径（相对路径是相对于nginx的安装目录）。也可以是绝对路径。默认的 html 读取路径是:<br>
  `/usr/local/Cellar/nginx/[version]/html`<br>
这里的 html 文件夹实际上是一个替身（快捷方式），实际的默认文件位置是在
默认文件路径 `/usr/local/var/www `
