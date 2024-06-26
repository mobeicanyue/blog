---
title: 如何在博客中使用霞鹜文楷字体
tags:
  - Hexo
  - Fluid
abbrlink: f24b41b1
date: 2023-12-27 14:58:04
---

近来阅读竹林里有冰的博客，发现他的博客字体很好看，想着把他的字体也用到我的博客里，于是就有了这篇文章。

## 1. 字体介绍
[霞鹜文楷字体仓库](https://github.com/lxgw/LxgwWenKai)
霞鹜文楷 我之前其实在手机上用过，不过后面由于装机加上懒得折腾就没有再设置了，但是博客我觉得为了好的阅读体验还是可以换一下的
![霞鹜文楷](https://pic1.zhimg.com/80/v2-c1e0f7f058fbbf1cc6ac3aa0a2480178_1440w.webp)

关于在 web 中使用 霞鹜文楷 请参阅 [issue#24](https://github.com/lxgw/LxgwWenKai/issues/24)

其中 [chawyehsu](https://github.com/chawyehsu/lxgw-wenkai-webfont) 提供了打包，竹林里有冰教程中引用的就是其打包字体，但是点进去仓库发现已经有半年没更新了。在 issue 的下面我们可以看到 [CMBill](https://github.com/CMBill/lxgw-wenkai-screen-web) 提供了一个新的打包，我的教程就是这个打包。

在 [CMBill](https://github.com/CMBill/lxgw-wenkai-screen-web) 的仓库 README 中跳转 [css](https://cdn.jsdelivr.net/npm/lxgw-wenkai-screen-web/style.css) 看看内容：

```css
@import url('./lxgwwenkaigbscreen/result.css');
@import url('./lxgwwenkaimonogbscreen/result.css');
@import url('./lxgwwenkaimonoscreen/result.css');
@import url('./lxgwwenkaiscreen/result.css');
```


可以发现这个 css 分了几个不同的字体种类，有不同的霞鹜文楷变体可供选择
将 `style.css` 替换为 `@import url` 之后的内容（去掉./），就可以直接使用了

我的博客字体文件为
`https://cdn.jsdelivr.net/npm/lxgw-wenkai-screen-web/lxgwwenkaiscreen/result.css`

## 2. 使用方法

### 2.1 直接引用
直接将文后提供的链接以 `<link>` 的形式添加到网页的 `<head>` 内即可。
```html
<html>
<head>
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/lxgw-wenkai-screen-web/lxgwwenkaiscreen/result.css" />
  <style>
    body {
      font-family: "LXGW WenKai Screen";
      font-weight: normal;
    }
  </style>
</head>
<body>
  
</body>
</html>
```

### 2.2 Hexo 博客
如果你使用的是 Hexo 博客，可以采用注入的方式，将字体 css 注入到你的博客中。
首先在根目录创建一个 `scripts` 文件夹，然后在 `scripts` 文件夹中创建一个 `font.js` 文件，内容如下：
```javascript
hexo.extend.injector.register('head_end',
'<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/lxgw-wenkai-screen-web/lxgwwenkaiscreen/result.css" />',
'default');
```
然后修改你的主题配置文件
```yaml
font:
  font_size: 17px
  font_family: "LXGW WenKai GB"
  letter_spacing: 0.02em
  code_font_size: 85%
```

### 2.3 Fluid 主题
如果你使用 Hexo 的 Fluid 主题，那么恭喜你，替换字体很简单，只需要在主题的配置文件里加上一行 `custom_css: https://xxxx`代码就可以了，再修改 font_family。比如我的是 `fontFamily: LXGW WenKai GB`。

```yaml
# 主题字体配置
font:
  font_size: 17px
  font_family: "LXGW WenKai GB"
  letter_spacing: 0.02em
  code_font_size: 85%

custom_js:
custom_css: https://cdn.jsdelivr.net/npm/lxgw-wenkai-screen-web/lxgwwenkaiscreen/result.css
```
## 3. 效果展示
![效果](https://pic4.zhimg.com/80/v2-7a56f955ee9907745c1445b4efed89fb_1440w.webp)
