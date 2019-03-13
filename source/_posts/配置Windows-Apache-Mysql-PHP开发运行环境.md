---
title: 配置Windows+Apache+Mysql+PHP开发运行环境
date: 2019-03-13 09:43:49
tags: PHP
categories: PHP
---
**安装Apache**

1. 获取***Apache Server*** [下载](http://mirror.bjtu.edu.cn/apache//httpd/binaries/win32/httpd-2.2.17-win32-x86-openssl-0.9.8o.msi)。
2. 这个是包括了open ssl 模块的window可执行程序，直接运行安装到我们需要放置的目录下面。注意使用80端口，这样不必额外制定端口号就可以访问本地Http Server了。
3. 一切安装好后，打开浏览器，输入http://localhost进行测试，出现“It Works”那么安装就成功了。

**安装Mysql**

1. 获取 ***Mysql***[下载](http://www.mysql.com/downloads/mirror.php?id=397271#mirrors)。
2. 安装 Mysql，这个是打包好了的安装包，一路根据提示安装上即可。
3. 另外还有一个Mysql Workbench，这个是一个可视化的Mysql管理软件，可以一同下下来或者选用别的顺手的管理客户端均可。
4. 用管理客户端链接本地的Mysql，能连接上说明安装成功。

**安装PHP**

1. 获取 **PHP **[下载](http://windows.php.net/download/)。可以看到多个版本供我们选择：
    >*   如果Apache的版本是1或2，那么下载VC6编译的版本
    >*   如果选用IIS作为Server，那么使用VC9编译的版本
    >*   X86代表32位的操作系统，X64则代表64位操作系统
    >*   Thread Safe和Non Thread Safe,取决于Web Server对PHP的执行方式。如果是ISAPI,需要调用dll来处理用户请求，由于处理完后相关dll不会马上消失，所以需要进行线程安全检查以使用多线程，从而提高效率，使用Thread Safe较好。如果是Fast CGI,由于只进行单线程的运行，因此没必要进行线程并发下的安全性检查，去掉线程安全检查等于取消不必要的系统耗费从而提高运行速度，使用 Non Thread Safe的较好。

这里也有直接的安装包，直接安装即可，但我们选择ZIP包进行手工安装，一来手工安装更灵活，二来可以了解PHP的内部结构，这个对于以后进一步使用PHP来说比较重要哦。
2. 将压缩包解压到你的目标磁盘目录，如解压后的目录类似C:\php，注意目录间最好不用空格，由于有的Web Server可能不支持带空格的路径。
3.  配置php5ts.dll路劲的环境变量。在根目录下面有些dll含有Web Server的名字，这些相关的Server模块可以让Web Server运行PHP时更加高效。所有的模块都需要用到php5ts.dll，因此需要让系统知道他的位置，查找顺序一般为：
>* php.exe的执行位置，或者Web Server的执行目录（一般为bin）如果Web Server使用了server模块。
>* 环境变量PATH下包含的路径。
2.  把当前的根目录加到PATH下，这样无论Web Server如何配置，系统都可以寻找到php5ts.dll
3. 配置PHP初始化信息，直接把php.ini-production复制后改名为php.ini即可，PHP运行时会自动查找并读取php.ini文件。另外如果使用Windows NT, 2000, XP 或 2003上的NTFS格式，确保运行Web Server的用户对php.ini有读取的权限。
4. 关联PHP和Apache,此配置后Apache便具有PHP的解析能力。这里有两种方式去设置PHP与Apache的协同工作。一种是作为CGI，另一种是作为Apache的模块来安装，上面提到Server模块更好，因此我采用这种方式安装，将以下三行加入Apache的httpd.conf中即可。
   >LoadModule php5_module "c:/php/php5apache2_2.dll"
   >AddType application/x-httpd-php .php
   > PHPIniDir "C:/php"

**集成检测**

 最后来检查下我们的环境是否正常工作。

1. 新建一个文本名称加扩展名为test.ini。
2. 在文件中添加如下代码：

><?php
>phpinfo();
>?>

3. 将其放到Apache Server的htdocs目录下。
4. 接着在浏览器中输入http://localhost/test.php,如果出现了PHP的版本及组件相关统计信息，说明正常工作了。其中mysqlnd为enable说明Mysql的驱动也正常启用了。

这样一个WAMP环境就搭建起来了，这个环境是进行开发与学习的基础，就先介绍到这里了。

