---
title: Hexo 安装 Fluid 主题
tags:
  - Hexo
  - Fluid
  - 部署
abbrlink: 5eba38dc
date: 2023-12-27 02:09:48
---

[官方文档](https://hexo.fluid-dev.com/docs/start)其实挺详细的了。本文记录一下自己的操作。

## 1. 安装主题

### 1.1 安装方式一：从 NPM 安装

在博客目录执行命令：
```bash
npm install --save hexo-theme-fluid
```
然后在博客根目录下创建 _config.fluid.yml，将官方仓库的主题配置 [_config.yml](https://github.com/fluid-dev/hexo-theme-fluid/blob/master/_config.yml) 内容复制过去。

现在目录下有这三个 yml 文件：
![yml-files](https://pic2.zhimg.com/80/v2-052fdc56939475513bf83af1bd6b1bb5_1440w.webp)

可删除 `_config.landscape.yml` , 它是 hexo 默认的主题文件。

### 1.2 安装方式二：解压 GitHub 源码包

下载 [主题最新 release 版本](https://github.com/fluid-dev/hexo-theme-fluid/releases) 解压到 themes 目录，并将解压出的文件夹重命名为 fluid。注意，这种方式安装的主题自带配置文件 `_config.yml`，位于主题根目录，不需要手动创建。

这种方式适用于想灵活修改主题的用户，因为 Fluid 更新较慢，如果你有一些自定义需求，可以直接修改源码。

## 2. 修改 hexo 配置

修改 Hexo 博客目录中的 `_config.yml`，将 `theme` 属性改为 `fluid`, `language` 属性改为 `zh-CN`：
```yml
theme: fluid

language: zh-CN
```

## 3. 创建「关于页」

Fluid 默认不会创建「关于页」，需要手动创建。

执行以下命令：
```bash
hexo new page about
```

创建成功后修改 `/source/about/index.md`，添加 `layout` 属性：
```
---
title: about
date: 2023-12-26 22:43:21
layout: about
---
这里写关于页的正文，支持 Markdown, HTML
```
不出意外你就可以看到关于页了。

执行以下命令：
```bash
hexo clean && hexo g && hexo s -o
```

后访问 `http://localhost:4000/about/` 即可看到效果。

![关于页](https://pic2.zhimg.com/80/v2-c807849599bc8439c765cf501ca36419_1440w.webp)

PS: 可以跟着文档把关于页面的几个 icon 一起改了。

## 4. 修改主题配置

[官方文档](https://hexo.fluid-dev.com/docs/guide/) 比较完善，耐心看完即可，有详细的配置说明。

## 5. 修改网站图标
`修改网站图标` 文档好像没提到，我这里写一下
首先把你的图标放到 `/source/images/` 目录下，然后
打开 `_config.fluid.yml` 找到这个配置：
```yml
# 用于浏览器标签的图标
# Icon for browser tab
favicon: images/favicon.png

# 用于苹果设备的图标
# Icon for Apple touch
apple_touch_icon: images/favicon.png
```
将 `favicon.png` 改为你的图标路径即可。

## 6. 修改 slogan 为 api 语录
效果如图所示：
![语录](https://pic4.zhimg.com/80/v2-de40d65e16c881935c1acc00eb51ab0f_1440w.webp)

在主题配置 `_config.fluid.yml` 中开启：
```yml
index:
  slogan:
    enable: true
    text: 这是一条 Slogan
    api:
      enable: true
      url: "https://v1.hitokoto.cn/"
      method: "GET"
      headers: {}
      keys: ["hitokoto"]
```
把 `url` 改为你想要的 api 地址，`keys` 改为你想要的字段。具体参数可以看[官方文档](https://hexo.fluid-dev.com/docs/guide/#slogan-%E6%89%93%E5%AD%97%E6%9C%BA)

## 7. 修改背景图片 为 api 图片
既然可以改 slogan 为 api 语录，那么背景图片当然也可以改为 api 图片 笑）。

效果如图所示：
![api 图片](https://pic4.zhimg.com/80/v2-6a515a3f1baedebcec5499c1d20eb467_1440w.webp)

在主题配置 `_config.fluid.yml` 中搜索
```yml
/img/default.png
```
将其改为你想要的 api 地址即可。
我使用的是 [Bing 每日图片](https://api.vvhan.com/api/bing)
感谢接口提供者！！！