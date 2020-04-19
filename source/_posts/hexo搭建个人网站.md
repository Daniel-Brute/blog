---
title: hexo搭建个人网站
categories: 
- 工具
tags: 
- hexo
---

# 1 工具安装步骤

涉及到安装的软件有nodejs、npm和hexo。

## 1.1 安装nodejs

下载地址：http://nodejs.cn/

安装成功后设置nodejs环境变量

```
NODE_PATH = "..."
```

## 1.2 安装npm

- 配置npm信息

```
npm -v（npm安装成功后npm自动安装）
npm config set cache "%NODE_PATH%\node_cache"
npm config set prefix "%NODE_PATH%\node_global"
npm config set registry=http://registry.npm.taobao.org
```

- 检查配置：

```
{用户目录}\.npmrc
npm install npm -g
npm list -global

```

- 配置环境变量

```
path: %NODE_PATH%\node_global\node_modules
```

## 1.3 安装hexo

```
npm install -g hexo-cli
hexo -v
```

# 2 初始化项目

## 2.1 创建项目

- 创建一个新的hexo项目

```
hexo init "项目名"
```

## 2.2 目录介绍

```
├── node_modules 依赖包
├── public #存放被解析markdown、html文件
├── scaffolds #当您新建文章时，根据 scaffold生成文件
├── source #资源文件夹
|   └── _posts #博客文章目录
└── themes #主题
├── _config.yml #网站的配置信息。标题、网站名称等
├── db.json #source解析所得到的
├── package.json # 应用程序的配置信息
```


## 2.3 简单配置

- 1）设置主页面信息 

```
title: ""
subtitle: ''
description: ''
keywords:
author: 狗蛋
language: zh-CN
timezone: ''
```

- 2）配置主题

主题模板地址：

```
https://hexo.io/themes/
```

配置方式：

```
theme: ayer
```

- 3）部署配置(_config.yml)

```
deploy:
  type: git
  repo: https://github.com/Daniel-Brute/Daniel-Brute.github.io.git
 # coding: git@git.dev.tencent.com:Beavan/Blog.git
 # 支持同时部署到多个Pages服务
  branch: master
```

## 2.4 加载插件

- 插件地址（官方）：

```
https://hexo.io/plugins/
```

- 安装插件：

```
npm install 插件名 --save
```

- 案例：加载文章中图片配置的插件

> （1）配置：

```
1.修改_config.yml配置
post_asset_folder: true

2. 将URL地址设置为网站的地址
# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: 'https://github.com/Daniel-Brute.github.io'
root: /
```

> （2）加载插件（非官方）：

```
npm install https://github.com/CodeFalling/hexo-asset-image --save
```

同时修改插件的index.jsp
```
'use strict';
var cheerio = require('cheerio');

// http://stackoverflow.com/questions/14480345/how-to-get-the-nth-occurrence-in-a-string
function getPosition(str, m, i) {
  return str.split(m, i).join(m).length;
}

var version = String(hexo.version).split('.');
hexo.extend.filter.register('after_post_render', function(data){
  var config = hexo.config;
  if(config.post_asset_folder){
        var link = data.permalink;
    if(version.length > 0 && Number(version[0]) == 3)
       var beginPos = getPosition(link, '/', 1) + 1;
    else
       var beginPos = getPosition(link, '/', 3) + 1;
    // In hexo 3.1.1, the permalink of "about" page is like ".../about/index.html".
    var endPos = link.lastIndexOf('/') + 1;
    link = link.substring(beginPos, endPos);

    var toprocess = ['excerpt', 'more', 'content'];
    for(var i = 0; i < toprocess.length; i++){
      var key = toprocess[i];
 
      var $ = cheerio.load(data[key], {
        ignoreWhitespace: false,
        xmlMode: false,
        lowerCaseTags: false,
        decodeEntities: false
      });

      $('img').each(function(){
        if ($(this).attr('src')){
            // For windows style path, we replace '\' to '/'.
            var src = $(this).attr('src').replace('\\', '/');
            if(!/http[s]*.*|\/\/.*/.test(src) &&
               !/^\s*\//.test(src)) {
              // For "about" page, the first part of "src" can't be removed.
              // In addition, to support multi-level local directory.
              var linkArray = link.split('/').filter(function(elem){
                return elem != '';
              });
              var srcArray = src.split('/').filter(function(elem){
                return elem != '' && elem != '.';
              });
              if(srcArray.length > 1)
                srcArray.shift();
              src = srcArray.join('/');
              $(this).attr('src', config.root + link + src);
              console.info&&console.info("update link as:-->"+config.root + link + src);
            }
        }else{
            console.info&&console.info("no src attr, skipped...");
            console.info&&console.info($(this));
        }
      });
      data[key] = $.html();
    }
  }
});
```

> （3）新建blog文件：

```
hexo new post 文件名
```

# 3 hexo项目的发布

以下命令操作必须在项目的**工作目录的根目录**下

- 清空hexo生成的静态文件

```
hexo clean
```

- 生成hexo的静态网页

```
 hexo g
```

- 本地部署hexo静态网站

```
 hexo s
```

- 将本地静态网站部署到仓库（_config中配置的repo地址）

```
hexo d
```