# PHP-EPG-Docker-Server 📺

PHP-EPG-Docker-Server 是一个用 PHP 实现的 EPG（电子节目指南）服务端， `Docker` 部署，自带设置界面，支持 `xmltv` 和 `DIYP & 百川` 格式。

## 主要功能 ℹ️
- **使用 Docker🐳 部署，提供 `amd64` 跟 `arm64` 架构镜像**
- 支持标准的 `xmltv` 和 `DIYP & 百川` 格式 📡
- 内置定时任务，支持设置定时拉取数据 ⏳
- 使用 `SQLite` 数据库存储 🗃️
- 包含网页设置页面 🌐
- 支持多个 EPG 源 📡
- 可配置数据保存天数 📅
- 支持设置频道忽略字符串 🔇
- 支持频道映射，支持**正则表达式** 🔄
- 内置 `phpLiteAdmin` 方便管理数据库 🛠️

![设置页面](/pic/management.png)

![定时任务日志](/pic/cronLog.png)

![更新日志](/pic/updateLog.png)

> **内置正则表达式说明：**
> 
> - 以 `regex:` 作为前缀
> 
> - 示例：
> 
>   - `regex:/^CCTV[-\s]*(\p{Han})/iu, $1` ：将 `CCTV风云足球`、`cctv-风云音乐` 等替换成 `风云足球`、`风云音乐`
> 
>   - `regex:/^CCTV[-\s]*(\d+[K\+]?)(?!美洲|欧洲).*/i, CCTV$1` ：将 `CCTV 1综合`、`CCTV-4K频道`、`CCTV - 5+频道` 等替换成 `CCTV1`、`CCTV4K`、`CCTV5+`（排除 `CCTV4美洲` 和 `CCTV4欧洲`）
> 
>   - `regex:/^(深圳.*?)频道$/i, $1` ：将 `深圳xx频道` 替换成 `深圳xx`

# 更新日志

## 2024-7-18更新：

1. 提供 Docker🐳 镜像（基于 php:7.4-apache ，支持 x86-64 跟 arm64 ）
2. 支持定时更新数据库
3. EPG 源支持添加注释
4. 支持更改登录密码 【默认为空！！！】
5. 支持查看定时任务日志
6. 支持查看数据库更新日志
7. 配置页面支持 Ctrl+S 保存
8. 更新部署流程

### TODO：

- 整合更轻量的 alpine-apache-php 容器
- 支持繁体字匹配
- 支持返回超级直播格式


## 2024-7-14更新：

1. 改用 Docker Compose🐳 部署
2. 更新部署流程

## 2024-7-13更新：

1. 优化自带正则表达式
2. 更新默认返回数据，供回放使用
3. 增加 TiviMate 示例图片

## 2024-7-13初始版本：

1. 支持标准 xmltv 和 DIYP&百川 格式
2. 包含网页设置页面
3. 支持多个 EPG 源
4. 可配置数据保存天数
5. 支持设置频道忽略字符串
6. 支持频道映射，支持正则表达式
7. 内置 phpLiteAdmin 方便管理数据库


## 部署步骤 🚀

1. 配置 `Docker` 环境

2. 拉取镜像并运行：

   ```bash
   docker run -d \
     --name php-epg \
     -p 5678:8080 \
     --restart always \
     taksss/php-epg:latest
   ```

      >
      > 默认端口为 `5678` ，根据需要自行修改。
      > 

3. 在浏览器中打开 `http://{服务器IP地址}:5678/epg/manage.php`

4. **默认密码为空**，根据需要自行设置

5. 添加 `EPG 源地址`， GitHub 源确保能够访问，点击 `更新配置` 保存

6. 点击 `更新数据库` 拉取数据，点击 `数据库更新日志` 查看日志，点击 `查看数据库` 查看具体条目

7. 设置 `定时任务` ，点击 `更新配置` 保存，点击 `定时任务日志` 查看定时任务时间表

![设置定时任务](/pic/cronSet.png)

![更新数据库](/pic/update.png)

![phpLiteAdmin](/pic/phpliteadmin.png)

## 使用步骤 🛠️
- 将 `http://{服务器IP地址}:5678/epg` 填入 `DIYP`、`TiviMate` 等软件的 `EPG 地址栏`


## 效果示例 🖼️

**DIYP**

![DIYP 示例](/pic/DIYP.png)

**TiviMate**

![TiviMate](/pic/TiviMate.jpg)

## 特别鸣谢 🙏
- [celetor/epg](https://github.com/celetor/epg)
- [sparkssssssssss/epg](https://github.com/sparkssssssssss/epg)
- [Black_crow/xmlgz](https://gitee.com/Black_crow/xmlgz)
- [DIYP](https://diyp.112114.xyz/)
- [EPG 51zmt](http://epg.51zmt.top:8000/)
