---
title: AList 安装配置教程
tags:
  - 部署
  - AList
abbrlink: 9c18a796
date: 2024-01-17 20:50:59
---

{% note secondary %}
AList 是一个支持多存储的文件列表 / WebDAV 程序，使用 Gin 和 Solidjs。本文为 Alist 部署教程，主要介绍 Alist 的配置及使用。参考官网教程：https://alist.nn.ci/zh/guide
{% endnote %}

## 1. 安装 Alist

{% fold info @1Panel 一键安装 %}
打开 `1Panel` 面板，点击 `应用商店`，搜索 `Alist`，点击 `安装` 即可。配置可以参考下面的 `docker-compose` 安装。

{% post_link 1panel-installation-and-usage-tutorial '1Panel 安装配置教程' %}
{% endfold %}

{% fold info @一键脚本安装 %}
仅适用于 Linux amd64/arm64 平台
打开命令行输入

```bash
curl -fsSL "https://alist.nn.ci/v3.sh" | bash -s install
```

alist 默认安装在 `/opt/alist` 中

可以通过以下命令来启动、关闭、重启、查看状态：
启动： `systemctl start alist`
关闭： `systemctl stop alist`
状态： `systemctl status alist`
重启： `systemctl restart alist`
{% endfold %}

{% fold info @docker-compose 安装 %}

docker-compose 的相关知识就不在这里赘述了，如果不了解可以自行搜索。

<br>

创建一个目录用于存放 `docker-compose.yml`
```bash
mkdir /etc/alist && cd /etc/alist
```
<br>

下载 `docker-compose.yml` 文件
```bash
wget https://alist.nn.ci/docker-compose.yml
```

<br>
运行容器
```bash
docker-compose up -d
```

<br>

注意 docker-compose.yml 文件中的 `volumes` 配置，后面要用到：

```yml
volumes:
    - '/etc/alist:/opt/alist/data'
```

{% endfold %}

{% fold @手动安装（适合 Windows） %}
打开 AList Release 下载待部署系统对应的文件。下载后解压，赋予文件执行权限后运行即可。Windows 推荐使用该方式安装。

参考：https://alist.nn.ci/zh/guide/install/manual.html
{% endfold %}

## 2.  Alist 初始化登录密码

**Alist 在登录之前需要初始化密码。**我们可以 `随机生成` 或者 `手动设置` 一个密码。

### 2.1 随机生成密码

- 通过 **一键脚本安装** 的，可以在安装路径下执行如下命令：
```bash
./alist admin random
```

- 通过 **doekcer-compose** 安装的，可执行如下命令。注意，如果你的容器名称不是 `alist`，需要将 `-it` 后面的 `alist` 替换为你的容器名称：
```bash
docker exec -it alist ./alist admin random
```

### 2.2 手动设置密码

`set ` 后面的 `NEW_PASSWORD` 即你要设置的密码。

- 通过 **一键脚本安装** 的，可以在安装路径下执行如下命令：
```bash
./alist admin set NEW_PASSWORD
```

- 通过 **doekcer-compose** 安装的，可执行如下命令。注意，如果你的容器名称不是 `alist`，需要将 `-it` 后面的 `alist` 替换为你的容器名称：
```bash
docker exec -it alist ./alist admin set NEW_PASSWORD
```

## 3. 配置 Alist

默认情况下，应用程序将在 http://localhost:5244 上启动。

我们还需要进行一些配置，才可以正常使用我们部署的网盘。浏览器访问上述链接，输入用户名 `admin` 和初始化后的 `密码`，点击登陆，点击页脚的 `管理` 进入管理页面。

### 3.1 添加存储

Alist 支持多种存储，包括本地存储、OneDrive、Google Drive 等。这里我们以本地存储为例。

1. 在 `Alist 的安装路径` 手动创建一个目录 `files` 用于存储网盘文件。一键脚本路径为 `/opt/alist/files`；docker-compose 路径为 `/etc/alist/files`。

2. 添加存储：
点击管理页面左边栏的 `存储`，然后点击`添加`，驱动选择 `本机存储`，点击`添加`

3. `挂载路径`填写 `/`，意味着这次添加的存储为 `Alist 网盘的根目录`。往下划，找到 `根文件夹路径`，也就是文件的物理存储路径。
  **一键脚本安装** 为 `/opt/alist/files`；
  **docker-compose 安装** 为 `/opt/alist/data/files`。（注意，这个是容器内的映射路径，不是宿主机的路径，参考上文 docker-compose 安装的 volumes 配置，`/etc/alist:/opt/alist/data`）

  也就是 `挂载路径(/)` --> `根文件夹路径(/opt/alist/files)`，这样就可以把 `/opt/alist/files` 映射到 `/`。也就是将 `/opt/alist/files` 映射到了网盘根目录。

网页划到最下面点击 `保存`

### 3.2 启用游客访问

**Alist 默认情况下不允许游客访问。**如果你希望游客访问你的网盘，可以按照以下步骤开启：

左边栏点击 `用户`，编辑 `guest` 用户，将 `停用` 取消勾选，点击保存。


### 3.3 启用索引

Alist 需要构建索引才能搜索文件，可以按照以下步骤开启搜索：

1. 转到索引页，选择一个搜索索引，并单击保存;
2. 保存索引后，单击构建索引来构建索引;
3. 现在你可以通过点击页面右上角的搜索块或使用快捷键 Ctrl + K 来搜索文件。

## 4. 更新 Alist

{% fold info @1Panel 一键安装 %}
打开 `1Panel` 面板，点击 `应用商店`，点击 `可升级`，找到 `Alist`，点击 `升级` 即可。
{% endfold %}

{% fold info @一键脚本安装 %}
```bash
curl -fsSL "https://alist.nn.ci/v3.sh" | bash -s update
```
{% endfold %}

{% fold info @docker-compose 安装 %}
```bash
docker-compose pull
docker-compose up -d
```
{% endfold %}

{% fold @手动安装 %}
下载最新版本的 Alist，解压后替换原有文件即可。
{% endfold %}

## 5. 高级配置

更多高级配置请参照官网 https://alist.nn.ci/zh/guide/advanced/
