# 1. cd 命令

cd：change directory，用于切换当前目录，它的参数是要切换到的目录的路径，可以是绝对路径，也可以是相对路径。如：
```
cd /root/Docements # 切换到目录/root/Docements  
cd ./path          # 切换到当前目录下的path目录中，“.”表示当前目录    
cd ../path         # 切换到上层目录中的path目录中，“..”表示上一层目录
```

# 2. ls 命令

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

# 3. mv 命令

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

# 4. rm 命令

该命令用于删除文件或目录，remove 之意，它的常用参数如下：
```
-f ：就是 force 的意思，忽略不存在的文件，不会出现警告消息，即使原档案属性设为唯读，亦直接删除，无需逐一确认。 
-i ：删除前逐一询问确认  
-r ：递归删除，最常用于目录删除，它是一个非常危险的参数
```
例如：
```
rm -i test.txt # 删除文件file，在删除之前会询问是否进行该操作  
rm -fr dir # 强制删除目录dir中的所有文件
```

# 5. tar 命令

该命令用于对文件进行打包，默认情况并不会压缩，如果指定了相应的参数，它还会调用相应的压缩程序（如 gzip 和 bzip 等）进行压缩和解压。它的常用参数如下：
```
-c ：新建打包文件  
-t ：查看打包文件的内容含有哪些文件名  
-x ：解打包或解压缩的功能，可以搭配-C（大写）指定解压的目录，注意-c,-t,-x不能同时出现在同一条命令中  
-j ：通过bzip2的支持进行压缩/解压缩  
-z ：通过gzip的支持进行压缩/解压缩  
-v ：在压缩/解压缩过程中，将正在处理的文件名显示出来  
-f filename ：filename为要处理的文件  
-C dir ：指定压缩/解压缩的目录 dir
```
例如：
```
压缩：tar -jcv -f filename.tar.bz2 要被处理的文件或目录名称  
查询：tar -jtv -f filename.tar.bz2  
解压：tar -jxv -f filename.tar.bz2 -C 欲解压缩的目录
多个文件压缩: tar czvf filename.tar.gz file1 file2 ... 也可以给 file* 文件 mv 目录再压缩
```

# 6. 系统信息

```
arch 显示机器的处理器架构(1) 
uname -m 显示机器的处理器架构(2) 
uname -r 显示正在使用的内核版本 
dmidecode -q 显示硬件系统部件 - (SMBIOS / DMI) 
hdparm -i /dev/hda 罗列一个磁盘的架构特性 
hdparm -tT /dev/sda 在磁盘上执行测试性读取操作 
cat /proc/cpuinfo 显示 CPU info 的信息 
cat /proc/interrupts 显示中断 
cat /proc/meminfo 校验内存使用 
cat /proc/swaps 显示哪些 swap 被使用 
cat /proc/version 显示内核的版本 
cat /proc/net/dev 显示网络适配器及统计 
cat /proc/mounts 显示已加载的文件系统 
lspci -tv 罗列 PCI 设备 
lsusb -tv 显示 USB 设备 
date 显示系统日期 
cal 2007 显示2007年的日历表 
date 041217002007.00 设置日期和时间 - 月日时分年.秒 
clock -w 将时间修改保存到 BIOS 
```

# 7. 系统的关机

```
shutdown -h now 关闭系统(1) 
init 0 关闭系统(2) 
telinit 0 关闭系统(3) 
shutdown -h hours:minutes & 按预定时间关闭系统 
shutdown -c 取消按预定时间关闭系统 
shutdown -r now 重启(1) 
reboot 重启(2) 
logout 注销 
```

# 8. 文件搜索 

```
find / -name file1 从 '/' 开始进入根文件系统搜索文件和目录 
find / -user user1 搜索属于用户 'user1' 的文件和目录 
find /home/user1 -name \*.bin 在目录 '/ home/user1' 中搜索带有'.bin' 结尾的文件 
find /usr/bin -type f -atime +100 搜索在过去100天内未被使用过的执行文件 
find /usr/bin -type f -mtime -10 搜索在10天内被创建或者修改过的文件 
find / -name \*.rpm -exec chmod 755 '{}' \; 搜索以 '.rpm' 结尾的文件并定义其权限 
find / -xdev -name \*.rpm 搜索以 '.rpm' 结尾的文件，忽略光驱、捷盘等可移动设备 
locate \*.ps 寻找以 '.ps' 结尾的文件 - 先运行 'updatedb' 命令 
whereis halt 显示一个二进制文件、源码或 man 的位置 
which halt 显示一个二进制文件或可执行文件的完整路径 
```

# 9. 挂载一个文件系统 

```
mount /dev/hda2 /mnt/hda2 挂载一个叫做hda2的盘 - 确定目录 '/ mnt/hda2' 已经存在 
umount /dev/hda2 卸载一个叫做hda2的盘 - 先从挂载点 '/ mnt/hda2' 退出 
fuser -km /mnt/hda2 当设备繁忙时强制卸载 
umount -n /mnt/hda2 运行卸载操作而不写入 /etc/mtab 文件- 当文件为只读或当磁盘写满时非常有用 
mount /dev/fd0 /mnt/floppy 挂载一个软盘 
mount /dev/cdrom /mnt/cdrom 挂载一个 cdrom 或 dvdrom 
mount /dev/hdc /mnt/cdrecorder 挂载一个 cdrw 或 dvdrom 
mount /dev/hdb /mnt/cdrecorder 挂载一个 cdrw 或 dvdrom 
mount -o loop file.iso /mnt/cdrom 挂载一个文件或 ISO 镜像文件 
mount -t vfat /dev/hda5 /mnt/hda5 挂载一个 Windows FAT32 文件系统 
mount /dev/sda1 /mnt/usbdisk 挂载一个usb 捷盘或闪存设备 
mount -t smbfs -o username=user,password=pass //WinClient/share /mnt/share 挂载一个 windows 网络共享 
```

# 10. 磁盘空间 
```
df -h 显示已经挂载的分区列表 
ls -lSr |more 以尺寸大小排列文件和目录 
du -sh dir1 估算目录 'dir1' 已经使用的磁盘空间' 
du -sk * | sort -rn 以容量大小为依据依次显示文件和目录的大小 
rpm -q -a --qf '%10{SIZE}t%{NAME}n' | sort -k1,1n 以大小为依据依次显示已安装的 rpm 包所使用的空间 (fedora, redhat类系统) 
dpkg-query -W -f='${Installed-Size;10}t${Package}n' | sort -k1,1n 以大小为依据显示已安装的deb包所使用的空间 (ubuntu, debian类系统) 
```

# 11. 用户和群组 
```
groupadd group_name 创建一个新用户组 
groupdel group_name 删除一个用户组 
groupmod -n new_group_name old_group_name 重命名一个用户组 
useradd -c "Name Surname " -g admin -d /home/user1 -s /bin/bash user1 创建一个属于 "admin" 用户组的用户 
useradd user1 创建一个新用户 
userdel -r user1 删除一个用户 ( '-r' 排除主目录) 
usermod -c "User FTP" -g system -d /ftp/user1 -s /bin/nologin user1 修改用户属性 
passwd 修改口令 
passwd user1 修改一个用户的口令 (只允许 root 执行) 
chage -E 2005-12-31 user1 设置用户口令的失效期限 
pwck 检查 '/etc/passwd' 的文件格式和语法修正以及存在的用户 
grpck 检查 '/etc/passwd' 的文件格式和语法修正以及存在的群组 
newgrp group_name 登陆进一个新的群组以改变新创建文件的预设群组 
```

# 12. 文件的权限 
```
# 使用 "+" 设置权限，使用 "-" 用于取消
ls -lh 显示权限 
ls /tmp | pr -T5 -W$COLUMNS 将终端划分成5栏显示 
chmod ugo+rwx directory1 设置目录的所有人(u)、群组(g)以及其他人(o)以读(r)、写(w)和执行(x)的权限 
chmod go-rwx directory1 删除群组(g)与其他人(o)对目录的读写执行权限 
chown user1 file1 改变一个文件的所有人属性 
chown -R user1 directory1 改变一个目录的所有人属性并同时改变改目录下所有文件的属性 
chgrp group1 file1 改变文件的群组 
chown user1:group1 file1 改变一个文件的所有人和群组属性 
find / -perm -u+s 罗列一个系统中所有使用了SUID 控制的文件 
chmod u+s /bin/file1 设置一个二进制文件的 SUID 位 - 运行该文件的用户也被赋予和所有者同样的权限 
chmod u-s /bin/file1 禁用一个二进制文件的 SUID位 
chmod g+s /home/public 设置一个目录的 SGID 位 - 类似 SUID ，不过这是针对目录的 
chmod g-s /home/public 禁用一个目录的 SGID 位 
chmod o+t /home/public 设置一个文件的 STIKY 位 - 只允许合法所有人删除文件 
chmod o-t /home/public 禁用一个目录的 STIKY 位 
```

# 13. YUM 软件包升级器
> Fedora, RedHat及类似系统

```
yum install package_name 下载并安装一个 rpm 包 
yum localinstall package_name.rpm 将安装一个 rpm 包，使用你自己的软件仓库为你解决所有依赖关系 
yum update package_name.rpm 更新当前系统中所有安装的 rpm 包 
yum update package_name 更新一个 rpm 包 
yum remove package_name 删除一个 rpm 包 
yum list 列出当前系统中安装的所有包 
yum search package_name 在 rpm 仓库中搜寻软件包 
yum clean packages 清理 rpm 缓存删除下载的包 
yum clean headers 删除所有头文件 
yum clean all 删除所有缓存的包和头文件 
```

# 14. DEB 包
> Debian, Ubuntu 以及类似系统

```
dpkg -i package.deb 安装/更新一个 deb 包 
dpkg -r package_name 从系统删除一个 deb 包 
dpkg -l 显示系统中所有已经安装的 deb 包 
dpkg -l | grep httpd 显示所有名称中包含 "httpd" 字样的 deb 包 
dpkg -s package_name 获得已经安装在系统中一个特殊包的信息 
dpkg -L package_name 显示系统中已经安装的一个 deb 包所提供的文件列表 
dpkg --contents package.deb 显示尚未安装的一个包所提供的文件列表 
dpkg -S /bin/ping 确认所给的文件由哪个 deb 包提供 
```

# 15. APT 软件工具
> Debian, Ubuntu 以及类似系统

```
apt-get install package_name 安装/更新一个 deb 包 
apt-cdrom install package_name 从光盘安装/更新一个 deb 包 
apt-get update 升级列表中的软件包 
apt-get upgrade 升级所有已安装的软件 
apt-get remove package_name 从系统删除一个deb包 
apt-get check 确认依赖的软件仓库正确 
apt-get clean 从下载的软件包中清理缓存 
apt-cache search searched-package 返回包含所要搜索字符串的软件包名称 
```

# 16. 备份 
```
dump -0aj -f /tmp/home0.bak /home 制作一个 '/home' 目录的完整备份 
dump -1aj -f /tmp/home0.bak /home 制作一个 '/home' 目录的交互式备份 
restore -if /tmp/home0.bak 还原一个交互式备份 
rsync -rogpav --delete /home /tmp 同步两边的目录 
rsync -rogpav -e ssh --delete /home ip_address:/tmp 通过 SSH 通道 rsync 
rsync -az -e ssh --delete ip_addr:/home/public /home/local 通过 ssh 和压缩将一个远程目录同步到本地目录 
rsync -az -e ssh --delete /home/local ip_addr:/home/public 通过 ssh 和压缩将本地目录同步到远程目录 
dd bs=1M if=/dev/hda | gzip | ssh user@ip_addr 'dd of=hda.gz' 通过 ssh 在远程主机上执行一次备份本地磁盘的操作 
dd if=/dev/sda of=/tmp/file1 备份磁盘内容到一个文件 
tar -Puf backup.tar /home/user 执行一次对 '/home/user' 目录的交互式备份操作 
( cd /tmp/local/ && tar c . ) | ssh -C user@ip_addr 'cd /home/share/ && tar x -p' 通过 ssh 在远程目录中复制一个目录内容 
( tar c /home ) | ssh -C user@ip_addr 'cd /home/backup-home && tar x -p' 通过 ssh 在远程目录中复制一个本地目录 
tar cf - . | (cd /tmp/backup ; tar xf - ) 本地将一个目录复制到另一个地方，保留原有权限及链接 
find /home/user1 -name '*.txt' | xargs cp -av --target-directory=/home/backup/ --parents 从一个目录查找并复制所有以 '.txt' 结尾的文件到另一个目录 
find /var/log -name '*.log' | tar cv --files-from=- | bzip2 > log.tar.bz2 查找所有以 '.log' 结尾的文件并做成一个 bzip 包 
dd if=/dev/hda of=/dev/fd0 bs=512 count=1 做一个将 MBR (Master Boot Record)内容复制到软盘的动作 
dd if=/dev/fd0 of=/dev/hda bs=512 count=1 从已经保存到软盘的备份中恢复 MBR 内容 
```







