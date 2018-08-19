---
title: Hexo 简明教程
date: 2018-08-16 15:28:36
tags: 
	- Hexo
	- Tutorial
categories:
	- Tutorial
	- Markdown
---

Hexo 是一个基于 node.js 的博客框架。它简单、强大，支持 Markdown 写作，一键就可以部署到 [GitHub](https://github.com/) 或 [Coding](https://coding.net/) 平台，瞬间拥有个人博客网站，是程序员、技术宅居家必备利器。同时 Hexo 还有丰富的主题和插件可供定制和扩展，适合有折腾癖好的同学深入探索。

这是一篇极简的使用教程，涉及到 Hexo 安装、配置、部署以及网站备份（方便多地写作更新）等内容。基本满足了想快速上手使用的同学的需求。更详细的使用方法，可以参考 [Hexo 中文文档](https://hexo.io/zh-cn/docs/)和 [Hexo Next 主题中文文档](http://theme-next.iissnan.com/)。

<!-- more -->

## 安装

建议在 Linux 系统下安装和使用 Hexo，即便是 Win10 系统，也可以通过安装 Linux 子系统（WLS）方便地使用 Linux 环境。

安装前提
- git
- node.js

git 在各 Linux 发行版的仓库中都有，可通过系统的包管理工具安装，以 Ubuntu 为例：

```bash
sudo apt install git
```

node.js 需要先安装包管理工具 npm，详细的安装方法可参考 [npm 中文网](https://www.npmjs.com.cn/getting-started/installing-node/)。对于 Ubuntu 系统，可使用如下命令安装：

```bash
curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -
sudo apt-get install -y nodejs
```


安装 Hexo

```bash
sudo npm install -g hexo-cli
```

## 使用

### 建站

```bash
hexo init blog    # blog 是自己起的目录名
cd blog
npm install
```

完成后，blog 目录下结构如下：

```
├── _config.yml
├── package.json
├── package-lock.json
├── node_modules(目录)
├── scaffolds(目录)
├── source
|   ├── _drafts
|   └── _posts
└── themes(目录)
```

**_config.yml**

网站配置信息（标题，作者，主题、部署等等）

**package.json/package-lock.json**

Hexo 是一个 node.js 应用，可以通过 npm 安装各种扩展包，这两个文件就记录了安装的包信息。

**node_modules 目录**

这个目录中存放通过 npm 安装的包

**scaffolds 目录**

模版文件夹。当您新建文章时，Hexo 会根据 scaffold 来建立文件。

**source 目录**

这个目录就是用户写文章的地方，_post 目录下存放用户写的 Markdown 文件会被解析成 HTML 并放入 public 文件夹中。

**theme 目录**

主题文件夹，Hexo 会根据主题来生成不同风格的页面。

### 写作

#### 新建文章

```bash
hexo new "Write blog using markdown"
# 或简写为
hexo n "Write blog using markdown"
```

该命令会在 _post 目录下生成文件 write-blog-using-markdown.md。可以在文件开头设置标题，时间、标签，分类等，如下：

```
---
title: 用 Markdown 写博客
date: 2018-08-13 09:22:18
tags:
    - markdown
    - blog
categories:
    - tutorial
    - markdown
---
```

接着这部分就是文章的正文，遵循 Markdown 格式。

#### 生成静态页面

```bash
hexo generate
# 或简写为
hexo g
```

#### 启动网站

```bash
hexo server
# 或简写为
hexo s
```

打开浏览器，在地址栏中输入 http://localhost:4000 就可以看到自己的博客了。本地网站只是方便开发时预览效果，其他人无法通过互联网访问。

*如果无法显示，可能是 4000 端口被占用了，可以使用如下命令指定端口*
```bash
hexo s -p 4444
```
*或者修改 node_modules/hexo-server/index.js 文件，修改默认端口*

```js
hexo.config.server = assign({
  port: 4444,
  log: false,
  ip: '0.0.0.0',
  compress: false,
  header: true
}, hexo.config.server);
```

#### 插入图片

在 _config.yml 开启资源文件夹

```
post_asset_folder: true
```

这样，在 _posts 目录下会生成一个与文章同名的文件夹。把需要插入到文章中的图片放到该文件夹中，并在写文章时通过如下标签引用即可。

```
{% asset_img <图片名> [图片标题] %}
```

### 部署到 Github 或 Coding

首先在 Github 中新建名为 jia-zhuang.github.io 的项目，这样部署成功后，就可以通过链接 https://jia-zhuang.github.io 来访问网站了。

*若是在 Coding 中，项目名应为 jiazhuang.coding.me，网站访问链接为 http://jiazhuang.coding.me*

安装 hexo-deployer-git

```bash
npm install hexo-deployer-git --save
```

修改 _config.yml 配置

```
deploy:
  - type: git
    repo: git@git.coding.net:jiazhuang/jiazhuang.coding.me.git
    branch: coding-pages
  - type: git
    repo: git@github.com:jia-zhuang/jia-zhuang.github.io.git
    branch: master
```

*注意：我们这里使用 coding-pages 分支部署网站，把 master 分支留给整个 Hexo 的备份；而Gibhub 个人主页只能使用 master 分支*

在 Github 和 Coding 中开启 Github pages 和 Coding pages 服务，选择对应分支。

## 使用 Next 主题

Hexo 默认的 Landscape 主题不够美观，换成流行的 Next 主题吧！通过对 Next 主题简单的设置，基本可以满足新手所有的需求。

### 安装和启用

```bash
git clone https://github.com/iissnan/hexo-theme-next themes/next
```

主题将会被下载到 theme/next 目录下。

在站点配置文件 _config.yml 更换主题：

```
#theme: landscape    # 注释掉这一行，换成下面
theme: next
```

### 配置 Next

Next 主题有自己的配置文件，位于 theme/next/_config.yml，里面有丰富的配置，详细可以参考 [Next中文文档](http://theme-next.iissnan.com/)。这里列出几个常用配置。

#### 更换风格

```
# Schemes
#scheme: Muse
scheme: Mist
#scheme: Pisces
#scheme: Gemini
```

一共有四种风格，可以通过注释来开启和关闭某个风格。


#### 开启数学公式支持

```
mathjax:
  enable: true
  per_page: true
  cdn: //cdn.bootcss.com/mathjax/2.7.1/latest.js?config=TeX-AMS-MML_HTMLorMML
```

#### 添加邮箱和 Github 链接

```
social:
  GitHub: "https://github.com/jia-zhuang || github"
  E-Mail: mailto:jiazzzz@qq.com || envelope
```

#### 设置菜单

```
menu:
  home: / || home
  tags: /tags/ || tags
  categories: /categories/ || th
  archives: /archives/ || archive
```

这样会在主页增加 tags 和 categories 菜单（链接），还需生成相应的页面：

```bash
hexo g page tags
hexo g page categories
```

这样会在 source 目录下生成 tags 和 categories 目录。分别打开两个目录中的 index.md，可以修改页面标题，但需要设置 type 字段：

```
---
title: 标签
date: 2018-08-14 09:13:19
type: tags
---
```
```
---
title: 分类
date: 2018-08-14 09:13:19
type: categories
---
```

#### 修改头像

将图片放入 theme/next/source/images 目录下，并在 Next 配置文件 theme/next/_config.yml 文件中引用图片：

```
avatar: /images/gene.jpg
```

## 其他资源

除 Next 之外，Hexo 还有非常丰富的主题，可以浏览 [Hexo 官网](https://hexo.io/themes/)寻找适合自己口味的主题。

随着博客数量和网站需求的增加，可能需要多种多样的扩展功能，可以在 [Hexo 官网插件页面](https://hexo.io/plugins/)找当满足自己需求的插件。

