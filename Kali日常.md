# Kali日常

时间：2019/9/3

## 0x1 VMware Tools安装

首先点开

![1567488749020](C:\Users\tx527\AppData\Roaming\Typora\typora-user-images\1567488749020.png)

里面有一个安装vmware tools的选项，点击之后，桌面会有一个光盘之类的图片，点击打开

在文件里面会有这么一个文件

~~~~~~
#：VMwareTools-10.3.10-12406962.tar.gz
~~~~~~

将其复制到主目录。进行解压，命令为：

~~~
#：tar -zxvf VMwareTools-10.3.10-12406962.tar.gz
~~~

解压完成之后，ls查看当前文件目录

![1567489339299](C:\Users\tx527\AppData\Roaming\Typora\typora-user-images\1567489339299.png)

进入其目录

![1567489385427](C:\Users\tx527\AppData\Roaming\Typora\typora-user-images\1567489385427.png)

可以看到一个pl文件，它是一个可执行文件，可以使用./来执行

~~~
#：./vmware-install.pl 
~~~

之后一直默认安装就行了，安装完成之后，重启:reboot

重启完，在左上角的查看里面有一个立即适应客户机，点击即可自适应。

至此完成安装。

## 0x2 更新源

打开sources.list文件

~~~
#：vim /etc/apt/sources.list
~~~

然后将下面的源写进去

~~~
#中科大
deb http://mirrors.ustc.edu.cn/kali kali-rolling main non-free contrib
deb-src http://mirrors.ustc.edu.cn/kali kali-rolling main non-free contrib
 
#阿里云
deb http://mirrors.aliyun.com/kali kali-rolling main non-free contrib
deb-src http://mirrors.aliyun.com/kali kali-rolling main non-free contrib
 
#清华大学
deb http://mirrors.tuna.tsinghua.edu.cn/kali kali-rolling main contrib non-free
deb-src https://mirrors.tuna.tsinghua.edu.cn/kali kali-rolling main contrib non-free
 
#浙大
deb http://mirrors.zju.edu.cn/kali kali-rolling main contrib non-free
deb-src http://mirrors.zju.edu.cn/kali kali-rolling main contrib non-free
 
#东软大学
deb http://mirrors.neusoft.edu.cn/kali kali-rolling/main non-free contrib
deb-src http://mirrors.neusoft.edu.cn/kali kali-rolling/main non-free contrib
 
#官方源
deb http://http.kali.org/kali kali-rolling main non-free contrib
~~~

：wq

保存之后，更新

~~~
#：apt-get update && apt-get upgrade && apt-get dist-upgrade
#：apt-get clean
~~~



## 0x3 ssh及apache2服务开启

### 0x31 ssh服务的开启

编辑配置文件

~~~
#：vim /etc/ssh/sshd_config
~~~

在里面找到下面两项

~~~
#PasswordAuthentication yes

#PermitRootLogin prohibit-password
~~~

改为：

~~~
PasswordAuthentication yes

PermitRootLogin yes  //这里是允许root用户登录，这里如果没有改的话，xsell登录的时候，密码输对也连不上
~~~

然后按下Esc，再按shift + :，输入wq保存并退出

接下来就可以开启ssh服务了

命令

~~~
#：service ssh start        //开启服务
#：service ssh status       //查看服务状态
#：update-rc.d ssh enable   //设置开机自启，使用update-rc.d命令添加开机执行脚本
~~~

![1567491335186](C:\Users\tx527\AppData\Roaming\Typora\typora-user-images\1567491335186.png)

ctrl + c退出

开启之后，xshell就可以直接连上了；

### 0x32 apache2服务开启

kali linux安装完成之后，自带apache2

直接开启就行了

~~~
#：service apache2 start
#：update-rc.d apache2 enable  //
~~~

kali apache2的配置文件目录

~~~
/etc/apache2/
~~~

![1567493968416](C:\Users\tx527\AppData\Roaming\Typora\typora-user-images\1567493968416.png)

在apache2.conf里面，想要遍历目录的话(测试使用，一般禁用，否则的话别人知道你浏览器目录就危险了)，可以这么设置

![1567494245569](C:\Users\tx527\AppData\Roaming\Typora\typora-user-images\1567494245569.png)

设置完成之后，访问网页的时候就会有这样的效果，这样就可以方便的测试函数了，将php文件都放在这个目录下，都会显示出来，就不需要一个一个去打。

![1567494316020](C:\Users\tx527\AppData\Roaming\Typora\typora-user-images\1567494316020.png)

这里是虚拟机里面访问，在主机上访问的话，需要知道虚拟机的IP，使用ifconfig查看虚拟机ip(本机为NET模式)

知道ip，之后直接在主机上访问即可

![1567494463758](C:\Users\tx527\AppData\Roaming\Typora\typora-user-images\1567494463758.png)

## 0x4 常用命令记录

#### 0x41 简单命令

##### 1 创建目录 

​	#：mkdir -p dirname [dir]

​		-p是确保目录名称存在，不存在就建一个

​		dirname 目录名

​		dir 创建的目录路径，写地话，默认创建在当前目录下

​	dirname 可以又后缀

##### 2 创建文件

​	#：vim 文件名  [路径]  //比如：vim test.php,执行之后就会进入编辑界面，在里面就可以编写文件的							内容了

​	#：touch 文件名  [路径] //比如：touch flag.txt,执行之后，在当前目录下就会生成一个flag.txt编				             辑内容需要自己打开

##### 3 解压文件

​	#：zip文件解压：unzip 文件名

​	#：tar.gz文件解压：tar -xvzf  文件名//xvzf顺序无所谓

##### 4 ls指令

​	#：ls [路径]  //路径可选，不写的时候，就是显示当前路径下的文件

​	#：ls -l     //显示当前路径下的文件及文件信息

![1568200768603](C:\Users\tx527\AppData\Roaming\Typora\typora-user-images\1568200768603.png)

​	#：ls -la   //显示当前目录下的所有文件及文件夹包括隐藏的.和..等的详细信息

##### 5 clear指令

​	清除当前终端的所有

​	快捷键：ctr+l

##### 6 su指令

​	(switch use)

​	#：su 需要切换的用户名（不写的话，默认root）

​	高权限用户切换到低权限用户，不需要密码。反之需要密码

##### 7 cd指令

​	（change directory）

​	#：cd 路径

​	当不写路径，就返回到当前用户的家目录（可以使用tab补全）

​	路径：

​	相对路径：相对当前路径的一种表现形式；特点：只要不是'/'开头的的就是

​	绝对路径：直接从'根'开始的一种路径形式；特点：以'/'开头

8 pwd指令

​	（print working directory）

​	打印当前的工作路径

![1568203386213](C:\Users\tx527\AppData\Roaming\Typora\typora-user-images\1568203386213.png)

##### 9 cp指令

​	(copy)

​	#：cp [-r] 需要复制的文档 需要保存的路径 

​	-r：表示递归。如果复制的是文件夹，需要使用-r

~~~
#：cp mortals.txt /etc/     //将当前目录下的mortals.txt 复制到/etc/  下
#：cp -r mortals /etc/       
~~~

##### 10 mv指令

​	(move)相当于剪切+粘贴

​	不需要参数，对文件和文件夹一样的操作

​	#：mv 需要操作的文档 新的文档的位置    

~~~
#：mv /home/mortals.txt /root/   //剪切到root目录下   
#：mv /home/mortals /root/
~~~

​	重命名

~~~~
#：mv /home/mortals.txt /root/tx.doc
#：mv /home/mortals /root/tx
~~~~

##### 11 rm指令

​	命令：rm  (remove)

​	语法：rm [-rf] 需要删除的文档

​		-r:表示递归

​		-f:表示强制	

​	删除目录需要使用参数-r

​	一般确认想要删除  直接用rm -rf 文件

​	慎用 rm -rf /  从语法上讲，该指令可以执行，表示递归删除根，但是这个操作很危险，不建议执行。

#### 0x42 文档查看命令

##### 1 tail指令

​	tail-末尾

​	作用：查看一个文件的末n行

​	语法：tail -n 文件路径

​	说明：n可以不写，默认是末尾十行

##### 2 head指令

​	作用：查看文件的头十行

​	语法：head -n 文件路径

​	说明：n可以不写，默认是头十行

##### 3 cat指令

​	作用：查看某个文件的全部内容【正序】

​	语法：cat 文件路径1 文件路径2 文件路径3...

​	可以显示多个文件的内容

##### 4 tac指令 

​	作用：查看某个文件的全部内容【逆序】

​	语法：cat 文件路径1 文件路径2 文件路径3...

​	从最后一行开始输出

##### 5 vim指令

#### 0x43 关机重启指令

##### 1 reboot指令

##### 2 shutdown

​	语法：shutdown -h 时间

​	常见值：

​		now（shutdown -h now）表示立即关机

​		+m（m表示分钟）比如+5 表示五分钟之后关机

##### 3 halt指令

​	作用：关机（关闭内存）

#### 0x44 进阶指令

##### 1 du指令

​	作用：du 表示directory used，目录的使用情况

​	语法：du -sh 目录路径

​		-s：表示sumary,汇总统计

​		-h：以较高可读性的形式显示

![1568212544464](C:\Users\tx527\AppData\Roaming\Typora\typora-user-images\1568212544464.png)

du指令与ls -l指令显示目录大小的区别

![1568213041297](C:\Users\tx527\AppData\Roaming\Typora\typora-user-images\1568213041297.png)

## 0x5 工具安装记录

### 0x51 隐写工具steghide

安装命令：

~~~
#：apt-get install steghide
~~~

可以使用该工具将一个待文件隐藏另一个隐藏文件载体里(图片或音频文件)，为了方便，将其放在一个目录下

命令：

~~~
#：steghide emdeb -cf 载体名 -ef 待隐藏文件名
#：steghide embed -cf 3.png -ef flag.txt
~~~

![1567496668167](C:\Users\tx527\AppData\Roaming\Typora\typora-user-images\1567496668167.png)

嵌入之后，并没有多大的变化

这是嵌入文件之前图片的信息

![1567496950997](C:\Users\tx527\AppData\Roaming\Typora\typora-user-images\1567496950997.png)

嵌入之后

![1567496982836](C:\Users\tx527\AppData\Roaming\Typora\typora-user-images\1567496982836.png)

使用以下命令显示隐藏的信息

~~~
steghide info 3.png
~~~

![1567497172882](C:\Users\tx527\AppData\Roaming\Typora\typora-user-images\1567497172882.png)

提取隐藏文件

~~~
steghide extract -sf 3.png
~~~

![1567497376912](C:\Users\tx527\AppData\Roaming\Typora\typora-user-images\1567497376912.png)

将文件嵌入到载体之后，可以将文件删除，使用以上命令提取之后，在该目录下就会得到文件，并且文件没有损坏



pcat师傅的python实现爆破，我没成功，这里记录一下代码，windows平台下，先下载https://sourceforge.net/projects/steghide/，然后解压就可以直接使用，在windows里除非把steghide所在文件夹加入系统变量Path里，否则上面py代码文件、3.jpg、passwd.txt得放在steghide所在文件夹里。

~~~python
# -*- coding: utf8 -*-
#author:pcat
#http://pcat.cnblogs.com
from subprocess import *

def foo():
    stegoFile='3.jpg'
    extractFile='hide.txt'
    passFile='passwd.txt'

    errors=['could not extract','steghide --help','Syntax error']
    cmdFormat="steghide extract -sf %s -xf %s -p %s"
    f=open(passFile,'r')

    for line in f.readlines():   #readlines() 用于读取文件所有的行，并返回列表
        cmd=cmdFormat %(stegoFile,extractFile,line.strip())  #line.strip()去掉该行首尾空白
        p=Popen(cmd,shell=True,stdout=PIPE,stderr=STDOUT)
        content=unicode(p.stdout.read(),'gbk')
        for err in errors:
            if err in content:
                break
        else:
            print content,
            print 'the passphrase is %s' %(line.strip())
            f.close()
            return

if __name__ == '__main__':
    foo()
    print 'ok'
    pass
~~~



### 0x52 mprf以及mpc安装

一、必须先安装mprf:

1、unzip mpfr-4.0.2.zip  

	/*Linux unzip命令用于解压缩zip文件，unzip为.zip压缩文件的解压缩程序。*/

2、cd mpfr-4.0.2/   

3、./configure

	/*主要的作用是对即将安装的软件进行配置，检查当前的环境是否满足要安装软件的依赖关系，但并不是所有的tar包都是源代码的包*/

4、make 

	/*用来编译的，它从Makefile中读取指令，然后编译。*/

5、make install

	/*用来安装的，它也从Makefile中读取指令，安装到指定的位置*/

二、再安装mpc:

tar -xvzf mpc-1.1.0.tar.gz 

cd mpc-1.1.0/

./configure 
make 
make install

三、最后安装gmpy2

pip3 install gmpy2



先下载文件：RsaCtfTool-master.zip，放到主目录上。

git clone https://github.com/Ganapati/RsaCtfTool.git

cd RsaCtfTool

pip install -r requirements.txt  /安装到python2中去

### 0x53 hashpump安装

~~~
git clone https://github.com/bwall/HashPump
apt-get install g++ libssl-dev
cd HashPump
make
make install
~~~

HashPump用法

看下如下代码（实验吧的一个题）：

~~~php
<?php
$secret="XXXXXXXXXXXXXXX"; // This secret is 15 characters long for security!
$username="admin";
$password = $_POST["password"];
if($COOKIE["getmein"] === md5($secret . urldecode($username . $password))){
    echo "Congratulations! You are a registered user.\n";
    die ("The flag is ". $flag);
}else{
    die("Your cookies don't match up! STOP HACKING THIS SITE.");
}
?>
~~~

在里面可以看到hash值为：571580b26c65f306376d4f64e53cb5c7

![1567524715443](C:\Users\tx527\AppData\Roaming\Typora\typora-user-images\1567524715443.png)

从代码里面及哈希值：

~~~
$secret是密文，长度为15，如果再算上后面第一个admin，长度就是20
而数据是admin
签名（哈希值）是571580b26c65f306376d4f64e53cb5c7
~~~

使用hashpump

![1567524931181](C:\Users\tx527\AppData\Roaming\Typora\typora-user-images\1567524931181.png)

得到新的签名和数据

~~~
292143649bc420ed83ef095fe5c2753e
admin\x80\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\xc8\x00\x00\x00\x00\x00\x00\x00tx
~~~

将0x换成%

~~~
292143649bc420ed83ef095fe5c2753e
admin%80%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%c8%00%00%00%00%00%00%00tx
~~~

BP抓包，改包发送

![1567525350034](C:\Users\tx527\AppData\Roaming\Typora\typora-user-images\1567525350034.png)

