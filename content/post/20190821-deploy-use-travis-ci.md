---
title: "使用Travis-CI部署Hugo，实现自动化部署"
date: 2019-08-21T14:17:57+08:00
lastmod: 2019-08-21T14:17:57+08:00
draft: false
keywords: ["hugo","travis"]
description: ""
tags: ["devOps","hugo","travis"]
categories: ["运维"]
---

<!--more-->

# 需求描述
Hugo是一个静态网站生成工具，如果每次都要手动编译然后上传着实麻烦，如果能每次编辑完提交之后使用自动化运维自动生成对应的网站，则回非常方便。

travis则提供了这样一个自动化运维的功能，而且对github的开源工程是免费的，和GitHub能较好的集成。

使用travis配置自动化部署需要如下几步：

## 步骤说明
1. 建立hugo 项目分支
2. blog分支中配置`.travis.yml` 
3. 申请github key
4. 在travis上配置

下面开始详细叙述。
# 建立hugo 项目分支

```
hugo new site blog
cd blog
git init
git remote add origin git@github.com:m2kar/m2kar.github.io.git
git checkout -b blog
git add .
git commit -m "first commit"
git push -u origin blog

```

然后在github的项目设置中把blog设置为主分支。

其他细节可以参考: https://mogeko.me/2018/018/ 不再赘述。

# blog分支中配置`.travis.yml`

```yml

language: go

branches:
  only:
    - blog  # 设置自动化部署的源码分支
    - 2-travis-auto-depoly # test for travis

# cache this directory
cache:
  directories:
    - themes
    # - public
    #- themes 主题没有更改时可以缓存

before_install:
- export TZ='Asia/Shanghai'  # 设置时区
  #- npm install -g hexo
  #- npm install -g hexo-cli

# 安装依赖组件
install:
  - wget https://github.com/gohugoio/hugo/releases/download/v0.51/hugo_0.51_Linux-64bit.tar.gz
  - tar -xzvf hugo_0.51_Linux-64bit.tar.gz
  - chmod +x hugo
  - export PATH=$PATH:$PWD
  - hugo version
    #- git clone -b ${TRAVIS_BRANCH} --recursive https://github.com/m2kar.github.io.git
    #- cd m2kar.github.io
  - git log -p -2 | cat
  - commit_msg=$(git log -n1 --pretty=format:"%s")
  - git clone --recursive -b master https://${GH_APIKEY}@github.com/m2kar/m2kar.github.io.git public
  - rm -rf public/*

script:
  - hugo -t even
  - cp CNAME public/
  - pushd public 
  - git add .
  - git commit -m "auto depoly ${commit_msg} "
  - git push
  - echo "depoly done"
```

参见： https://github.com/m2kar/m2kar.github.io/blob/blog/.travis.yml
# 申请github key

在 https://github.com/settings/tokens 申请一个token,并授予访问`public_repo`的权限，记录下来这个token。

# 在travis上配置
1. 访问 https://travis-ci.org
2. 激活 <用户名>.github.io 这个项目。
3. 在setting中打开"Build pushed branches"这个开关
4. 在setting中添加名为"GH_APIKEY"的环境变量,把刚才申请的token填进去，Branch选择`blog`分支。

配置完成。

# 在Linux/Mac上部署docker实现hugo

在本地进行测试的时候，也需要安装hugo程序。

docker是个非常方便的工具，可以方便进行各种程序的部署安装。

[jguyomard/hugo-builder](https://hub.docker.com/r/jguyomard/hugo-builder/) 提供了一个封装完好的hugo工具。

## 生成网站

```
docker run --rm -it -v $PWD:/src -u ${UID}:hugo jguyomard/hugo-builder hugo
```

## 启动hugo服务器并映射到1313端口
```
docker run --rm -it -v $PWD:/src -u ${UID}:hugo -p 1313:1313 jguyomard/hugo-builder hugo server --bind 0.0.0.0
```
## 使用alias方便调用

把下面两行放到`.zshrc`文件中
```bash
 alias hugo="docker run --rm -it -v $PWD:/src -u ${UID}:hugo jguyomard/hugo-builder hugo"
 alias hugo-server="docker run --rm -it -v $PWD:/src -u ${UID}:hugo -p 1313:1313 jguyomard/hugo-builder hugo server --bind 0.0.0.0"
 ```
执行`source ~/.zshrc`


--------
- **版权声明**：本文为 m2kar 的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
- **作者**: m2kar
- **打赏链接**: [欢迎打赏m2kar](http://m2kar-cn.mikecrm.com/wy97haW)
- **邮箱**: [m2kar.cn#gmail.com](mailto:m2kar.cn@gmail.com)
- **主页**: [m2kar.cn](https://m2kar.cn)
- **Github**: [github.com/m2kar](https://github.com/m2kar)
- **CSDN**: [M2kar的专栏](https://blog.csdn.net/still_night)

