---
layout: post
title: "OSX中php mysql \"No such file\"的解决"
description: "osx中php mysql问题的解决"
category: "osx"
tags: ["osx", "php", "mysql"]
---
{% include JB/setup %}

我的mbp是从Lion升级到Moutain Lion的，并且在Lion中间启用的web服务（即是开启了php, apache），然后用mysql官网上提供的安装包安装的mysql。之所以没有使用macports安装是因为我想要一个很方便能够启动关闭mysql的工具，官方的安装包安装后会在系统设置里面提供一个按钮，方便关闭和开启。升级到MoutainLion后发现在共享里面压根就没有了web服务共享这一项，需要手动开启了，具体怎么开启就不啰嗦了。原来我在我本地搭建了Discuz方便测试，升级后发现启动不了了，提示MySQL的No Such File，这郁闷坏了，开始怀疑是数据库文件有问题，但是用navicat管理又完全没问题，so数据库是没问题的，后来仔细一想,php和mysql的链接很有可能是通过本地socket来做的，说的No Such File中的File是mysql.sock这个文件找不到了，来确定下。

##确定问题
弄了一个简单的php文件，内容如下:

    <?php
    php_info();_
    ?>
    
看了下输出：

    mysql.default_socket	/var/mysql/mysql.sock	/var/mysql/mysql.sock
    
然后在mysql的命令行里面输入status返回:

    UNIX socket:		/tmp/mysql.sock
    
明显的不匹配了，问题找到了。

##解决问题
要解决这个问题有两个办法修改1、mysql，2、修改php，我的mysql还要给我其他服务用呢，so不能随便动啊，这里修改php的配置，php配置文件在哪呢？在终端运行下面的命令:

    php -init
    
会给你下面的输出:

    Configuration File (php.ini) Path: /etc
    Loaded Configuration File:         /private/etc/php.ini
    Scan for additional .ini files in: (none)
    Additional .ini files parsed:      (none)

看到了吧，/private/etc/php.ini就是咱们的配置文件了，这个问题默认是没有写权限的，给添加上写权限以后用mvim打开，将/var/mysql/mysql.sock全部替换成 /tmp/mysql.sock
或者直接这样

    %s/\/var\/mysql\/mysql.sock/\/tmp\/mysql.sock/g
    
直接搞定。

##总结
这个问题应该以前就遇到过，只是没有记录下来，这里做个记录，好记性不如烂笔头么。
    