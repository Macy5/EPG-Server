![php-epg](https://socialify.git.ci/TakcC/PHP-EPG-Docker-Server/image?description=1&descriptionEditable=Docker%F0%9F%90%B3%20%E9%83%A8%E7%BD%B2%EF%BC%8C%E5%B8%A6%E8%AE%BE%E7%BD%AE%E7%95%8C%E9%9D%A2%EF%BC%8C%E6%94%AF%E6%8C%81%20DIYP%20%26%20%E7%99%BE%E5%B7%9D%20%E3%80%81%20%E8%B6%85%E7%BA%A7%E7%9B%B4%E6%92%AD%20%E4%BB%A5%E5%8F%8A%20xmltv%20%E3%80%82&font=Inter&forks=1&issues=1&language=1&name=1&owner=1&pattern=Circuit%20Board&pulls=1&stargazers=1&theme=Auto)

# PHP-EPG-Docker-Server 📺
![Docker Pulls](https://img.shields.io/docker/pulls/taksss/php-epg) ![Image Size](https://img.shields.io/docker/image-size/taksss/php-epg
)

PHP 实现的 EPG（电子节目指南）服务端， `Docker` 部署，自带设置界面，支持 **`DIYP & 百川`** 、 **`超级直播`** 以及 **`xmltv`** 格式。

## 主要功能 ℹ️
- 支持返回 **`DIYP & 百川`** 、 **`超级直播`** 以及 **`xmltv`** 格式 📡
- 一键生成匹配 `M3U` 的 `xmltv` 格式 `EPG` 文件 💯
- 提供 **`amd64`** 跟 **`arm64`** 架构 `Docker` 镜像，支持**电视盒子**等设备 🐳
- 基镜像采用 `alpine-apache-php`，**压缩后大小仅 `20M`** 📦
- 采用**先构建再存数据库**的策略，**提高读取速度** 🚀
- 支持**繁体中文频道匹配** 🌐
- 支持**多对一频道映射**，支持**正则表达式** 🔄
- 支持设置**频道忽略字符表** 🔇
- 支持生成**指定频道节目单** 📝
- 内置**定时任务**，支持设置定时拉取数据 ⏳
- 兼容多种 `xmltv` 格式 🗂️
- 使用 `SQLite` 数据库存储 🗃️
- 包含网页设置页面 🌐
- 支持多个 EPG 源 📡
- 可配置数据保存天数 📅
- 内置 `phpLiteAdmin` 方便管理数据库 🛠️

![设置页面](/pic/management.png)

> **内置正则表达式说明：**
> - 以 `regex:` 作为前缀
> - 示例：
>   - `regex:/^CCTV[-\s]*(\p{Han})/iu, $1` ：将 `CCTV风云足球`、`cctv-风云音乐` 等替换成 `风云足球`、`风云音乐`
>   - `regex:/^CCTV[-\s]*(\d+(\s*P(LUS)?|[K\+])?)(?!美洲|欧洲).*/i => CCTV$1` ：将 `CCTV 1综合`、`CCTV-4K频道`、`CCTV - 5+频道`、`CCTV - 5PLUS频道` 等替换成 `CCTV1`、`CCTV4K`、`CCTV5+`、`CCTV5PLUS`（排除 `CCTV4美洲` 和 `CCTV4欧洲`）
>   - `regex:/^(深圳.*?)频道$/i, $1` ：将 `深圳xx频道` 替换成 `深圳xx`

## 更新日志 📝

### 2024-8-24更新：

1. 新增：改用 `XMLWriter` 生成 `xmltv` 文件，加快生成速度，降低内存占用（生成全量数据，`100M` 以内即可）
2. 新增：“限定频道列表”可直接从 `M3U` 、 `TXT` 地址获取
3. 新增：设置“限定频道列表”后，生成 `xmltv` 的频道名以列表的为准（可生成匹配 `M3U` 的 `EPG` 文件）
4. 新增：`NewTV`、`SiTV`、`iHOT`、`CHC` 系列频道映射规则
5. 优化：将数据文件统一放到 `data` 文件夹，方便进行[数据持久化](#数据持久化)（⚠️注意更新）
6. 优化：节目匹配时，优先采用精确匹配结果
7. 优化：部分 `EPG` 条目跨天时，生成 2 条数据
8. 优化：`desc` 字段的生成逻辑
9. 优化：删除部分冗余选项

> ⚠️该版本改动较多，建议直接更新

### 2024-8-21更新：

1. 新增：退出按钮（感谢[mxdabc](https://github.com/mxdabc/epgphp)）
2. 修复：语法错误（感谢[mxdabc](https://github.com/mxdabc/epgphp)）
3. 新增：使用 `GitHub Actions` 生成、推送镜像到 `DockerHub` 及 `腾讯云容器镜像站`
4. 修复：点击退出按钮后再次登录，无法查看日志

### 2024-8-20更新：

1. 新增：同步提供 `腾讯云容器镜像` ，无法正常拉取镜像的用户可使用
2. 新增：默认返回“精彩节目”选项
3. 新增：更新前检查 EPG 文件，无变化则跳过
4. 优化：分批插入数据，降低内存占用
5. 优化：配置文件从 `config.php` 改为 `config.json`
6. 修复：频道映射每次只能更新一条的问题

### 其他见[更新日志](./CHANGELOG.md)

## TODO：

- [x] 支持返回超级直播格式
- [x] 整合更轻量的 alpine-apache-php 容器
- [x] 整合生成 xml 文件
- [x] 支持多对一频道映射
- [x] 支持繁体频道匹配
- [x] 仅保存指定频道列表节目单

## 部署步骤 🚀

1. 配置 `Docker` 环境

2. 若已安装过，先删除旧版本（注意备份数据）

   ```bash
   docker rm php-epg -f
   ```

3. 拉取镜像并运行：

   ```bash
   docker run -d \
     --name php-epg \
     -p 5678:80 \
     --restart always \
     taksss/php-epg:latest
   ```

   > 默认端口为 `5678` ，根据需要自行修改。
   > 无法正常拉取镜像的，可使用同步更新的 `腾讯云容器镜像`（`ccr.ccs.tencentyun.com/taksss/php-epg:latest`）

### 数据持久化
- 直接使用以下命令，`./data` 可根据自己需要更改
    ```bash
    docker run -d \
      --name php-epg \
      -v ./data:/htdocs/epg/data \
      -p 5678:80 \
      --restart always \
      taksss/php-epg:latest
     ```

- 新建 [`docker-compose.yml`](./docker-compose.yml) 文件后，在同目录执行 `docker-compose up -d`

## 使用步骤 🛠️

1. 在浏览器中打开 `http://{服务器IP地址}:5678/epg/manage.php`
2. **默认密码为空**，根据需要自行设置
3. 添加 `EPG 源地址`， GitHub 源确保能够访问，点击 `更新配置` 保存
4. 点击 `更新数据库` 拉取数据，点击 `数据库更新日志` 查看日志，点击 `查看数据库` 查看具体条目
5. 设置 `定时任务` ，点击 `更新配置` 保存，点击 `定时任务日志` 查看定时任务时间表

    > 建议从 `凌晨1点` 左右开始抓，很多源 `00:00 ~ 00:30` 都是无数据。
    > 隔 `6 ~ 12` 小时抓一次即可。

6. 点击 `更多设置` ，选择是否 `生成xml文件` 、`生成方式` ，设置 `限定频道节目单`
7. 用浏览器测试各个接口的返回结果是否正确：
    - `xmltv` 接口： `http://{服务器IP地址}:5678/epg/index.php`
    - `DIYP&百川` 接口： `http://{服务器IP地址}:5678/epg/index.php?ch=CCTV1`
    - `超级直播` 接口： `http://{服务器IP地址}:5678/epg/index.php?channel=CCTV1`
8. 将 **`http://{服务器IP地址}:5678/epg/index.php`** 填入 `DIYP`、`TiviMate` 等软件的 `EPG 地址栏`
    - ⚠️ 直接使用 `docker run` 运行的话，可以将 `:5678/epg/index.php` 替换为 **`:5678/epg`**。
    - ⚠️ 部分软件不支持跳转解析 `xmltv` 文件，可直接使用 **`:5678/epg/t.xml.gz`** 或 **`:5678/epg/t.xml`** 访问。

> **快捷键：**
> - `Ctrl + S`：保存设置
> - `Ctrl + /`：对选中 EPG 地址设置（取消）注释

**设置定时任务**

![设置定时任务](/pic/cronSet.png)

**定时任务日志**

![定时任务日志](/pic/cronLog.png)

**更新日志**

![更新日志](/pic/updateLog.png)

**查看、搜索频道**

![查看频道列表](/pic/channels.png)

**更多设置**

![更多设置](/pic/moresetting.png)

**phpLiteAdmin**

![phpLiteAdmin](/pic/phpliteadmin.png)

## 效果示例 🖼️

**DIYP**

![DIYP 示例](/pic/DIYP.png)

**TiviMate**

![TiviMate](/pic/TiviMate.jpg)


## 特别鸣谢 🙏
- [ChatGPT](https://chatgpt.com/)
- [celetor/epg](https://github.com/celetor/epg)
- [sparkssssssssss/epg](https://github.com/sparkssssssssss/epg)
- [Black_crow/xmlgz](https://gitee.com/Black_crow/xmlgz)
- [DIYP](https://diyp.112114.xyz/)
- [EPG 51zmt](http://epg.51zmt.top:8000/)
