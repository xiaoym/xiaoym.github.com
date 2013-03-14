---
layout: post
title: "git submodule的使用"
description: ""
category: 
tags: [git submodule]
---
{% include JB/setup %}

#git submodule
早就使用git来作为版本管理工具，但是直到前几天才看到git还有submodule这个功能。submodule可以将别的git仓库作为当前项目的子项目来出来，比如说公司有一部分公用的代码，原来的做法是约定将公用的仓库放在与当前项目同级目录下，然后在project.properties文件中配置，即是这样的文件结构

>├── current_proj
>│   ├── libs
>│   └── project.properties
>└── lib_proj

current_proj是当前的项目,lib_proj是引用的项目。这样做唯一一点不爽就是在全新clone项目的时候必须特别小心的建立上面的结构，而且不能指定lib_proj的版本。
在了解了submodule后决定使用submodule来管理这样的需求。

##给项目增加submodule
    git submodule add git_repo_url local_path
上面的命令可以将git_repo_url指定的git仓库放置到local_path下。

## 同步submodule
    git submodule init
    git submodule update

上面的命令将同步submodule

##删除submodule
这个过程比较麻烦需要几步。
###step 1删除git cache和物理文件夹
git rm --cache local_path
rm -rf local_path

###step 2删除.gitmodules内容
git submodule add会在gitmodules中间增加相应的配置，删除时候需要将其删除

###step 3 删除.git/config中submodule配置
打开文件看看就知道了。

###step 4 sync
    git submodule sync
同步下确保没问题

###step 5 commit
将上面我们做的修改全部提交。

##注意事项
git仓库仅仅保存submodule的commit，不会保存具体的内容，所以请一定确保在submodule里面commit并且push了数据，让后再提交相应的修改，保证别人同步下来的submodule版本一直。