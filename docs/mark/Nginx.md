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

## 3. location 匹配规则
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

## 4. 静态文件服务
最简单的本地静态文件服务配置示例：
```
server {
        listen       80;
        server_name www.test.com;
        charset utf-8;
        root   /data/www.test.com;
        index  index.html index.htm;
       }
```
就这些？恩，就这些！如果只是提供简单的对外静态文件，它真的就可以用了。可是他不完美，远远没有发挥 Nginx 的半成功力，为什么这么说呢，看看下面的配置吧，为了大家看着方便，我们把每一项的作用都做了注释。

```
http {
    # 这个将为打开文件指定缓存，默认是没有启用的，max 指定缓存数量，
    # 建议和打开文件数一致，inactive 是指经过多长时间文件没被请求后删除缓存。
    open_file_cache max=204800 inactive=20s;

    # open_file_cache 指令中的inactive 参数时间内文件的最少使用次数，
    # 如果超过这个数字，文件描述符一直是在缓存中打开的，如上例，如果有一个
    # 文件在inactive 时间内一次没被使用，它将被移除。
    open_file_cache_min_uses 1;

    # 这个是指多长时间检查一次缓存的有效信息
    open_file_cache_valid 30s;

    # 默认情况下，Nginx的gzip压缩是关闭的， gzip压缩功能就是可以让你节省不
    # 少带宽，但是会增加服务器CPU的开销哦，Nginx默认只对text/html进行压缩 ，
    # 如果要对html之外的内容进行压缩传输，我们需要手动来设置。
    gzip on;
    gzip_min_length 1k;
    gzip_buffers 4 16k;
    gzip_http_version 1.0;
    gzip_comp_level 2;
    gzip_types text/plain application/x-javascript text/css application/xml;

    server {
            listen       80;
            server_name www.test.com;
            charset utf-8;
            root   /data/www.test.com;
            index  index.html index.htm;
           }
}
```

## 5. 日志
Nginx 日志主要有两种：access_log(访问日志) 和 error_log(错误日志)。

1>  access_log 访问日志<br>

access_log 主要记录客户端访问 Nginx 的每一个请求，格式可以自定义。通过 access_log 你可以得到用户地域来源、跳转来源、使用终端、某个 URL 访问量等相关信息。<br>

log_format 指令用于定义日志的格式，语法: log_format name string; 其中 name 表示格式名称，string 表示定义的格式字符串。log_format 有一个默认的无需设置的组合日志格式。<br>

> 默认的无需设置的组合日志格式

```
log_format combined '$remote_addr - $remote_user  [$time_local]  '
                    ' "$request"  $status  $body_bytes_sent  '
                    ' "$http_referer"  "$http_user_agent" ';
```
access_log 指令用来指定访问日志文件的存放路径（包含日志文件名）、格式和缓存大小，语法：access_log path [format_name [buffer=size | off]]; 其中 path 表示访问日志存放路径，format_name 表示访问日志格式名称，buffer 表示缓存大小，off 表示关闭访问日志。

> log_format 使用示例：在 access.log 中记录客户端 IP 地址、请求状态和请求时间

```
log_format myformat '$remote_addr  $status  $time_local';
access_log logs/access.log  myformat;
```
需要注意的是：log_format 配置必须放在 http 内，否则会出现警告。Nginx 进程设置的用户和组必须对日志路径有创建文件的权限，否则，会报错。<br>

2> error_log 错误日志<br>

error_log 主要记录客户端访问 Nginx 出错时的日志，格式不支持自定义。通过查看错误日志，你可以得到系统某个服务或 server 的性能瓶颈等。因此，将日志利用好，你可以得到很多有价值的信息。

error_log 指令用来指定错误日志，语法: error_log path [level]; 其中 path 表示错误日志存放路径，level 表示错误日志等级，日志等级包括 debug、info、notice、warn、error、crit、alert、emerg，从左至右，日志详细程度逐级递减，即 debug 最详细，emerg 最少，默认为 error。

注意：error_log off 并不能关闭错误日志记录，此时日志信息会被写入到文件名为 off 的文件当中。如果要关闭错误日志记录，可以使用如下配置：

> Linux 系统把存储位置设置为空设备

```
error_log /dev/null;

http {
    # ...
}
```

> Windows 系统把存储位置设置为空设备

```
error_log nul;

http {
    # ...
}
```

## 6. Nginx 陷阱和常见错误

> 原文： https://www.nginx.com/resources/wiki/start/topics/tutorials/config_pitfalls/

不管是新手还是老用户，都可能会掉到一个陷阱中去。下面我们会列出一些我们经常看到，和 经常需要解释如何解决的问题。在 Freenode 的 Nginx IRC 频道中，我们频繁的看到这些问题出现。<br>

**本指南说**

最经常看到的是，有人从一些其他的指南中，尝试拷贝、粘贴一个配置片段。并不是说其他所有的指南都是错的，但是里面错误的比例很可怕。即使是在 Linode 库中也有质量较差的信息，一些 Nginx 社区成员曾经徒劳的试图去纠正。 <br>

本指南的文档，是社区成员所创建和审查，他们直接和所有类型的 Nginx 用户在一起工作。 这个特定的文档之所以存在，是因为社区成员看到有大量普遍和重复出现的问题。<br>

**我的问题没有被列出来**

在这里你没有看到和你具体问题相关的东西。也许我们并没有解决你经历的具体问题。 不要只是大概浏览下这个网页，也不要假设你是无意才找到这里的。你找到这里，是因为这里列出了你做错的一些东西。<br>

在许多问题上，当涉及到支持很多用户，社区成员不希望去支持破碎的配置。所以在提问求助前，先修复你的配置。 通读这个文档来修复你的配置，不要只是走马观花。

### 1. chmod 777

永远不要 使用 777，这可能是一个漂亮的数字，有时候可以懒惰的解决权限问题， 但是它同样也表示你没有线索去解决权限问题，你只是在碰运气。 你应该检查整个路径的权限，并思考发生了什么事情。

要轻松的显示一个路径的所有权限，你可以使用：

```
 namei -om /path/to/check
```

糟糕的配置：

```
server {
    server_name www.example.com;
    location / {
        root /var/www/Nginx -default/;
        # [...]
      }
    location /foo {
        root /var/www/Nginx -default/;
        # [...]
    }
    location /bar {
        root /var/www/Nginx -default/;
        # [...]
    }
}
```

这个是能工作的。把 root 放在 location 区块里面会工作，但并不是完全有效的。 错就错在只要你开始增加其他的 location 区块，就需要给每一个 location 区块增加一个 root。 如果没有添加，就会没有 root。让我们看下正确的配置。<br>

推荐的配置：

```
server {
    server_name www.example.com;
    root /var/www/Nginx -default/;
    location / {
        # [...]
    }
    location /foo {
        # [...]
    }
    location /bar {
        # [...]
    }
}
```

### 2.重复的 index 指令

糟糕的配置：

```
http {
    index index.php index.htm index.html;
    server {
        server_name www.example.com;
        location / {
            index index.php index.htm index.html;
            # [...]
        }
    }
    server {
        server_name example.com;
        location / {
            index index.php index.htm index.html;
            # [...]
        }
        location /foo {
            index index.php;
            # [...]
        }
    }
}
```

为什么重复了这么多行不需要的配置呢？简单的使用“ index ”指令一次就够了。只需要把它放到 http {} 区块里面，下面的就会继承这个配置。<br>

推荐的配置：

```
http {
    index index.php index.htm index.html;
    server {
        server_name www.example.com;
        location / {
            # [...]
        }
    }
    server {
        server_name example.com;
        location / {
            # [...]
        }
        location /foo {
            # [...]
        }
    }
}
```

### 3.使用 if

这里篇幅有限，只介绍一部分使用 if 指令的陷阱。更多陷阱你应该点击看看邪恶的 if 指令。 我们看下 if 指令的几个邪恶的用法。

> 注意看这里：[邪恶的 if 指令](https://xwsoul.com/posts/761)

**用 if 判断 Server Name**

糟糕的配置：

```
server {
    server_name example.com *.example.com;
        if ($host ~* ^www\.(.+)) {
            set $raw_domain $1;
            rewrite ^/(.*)$ $raw_domain/$1 permanent;
        }
        # [...]
    }
}
```
这个配置有三个问题。首先是 if 的使用, 为啥它这么糟糕呢? 你有阅读邪恶的 if 指令吗? 当 Nginx 收到无论来自哪个子域名的何种请求, 不管域名是 www.example.com 还是 example.com，这个 if 指令 总是 会被执行。 因此 Nginx 会检查 每个请求 的 Host header，这是十分低效的。 你应该避免这种情况，而是使用下面配置里面的两个 server 指令。<br>

推荐的配置：

```
 server {
    server_name www.example.com;
    return 301 $scheme://example.com$request_uri;
}
server {
    server_name example.com;
    # [...]
}
```

除了增强了配置的可读性，这种方法还降低了 Nginx 的处理要求；我们摆脱了不必要的 if 指令； 我们用了 $scheme 来表示 URI 中是 http 还是 https 协议，避免了硬编码。

**用 if 检查文件是否存在**

使用 if 指令来判断文件是否存在是很可怕的，如果你在使用新版本的 Nginx， 你应该看看 try_files，这会让你的生活变得更轻松。

糟糕的配置：
```
server {
    root /var/www/example.com;
    location / {
        if (!-f $request_filename) {
            break;
        }
    }
}
```

推荐的配置：
```
server {
    root /var/www/example.com;
    location / {
        try_files $uri $uri/ /index.html;
    }
}
```

我们不再尝试使用 if 来判断 $uri 是否存在，用 try_files 意味着你可以测试一个序列。 如果 $uri 不存在，就会尝试 $uri/，还不存在的话，在尝试一个回调 location。<br>

在上面配置的例子里面，如果 $uri 这个文件存在，就正常服务； 如果不存在就检测 $uri/ 这个目录是否存在；如果不存在就按照 index.html 来处理，你需要保证 index.html 是存在的。 try_files 的加载是如此简单。这是另外一个你可以完全的消除 if 指令的实例。






