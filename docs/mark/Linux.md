# Linux 常用命令及操作

**1. cd 命令**

用于切换当前目录，它的参数是要切换到的目录的路径，可以是绝对路径，也可以是相对路径。如：
```
cd /root/Docements # 切换到目录/root/Docements  
cd ./path          # 切换到当前目录下的path目录中，“.”表示当前目录    
cd ../path         # 切换到上层目录中的path目录中，“..”表示上一层目录
```

**2. ls 命令**

一个非常有用的查看文件与目录的命令，list之意，它的参数非常多，下面就列出一些我常用的参数，如下：
```
-l ：列出长数据串，包含文件的属性与权限数据等；ll 是 ls -l 的别名
-a ：列出全部的文件，连同隐藏文件（开头为.的文件）一起列出来（常用）  
-d ：仅列出目录本身，而不是列出目录的文件数据  
-h ：将文件容量以较易读的方式（GB，kB等）列出来  
-R ：连同子目录的内容一起列出（递归列出），等于该目录下的所有文件都会显示出来
```

> 这些参数也可以组合使用，下面举两个例子：

```
#  ls -lh    #不以字节方式显示文件大小
总用量 12K
-rwxrwxrwx 1 apache apache 4.6K 8月  23 14:21 app.config.php
-rwxrwxrwx 1 apache apache  442 8月  23 14:26 db.config.php


#  ls -ihS   #显示文件大小之后，从大到小排序
264672 app.config.php  264673 db.config.php
```

**3. mv 命令**

该命令用于移动文件、目录或更名，move 之意，它的常用参数如下：
```
-f ：force 强制的意思，如果目标文件已经存在，不会询问而直接覆盖  
-i ：若目标文件已经存在，就会询问是否覆盖  
-u ：若目标文件已经存在，且比目标文件新，才会更新
```
例如：
```
mv file1 file2 file3 dir # 把文件file1、file2、file3移动到目录dir中  
mv file1 file2 # 把文件file1重命名为file2

# 目标目录与原目录一致，指定了新文件名，效果就是仅仅重命名。
mv  /home/ffxhd/a.txt   /home/ffxhd/b.txt  

# 目标目录与原目录不一致，没有指定新文件名，效果就是仅仅移动。
mv  /home/ffxhd/a.txt   /home/ffxhd/test/ 

# 目标目录与原目录一致, 指定了新文件名，效果就是：移动 + 重命名。
mv  /home/ffxhd/a.txt   /home/ffxhd/test/c.txt
```



