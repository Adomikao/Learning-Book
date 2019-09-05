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

## 2. location 匹配规则
### 1> 语法规则

`
location [=|~|~*|^~] /uri/ { … }
`

|模式	 | 含义     |
|:----- |:------   |
|location = /uri  |	= 表示精确匹配，只有完全匹配上才能生效 |
|location ^~ /uri	| ^~ 开头对URL路径进行前缀匹配，并且在正则之前。|
|location ~ pattern	| 开头表示区分大小写的正则匹配 |
|location ~* pattern	| 开头表示不区分大小写的正则匹配 |
|location /uri  |	不带任何修饰符，也表示前缀匹配，但是在正则匹配之后 |
|location /	| 通用匹配，任何未匹配到其它location的请求都会匹配到，相当于switch中的 default|

前缀匹配时，Nginx 不对 url 做编码，因此请求为 /static/20%/aa，可以被规则 ^~ /static/ /aa 匹配到（注意是空格）<br>

多个 location 配置的情况下匹配顺序为:
- 首先精确匹配 =
- 其次前缀匹配 ^~
- 其次是按文件中顺序的正则匹配
- 然后匹配不带任何修饰的前缀匹配。
- 最后是交给 / 通用匹配
- 当有匹配成功时候，停止匹配，按当前匹配规则处理请求

> 注意：前缀匹配，如果有包含关系时，按最大匹配原则进行匹配。比如在前缀匹配：location /dir01 与 location /dir01/dir02，如有请求 http://localhost/dir01/dir02/file 将最终匹配到 location /dir01/dir02

例子，有如下匹配规则：
```
location = / {
   echo "规则A";
}
location = /login {
   echo "规则B";
}
location ^~ /static/ {
   echo "规则C";
}
location ^~ /static/files {
    echo "规则X";
}
location ~ \.(gif|jpg|png|js|css)$ {
   echo "规则D";
}
location ~* \.png$ {
   echo "规则E";
}
location /img {
    echo "规则Y";
}
location / {
   echo "规则F";
}
```

那么产生的效果如下：

- 访问根目录 /，比如 http://localhost/ 将匹配 规则A
- 访问 http://localhost/login 将匹配 规则B，http://localhost/register 则匹配 规则F
- 访问 http://localhost/static/a.html 将匹配 规则C
- 访问 http://localhost/static/files/a.exe 将匹配 规则X，虽然 规则C 也能匹配到，但因为最大匹配原则，最终选中了 规则X。你可以测试下，去掉规则 X ，则当前 URL 会匹配上 规则C。
- 访问 http://localhost/a.gif, http://localhost/b.jpg 将匹配 规则D 和 规则 E ，但是 规则 D 顺序- 优先，规则 E 不起作用，而 http://localhost/static/c.png 则优先匹配到 规则 C
- 访问 http://localhost/a.PNG 则匹配 规则 E ，而不会匹配 规则 D ，因为 规则 E 不区分大小写。
- 访问 http://localhost/img/a.gif 会匹配上 规则D,虽然 规则Y 也可以匹配上，但是因为正则匹配优先，而- 忽略了 规则Y。
- 访问 http://localhost/img/a.tiff 会匹配上 规则Y。

访问 http://localhost/category/id/1111 则最终匹配到规则 F ，因为以上规则都不匹配，这个时候应该是 Nginx 转发请求给后端应用服务器，比如 FastCGI（php），tomcat（jsp），Nginx 作为反向代理服务器存在。

所以实际使用中，笔者觉得至少有三个匹配规则定义，如下： 
```
# 直接匹配网站根，通过域名访问网站首页比较频繁，使用这个会加速处理，官网如是说。
# 这里是直接转发给后端应用服务器了，也可以是一个静态首页
# 第一个必选规则
location = / {
    proxy_pass http://tomcat:8080/index
}

# 第二个必选规则是处理静态文件请求，这是 nginx 作为 http 服务器的强项
# 有两种配置模式，目录匹配或后缀匹配，任选其一或搭配使用
location ^~ /static/ {
    root /webroot/static/;
}
location ~* \.(gif|jpg|jpeg|png|css|js|ico)$ {
    root /webroot/res/;
}

# 第三个规则就是通用规则，用来转发动态请求到后端应用服务器
# 非静态文件请求就默认是动态请求，自己根据实际把握
# 毕竟目前的一些框架的流行，带.php、.jsp后缀的情况很少了
location / {
    proxy_pass http://tomcat:8080/
}
```

### 2> rewrite 语法

- last – 基本上都用这个 flag
- break – 中止 rewrite，不再继续匹配
- redirect – 返回临时重定向的 HTTP 状态 302
- permanent – 返回永久重定向的 HTTP 状态 301

1、下面是可以用来判断的表达式：
```
-f 和 !-f 用来判断是否存在文件
-d 和 !-d 用来判断是否存在目录
-e 和 !-e 用来判断是否存在文件或目录
-x 和 !-x 用来判断文件是否可执行
```

2、下面是可以用作判断的全局变量
```
例：http://localhost:88/test1/test2/test.php?k=v
$host：localhost
$server_port：88
$request_uri：/test1/test2/test.php?k=v
$document_uri：/test1/test2/test.php
$document_root：D:\nginx/html
$request_filename：D:\nginx/html/test1/test2/test.php
```

### 3> redirect 语法
```
server {
    listen 80;
    server_name start.igrow.cn;
    index index.html index.php;
    root html;
    if ($http_host !~ "^star\.igrow\.cn$") {
        rewrite ^(.*) http://star.igrow.cn$1 redirect;
    }
}
```

### 4> 防盗链
```
location ~* \.(gif|jpg|swf)$ {
    valid_referers none blocked start.igrow.cn sta.igrow.cn;
    if ($invalid_referer) {
       rewrite ^/ http://$host/logo.png;
    }
}
```

### 5> 根据文件类型设置过期时间
```
location ~* \.(js|css|jpg|jpeg|gif|png|swf)$ {
    if (-f $request_filename) {
        expires 1h;
        break;
    }
}
```

### 6> 禁止访问某个目录
```
location ~* \.(txt|doc)${
    root /data/www/wwwroot/linuxtone/test;
    deny all;
}
```



