## 1.腾讯PHP经典面试题

一、PHP开发部分

1．合并两个数组有几种方式，试比较它们的异同

2．请写一个函数来检查用户提交的数据是否为整数（不区分数据类型，可以为二进制、八进制、十进制、十六进制数字）

3．PHP的strtolower()和strtoupper()函数在安装非中文系统的服务器下可能会导致将汉字转换为乱码，请写两个替代的函数实现兼容Unicode文字的字符串大小写转换

4．PHP的is_writeable()函数存在Bug，无法准确判断一个目录/文件是否可写，请写一个函数来判断目录/文件是否绝对可写

5．PHP的chmod()函数存在Bug，无法保证设置成功，请写一个函数在指定路径下创建一个目录/文件并确保可以正确设置权限掩码

6．PHP处理上传文件信息数组中的文件类型$_FILES['type']由客户端浏览器提供，有可能是黑客伪造的信息，请写一个函数来确保用户上传的图像文件类型真实可靠

7．PHP通过对数据的URL编码来实现与Javascript的数据交互，但是对于部分特殊字符的编解码与Javascript的规则不尽相同，请具体说明这种差异，并针对UTF-8字符集的数据，写出PHP的编解码函数和Javascript的编解码函数，确保PHP编码数据可以被Javascript正确解码 、Javascript编码的数据可以被PHP正确解码

8．试阐述Memcache的key多节点分布的算法？当任一节点出现故障时PHP的Memcache客户端将如何处置？如何确保Memcache数据读写操作的原子性？

9．如何实现PHP的安全最大化？怎样避免SQL注入漏洞和XSS跨站脚本攻击漏洞？

10．请设计一个数据结构可以实现无限级子菜单的树型菜单功能并提供菜单生成算法，用UML描述设计并写出相关PHP代码

二、系统相关部分

1．请简述Linux、FreeBSD、Soalaris、Mac OS、Windows几种系统下进程与线程的内核实现方式、管理机制的异同

2．请简述Linux/BSD系统下进程间通讯的方式有哪些，并具体说明在PHP下如何实现

3．请简述Linux/BSD系统下系统的消息/事件异步通知机制有几种，并加以比较

4．简单比较TCP/UDP协议的异同，对于PHP的Socket扩展与Stream扩展，试比较两者基于TCP/UDP协议的SOCKET编程差异？

5．为什么会出现僵死进程（孤儿进程）？怎样查看僵死进程？如何解决僵死进程问题？

6．对于System-V消息队列，如何解决系统本身对于消息队列条数、总容量（字节数）的限制？如何设置消息的优先级别？请比较阻塞模式和非阻塞模式的异同，并说明如何避免非阻塞模式下的消息队列堵塞？

7．请描述Apache 2.x版本的MPM（Multi-Processing Module）机制，并具体说明在不同的MPM机制下如何支持PHP？

8．请简述PHP在Apache下的几种运行方式并加以比较？如何让PHP在Linux+Apache下以Fast CGI方式运行？

9． 请写出让PHP能够在命令行下以脚本方式执行时安装PHP所必须指定的configure参数，并说明如何在命令行下运行PHP脚本（写出两种方式）同时向PHP脚本传递参数？

10．请简述PHP 5.2的内存池及其内存管理机制、垃圾回收机制

## 2.2018年小米高级 PHP 工程师面试题（模拟考试卷）

1．通过哪一个函数，可以把错误转换为异常处理？

A：set_error_handler
B：error_reporting
C：error2exception
D：catch

正确答案：A

答案分析：set_error_handler() 可指定一个回调函数，错误发生时，会自动通过指定的回调函数处理。在回调函数中抛出新的异常即可。

2．下列哪个shell函数的描述是正确的?

A：shell函数可以先调用后定义
B：shell函数需使用关键字function定义
C：shell函数内的变量可以声明为局部变量
D：shell函数只能通过return返回值，1是成功，0是失败

正确答案：C

答案分析：shell函数必须先定义在调用；声明时，无需使用关键字；通过local可以定义函数内的局部变量；shell函数返回值，0是成功，非0是错误，其他选项正确

3．下列关于全文检索技术的说法，不对的是：

A: Solr是新一代的全文检索组件，它比Lucene的搜索效率高很多，还能支持HTTP的访问方式，PHP调用Solr也很方便。 
B: MySQL中把一个字段建立FULLTEXT索引，就可以实现全文检索，目前MyISAM和InnoDB的table都支持FULLTEXT索引。
C: Sphinx是一个基于SQL的全文检索引擎，可以结合MySQL做全文搜索，它可以提供比数据库本身更专业的搜索功能。
D: Lucene附带的二元分词分析器CJKAnalyzer切词速度很快，能满足一般的全文检索需要。

正确答案：A

答案分析： Solr是新一代的全文检索组件，它基于Lucene，所以说它比Lucene快就是胡扯 ：）

4．关于单例模式的说法，错误的是？

A：单例模式的目的是确保在全局环境中，一个类只能有一个实例存在
B：单利模式一般要讲构造函数设置为 private
C：只需要将构造函数设置为private 即可确保全局中只有一个实例
D：连接数据库的功能通常用单例模式实现

正确答案：C

答案分析：构造函数设置为private，仅能确保无法通过 new 创建新实例，但仍可以通过 clone、反序列化等方式创建多个实例。

5．正则的引擎表述错误的是？

A 正则引擎主要可以分为两大类：一种是DFA，一种是NFA。
B 一般而论，NFA引擎则搜索更快一些。但是DFA以表达式为主导，更容易操纵，因此一般程序员更偏爱DFA引擎！
C NFA表达式主导,DFA文本主导.
D 可以使用是否支持忽略优先量词和分组捕获来判断引擎类型：支持 NFA,不支持 DFA

正确答案：B

答案分析：正确的说法应该是：一般而论，DFA引擎则搜索更快一些。但是NFA以表达式为主导，更容易操纵，因此一般程序员更偏爱NFA引擎！

6．方框中的正则表达式能与以下哪些选项匹配？

/.\123\d/

A. **123

B. ****1234

C. 1234

D.123


正确答案：B

答案分析：本题的要点是理解这个正则表达式的含义——从左往右，首先是零个或多个任意字符（.），跟着是一个星号（），然后是 123，最后是一个数字。因此答案是B。

7．如下关于数据库的说法，哪个是错误的？

A：为了效率数据库可以有多个读库
B：数据库可以用主从做热备
C：数据库不能提供多主多从架构
D: 数据库主从是通过日志同步的

正确答案：C

答案分析： 数据库可以提供多主多从架构。

8．下面哪个不是XSS漏洞的修复方式？

A:对参数进行htmlspecialchas过滤
B:对参数使用白名单过滤
C:不允许输入的内容显示到浏览器
D:禁止在js标签内输出用户输入的内容

正确答案：A

答案分析：这类过滤可以解决尖括号类型的xss，无法解决js标签内的xss

9．下列哪一项不是PHP SAPI模式？

A.ISAPI
B.CGI
C.FastCGI
D.RESTFUL APi

正确答案：D

答案分析：A~C是最常用的模式，D是一种接口的组织方式。

10．对一个大文件进行逐行遍历，如下方法性能较高的是？

A：写一个实现了IteratorAggregate 接口的类，通过该类使用foreach遍历。
B：使用file_get_contents 将文件内容一次性载入内存，然后逐行遍历。
C：通过exec函数，调用shell 工具遍历
D：使用别人写的类库

正确答案：A

答案分析：使用 IteratorAggregate 可将文件打开后通过移动指针的方式逐行遍历，不受文件大小影响。使用 file_get_contents 处理大文件很容易导致PHP内存溢出；调用exec 会产生额外的进程，影响性能；其他人写的类库质量不一定高。

11．如下选项，哪个不是设计模式应该遵循的原则？

A：组合优于继承
B：针对接口编程
C：尽可能降低耦合
D：尽量使用高性能的语法

正确答案：D
答案分析：设计模式的关注点在于代码的可维护性和可复用性，D选项不是设计模式关注的要点。

12．下列关于回溯的表达式错误的是？

A ab.lmn 匹配 abcdeflmnghijklmn 中的 abcdeflmnghijklmn
B ab.?lmn 匹配 abcdeflmnghijklmn 中的 abcdeflmn
C ab??c 匹配 abcdeflmnghijklmn 中的 abc
D .*lmn 匹配 abcdeflmnghijklmn 中的 abcdeflmn

正确答案：D

答案分析：D是贪婪匹配，所以应该匹配到的结果是abcdeflmnghijklmn

13．函数中如果使用了try catch finally 语法结构，return 应该写在哪儿？

A：finally 中
B：try 中
C：catch 中
D：任意位置

正确答案：A

答案分析：try 中 return 后 finally 会继续执行，如果 finally 中也有return，则最终返回值为 finally 中 return 的值。

14．以下关于NOSQL的说法，不对的是：

A: Redis支持字符串、哈希、列表、集合、有序集合等数据结构，目前Redis不支持事务。
B: MongoDB支持CAP定理中的AP，MySQL支持CAP中的CA，全部都支持不可能存在。
C: MongoDB不用先创建Collection的结构就可以直接插入数据，目前MongoDB不支持事务。
D: Memcache既支持TCP协议，也支持UDP协议，我们可以把PHP的Session存放到Memcache中。

正确答案：A

答案分析：Redis支持事务。

15．Innodb 锁机制说法错误的是？

A：Innodb提供了表锁与行锁两种锁机制
B：Innodb的表锁所会在表变更的时候触发
C：Innodb下update时会自动给涉及到的行加上排他锁，并创建出一个镜像副本， 此时进行select 时查询的是镜像副本的数据
D：Innodb行锁状态下读不受影响，写会受影响（涉及到的数据）

正确答案：A

16．下列哪个是创建一个每周三01:00~04:00每3分钟执行执行一次的crontab指令？

A： 1,4 3 /bin/bash /home/sijiaomao/ok.sh
B：/3 1,4 3 /bin/bash /home/sijiaomao/ok.sh
C：/3 1-4 3 /bin/bash /home/sijiaomao/ok.sh
D：/3 1-4 * /bin/bash /home/sijiaomao/ok.sh

正确答案：C

答案分析：A：每周三的1时4时每分钟执行一次 B：每周三的1时4时每3分钟执行一次 C：满足要求 D：每天的1时4时每3分钟执行一次

17．在拆分之前，系统中很多列表和详情页所需的数据是可以通过sql join来完成的。而拆分后，数据库可能是分布式在不同实例和不同的主机上，join将变得非常麻烦。下面哪种方法不能有效解决这个问题？

A 全局表,系统中所有模块都可能会依赖到的一些表在各个库中都保存。
B 字段冗余,“订单表”中保存“卖家Id”的同时，将卖家的“Name”字段也冗余，这样查询订单详情的时候就不需要再去查询“卖家用户表”。
C 主从复制,将数据库的读写分离。
D 数据同步,定时A库中的tbl_a表和B库中tbl_b关联，可以定时将指定的表做主从同步。

正确答案：C

答案分析：主从复制,将数据库的读写分离。只能扩容读并发，并不能缓解跨库join的问题。

18．关于网络IO模型，下列哪一项是正确的？

A.Select比Epoll更快
B.nginx使用的是select模型
C.apache支持select和epoll两种方式的切换
D.epoll能支持更大的并发

正确答案：D

答案分析：A epoll更快一些。B nginx使用epoll模型。C apache只支持select

19．PHP执行的时候有如下执行过程：Scanning(Lexing) - Compilation - Execution - Parsing，其含义分别为：

A：将PHP代码转换为语言片段(Tokens)、将Tokens转换成简单而有意义的表达式、将表达式编译成Opocdes、顺次执行Opcodes
B：将PHP代码转换为语言片段(Tokens)、将Tokens转换成简单而有意义的表达式、顺次执行Opcodes、将表达式编译成Opocdes
C：将PHP代码转换为语言片段(Tokens)、将表达式编译成Opocdes、顺次执行Opcodes、将Tokens转换成简单而有意义的表达式
D：将PHP代码转换为语言片段(Tokens)、将表达式编译成Opocdes、将Tokens转换成简单而有意义的表达式、顺次执行Opcodes

正确答案：C

答案分析：正确答案为C，正确的顺序为：Scanning(Lexing)、Parsing、Compilation、Execution


## 3.阿里php面试题（一）

一、选择题（需要注意有单选有多选）

1、以下HTTP状态码中哪个表示服务器拒绝访问（）
A、302 B、304 C、403 D、404

2、php core dump 导致nginx返回的http状态码（）
A、404 B、500 C、502 D、504

3、下面哪些函数可以用来读取文件（）
A、file B、file_get_contents C、fgets D、popen

4、下面哪些函数可以用来判断a字符串是否存在于b字符串（）
A、strpos B、strstr C、substr D、preg_match

5、下面那种类型攻击可能导致Cookie被发送到黑客手中（）
A、xss B、sql inject C、csrf D、ssrf

6、当nginx作为负载均衡服务器，与php web服务器分布式部署的时候，下面哪个能获取到客户端ip（）
A、$_SERVER["REMOTE_ADDR"] B、$_SERVER["SERVER_ADDR"] C、$_SERVER["REMOTE_HOST"] D、$_SERVER["HTTP_X_FORWARDED_FOR"]

7、下面哪些命令可以用来查看端口是否被占用（）
A、ps B、du C、netstat D、lsof

8、下面哪些函数可以用于获取字符串长度（）
A、strlen B、mb_strlen C、iconv_strlen D、strlength

9、下面哪些shell命令可以用于读取文件内容（）
A、cat B、tail C、less D、sed

10、你通常如何学习新技术（）
A、阅读新技术相关博客/文章 B、观看新技术相关直播/视频 C、线上向新技术对应专家提问 D、线下参加新技术的沙龙/活动 E、向周边懂技术的同事/老师请教

二、代码题

1、你经常去那些技术社区？列举一个并说明其优点。

2、现有学生表（t_student）和成绩表（t_score）
  学生表有学生id（id），学生姓名（name），性别（sex）字段；
  成绩表有成绩id（id），学生id（student_id），学科id（item_id）和成绩（score）字段。
  请通过SQL查找出每个学生所有学科的平均分和最高分和最低分

3、请用尽可能多的方式来判断一个ip是否属于192.168.1.1~192.168.1.100
  （必须写代码，不要只写思路，方法越多越好，提醒，这题很考验逻辑思维能力，请认真作答）
  （代码写在背面）

4、有一个字符串 $a = "503,123,789,453,987,102,23,45,90"；请写一个函数，将其从小到大排序，以一个数组返回，不能使用php自带的函数。

三、思考题

1、有一个后台模块只有白名单里的用户才能访问，下面这段代码返回结果是什么？应该如何改进。
```
function permissionCheck($uid)(
    $uids = "162,157,186";
    if(strpos($uid,$uids) === false){
        return false;
    }
    return ture;
)
```

permissionCheck(162);

2、在文章列表场景中，翻页到后面就非常慢，比如下面的sql，有什么优化的手段吗，不局限于sql优化。

select id,title,content from article
where is_del = 0 order by id desc limit 8000000,20;


## 4.阿里php面试题（二）

一、单选题（共27题，每题5分）

1.Memcache与Redis的比较错误的是？

A、Memcache过期后，不删除缓存，会导致下次取数据数据的问题，Redis有专门线程，清除缓存数据；

B、Memcache和redis都是只支持单线程；CPU利用方面Memcache和redis部分伯仲

C、Memcache只支持key value存储方式，Redis支持更多的数据类型，比如Key value，hash，list，set，zset；

D、Memcache自身不支持持久化，Redis支持持久化；

参考答案：B

答案解析：

Memcache支持多线程，redis支持单线程；CPU利用方面Memcache优于redis

2.mysql5.7中关于json类型的说明，不对的是哪个？

A、JSON数据可以做有效性检查

B、json数据中，还是需要遍历所有字符串才能找到数据

C、JSON使得查询性能提升

D、JSON支持部分属性索引，通过虚拟列的功能可以对JSON中的部分数据进行索引

参考答案：B

答案解析：

原生的JSON优势如下： 1. 存储上类似text,可以存非常大的数据。 2. JSON有效性检查：插入的数据必须是JSON类型的字符串才行。 3. 相比于传统形式，不需要遍历所有字符串才能找到数据。 4. 支持索引：通过虚拟列的功能可以对JSON中部分的数据进行索引

3.执行下面代码$x会变成什么值呢？

A、NULL

B、255

C、0

D、false

参考答案：C

答案解析：

正确答案：C 答案解析：oxFF是一个十六进制数，这时不会转整型比较，会先将16进制数字转换成10进制数字，再做比较。 使用int函数，PHP会使用is_numeric_string 判断字符串是否包含十六进制数字然后进行转换。发现0xff的0后面无数字，故为0.

4.大数据的数据库 （NoSQL）与关系型数据库的区别：

A、 水平扩展与垂直扩展

B、 是否支持事务的 ACID

C、 应用中两种数据库互相补充

D、 以上都是

参考答案：D

答案解析：无

5.关于判断文件类型，以下说法正确的是？

A、根据文件的扩展名可以正确判断文件的类型

B、根据文件的特征值可以正确判断文件类型

C、根据文件的大小及特征值可以正确判断文件类型

D、通过任何方法也无法100%确定文件类型

参考答案：D

答案解析：任何方式都可以伪造，所以我们只能通过方法无限接近，而无法完全保证可以判断正确。

6.以下命令描述争取的是？

ps -aux --sort -pcpu,+pmem | head -n 10

A、查询CPU使用排名前十的程序

B、查询访问CPU十次以上的程序

C、查询cpu和缓存访问前十的程序

D、查询cpu和内存前十的记录

参考答案：D

答案解析：ps -aux --sort -pcpu,+pmem | head -n 10 通过aux命令查询cup和内存前十的记录

7.关于json说法错误的是：

A、json_encode只能处理utf-8编码的数据

B、可以用sprintf组装或解析json字符串

C、json_encode只能编码数组

D、json_decode可以将json字符串解码成对象

参考答案：C

答案解析：json_encode 可以编码成数组和对象

8.以下关于进程和程序的区别的说法，错误的是？

A、程序没有状态，而进程是有状态的

B、程序是一组有序的静态指令，进程是一次程序的执行过程

C、程序可以长期保存，进程是暂时的

D、程序只能在前台运行，而进程可以在前台或后台运行

参考答案：D

答案解析：程序是一段可执行的代码文件，在linux上就是文件。 程序运行时就被称为进程，即进程是运行状态的程序。

9.PHP面向对象原则理解错误的是？

A、接口分离原则

B、依赖原则

C、替换原则

D、多项职责原则

参考答案：D

答案解析：五大基本原则 单一职责原则SRP(Single Responsibility Principle) 是指一个类的功能要单一，不能包罗万象。如同一个人一样，分配的工作不能太多，否则一天到晚虽然忙忙碌碌的，但效率却高不起来。 开放封闭原则OCP(Open－Close Principle) 一个模块在扩展性方面应该是开放的而（需要更多学习资料和面试题请加入qun6/7/7/0/7/9/7/7/0）在更改性方面应该是封闭的。比如：一个网络模块，原来只服务端功能，而现在要加入客户端功能， 那么应当在不用修改服务端功能代码的前提下，就能够增加客户端功能的实现代码，这要求在设计之初，就应当将服务端和客户端分开，公共部分抽象出来。 替换原则(the Liskov Substitution Principle LSP) 子类应当可以替换父类并出现在父类能够出现的任何地方。比如：公司搞年度晚会，所有员工可以参加抽奖，那么不管是老员工还是新员工， 也不管是总部员工还是外派员工，都应当可以参加抽奖，否则这公司就不和谐了。 依赖原则(the Dependency Inversion Principle DIP) 具体依赖抽象，上层依赖下层。 假设B是较A低的模块，但B需要使用到A的功能，这个时候，B不应当直接使用A中的具体类： 而应当由B定义一抽象接口，并由A来实现这个抽象接口，B只使用这个抽象接口：这样就达到 了依赖倒置的目的，B也解除了对A的依赖，反过来是A依赖于B定义的抽象接口。通过上层模块难以避免依赖下层模块，假如B也直接依赖A的实现，那么就可能造成循环依赖。一个常见的问题就是编译A模块时需要直接包含到B模块的cpp文件，而编译B时同样要直接包含到A的cpp文件。 接口分离原则(the Interface Segregation Principle ISP) 模块间要通过抽象接口隔离开，而不是通过具体的类强耦合起来。

10.三个人独立地破译一份密码，已知各人能译出的概率分别为 1/5，1/4，1/3，则密码能被破译的概率为 ?

A、1/60

B、3/5

C、59/60

D、13/30

参考答案：B

答案解析：题目中，至少有一人能破译密码和三人都不能破译密码是对立事件。 所以至少有一人能译出的概率=1－三人都没译出的概率=1－(1－1/5)(1－1/3)(1－1/4)=1－2/5=3/5。

11.PHP数组类型与其他类型转换，以下错误的是？

A、int,float,string,boolean,resource类型(array)$a等同于 array($a)

B、(array)object 键名是对象成员变量名，键值是对象成员属性

C、array(false)=[] 空数组

D、(array)null = [] 空数组

参考答案：C

答案解析：int,float,string,boolean,resource类型(array)$a等同于 array($a) (array)object 键名是对象成员变量名，键值是对象成员属性 (array)null = 空数组。

12.以下关于结构型模式说法错误的是？

A、结构型模式可以在不破坏类封装性的基础上，实现新的功能

B、结构型模式主要用于创建一组对象

C、结构型模式可以创建一组类的统一访问接口

D、结构型模式可以在不破坏类封装性的基础上，使得类可以同不曾估计到的系统进行交互

参考答案：B

答案解析：结构型（structural）：处理类或对象间的组合。

13.小王的部门领导给下达了一个任务：由于网站某个栏目访问量很大，因此需要专门给网站的某个url请求做负载均衡，那么该通过什么集群软件实现呢？

A、LVS集群软件

B、oneproxy集群软件

C、haproxy集群软件

D、keepalived集群软件

很多人在刚接触这个行业的时候或者是在遇到瓶颈期的时候，总会遇到一些问题，比如学了一段时间感觉没有方向感，不知道该从那里入手去学习，对此我整理了一些资料，需要的可以免费分享给大家（11年架构师带你解读年薪50万面试通关秘籍。）

如果喜欢我的文章，想与一群资深开发者一起交流学习的话，获取更多相关大厂面试咨询和指导，欢迎加入我的学习交流群点击此处PHP高级交流

14.以下文件被称为纯文本文件或ASCII文件的是（）。

A、 以.EXE为扩展名的文件

B、 以.TXT为扩展名的文件

C、 以BMP为扩展名的文件

D、 以.DOC为扩展名的文件

15.以下能够删除一列的是

A、alter table emp remove addcolumn

B、alter table emp drop column addcolumn

C、alter table emp delete column addcolumn

D、alter table emp delete addcolumn

16.以下哪个后缀的文件类型不是文本文件？

A、word

B、Excel

C、txt

D、pdf

18.以下关于非对称加密的说法错误的是

A、加密速度慢

B、安全性高

C、双方需要同步密钥

D、可以进行数字签名

19.关于Laravel中间件错误的是？

A、运行Artisan 命令 make:middleware 创建新的中间件

B、可定义前置 & 后置中间件

C、中间件是代理模式

D、中间件是中介模式

20.以下可以将PHP变量序列化并且保存到文件中的是？

A、serialize()

B、json_encode()

C、var_export()

D、以上都可以

21.关于php配置选项错误的是

A、开启 short_open_tag 允许使用PHP 代码开始标志的缩写形式（<? ?>）

B、如果启用了 magic_quotes_runtime，大多数返回任何形式外部数据的函数，包括数据库和文本段将会用反斜线转义引号

C、因为可以在运行时使用ini_set对配置选项进行设置，所以display_errors可以一直开启

D、PHP 的安全模式是为了试图解决共享服务器（shared-server）安全问题而设立的

22.关于PHP数组Bucket结构体，说法错误的是？

A、void *pData 指向value

B、void *pKey 指向key

C、void *pDataPtr 指向value的指针

D、struct bucket *pLast 存放同一个Bucket内的上一个元素

23.设计性能较优的关系模式称为规范化，规范化主要的理论依据是（ ）。

A、关系规范化理论

B、关系运算理论

C、关系代数理论

D、数理逻辑

24.从用户在浏览器中输入网址并回车，到看到完整的页面，中间都经历了哪些过程?

A、 浏览器->url->dns->ip->port->nginx->tcp->server name->php-fpm/fast cgi->php

B、 浏览器->url->dns->ip->tcp->port->nginx->server name->php-fpm/fast cgi->php

C、 浏览器->url->dns->ip->port->tcp->nginx->server name->php->php-fpm/fast cgi

D、 浏览器->url->dns->ip->port->tcp->nginx->server name->php-fpm/fast cgi->php

25.阅读下面PHP代码，并选择输出结果( )

A、0

B、1

C、2

D、3

26.以下哪条不是PHP语言的特性？

A、开源

B、免费

C、基于客户端

D、便捷高效

27.关于PHP数组key和value的限制条件，说法正确的是？

A、key只能是int或string类型，value可以使任何类型

B、key可以是任何类型，value可以是任何类型

C、key可以是任何类型，value只能是int或string类型

D、key只能是int或string类型，value只能是int或string类型

二、多选题（共3题，每题5分）
1.假设当前屏幕分别率为1024×768，定义一个居中的占屏幕一半大小的表格的语句是
```
A、<TABLE ALIGN=”CENTER” WIDTH=”50%”></TABLE>

B、<TABLE ALIGN=”CENTER” WIDTH=”512″></TABLE>

C、<DIV ALIGN=”CENTER”><TABLE WIDTH=”512″></TABLE></DIV>

D、<CENTER><TABLE WIDTH=”50%”></TABLE></CENTER>
```

参考答案：A,B,C,D

答案解析：center标签HTML5不推荐使用了...这种没有语义的纯样式标签是不符合w3c规范的，MDN上也有相关说明。

2.下列正则表达式不能匹配”www.innotechx.com”的是：
下列正则表达式不能匹配”www.innotechx.com”的是：

A、^w+.w+.w+$

B、[w]{0,3}.[a-z]*.[a-z]+

C、^w.*com$

D、[w]{3}.[a-z]{11}.[a-z]

3.为什么大型网站要使用消息队列？

A、解耦

B、异步

C、削峰

D、大数据处理