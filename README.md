![php-epg](https://socialify.git.ci/TakcC/PHP-EPG-Docker-Server/image?description=1&descriptionEditable=Docker%F0%9F%90%B3%20%E9%83%A8%E7%BD%B2%EF%BC%8C%E5%B8%A6%E8%AE%BE%E7%BD%AE%E7%95%8C%E9%9D%A2%EF%BC%8C%E6%94%AF%E6%8C%81%20DIYP%20%26%20%E7%99%BE%E5%B7%9D%20%E3%80%81%20%E8%B6%85%E7%BA%A7%E7%9B%B4%E6%92%AD%20%E4%BB%A5%E5%8F%8A%20xmltv%20%E3%80%82&font=Inter&forks=1&issues=1&language=1&name=1&owner=1&pattern=Circuit%20Board&pulls=1&stargazers=1&theme=Auto)

# PHP-EPG-Docker-Server 📺
![Docker Pulls](https://img.shields.io/docker/pulls/taksss/php-epg) ![Image Size](https://img.shields.io/docker/image-size/taksss/php-epg
)

PHP 实现的 EPG（电子节目指南）服务端， `Docker` 部署，自带设置界面，支持 **`DIYP & 百川`** 、 **`超级直播`** 以及 **`xmltv`** 格式。

## 主要功能 ℹ️
- 支持返回 **`DIYP & 百川`** 、 **`超级直播`** 以及 **`xmltv`** 格式 📡
- 提供 **`amd64`** 跟 **`arm64`** 架构 `Docker` 镜像，支持电视盒子等设备 🐳
- 基镜像采用 `alpine-apache-php`，**压缩后大小仅 `20M`** 📦
- 采用**先构建再存数据库**的策略，存在部分冗余数据，但能**提高读取速度** 🚀
- 支持**繁体中文频道匹配** 🌐
- 支持 **多对一频道映射**，支持**正则表达式** 🔄
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
> 
> - 以 `regex:` 作为前缀
> 
> - 示例：
> 
>   - `regex:/^CCTV[-\s]*(\p{Han})/iu, $1` ：将 `CCTV风云足球`、`cctv-风云音乐` 等替换成 `风云足球`、`风云音乐`
> 
>   - `regex:/^CCTV[-\s]*(\d+(\s*P(LUS)?|[K\+])?)(?!美洲|欧洲).*/i => CCTV$1` ：将 `CCTV 1综合`、`CCTV-4K频道`、`CCTV - 5+频道`、`CCTV - 5PLUS频道` 等替换成 `CCTV1`、`CCTV4K`、`CCTV5+`、`CCTV5PLUS`（排除 `CCTV4美洲` 和 `CCTV4欧洲`）
> 
>   - `regex:/^(深圳.*?)频道$/i, $1` ：将 `深圳xx频道` 替换成 `深圳xx`

## 更新日志 📝

### 2024-8-14更新：

1. 优化：频道映射显示方式
2. 新增：同时生成 `.xml.gz` 及 `.xml` 选项

### 2024-8-12更新：

1. 新增：`xmltv` 文件格式选项（ `.xml.gz` 或 `.xml`）
2. 优化：频道映射忽略空格（如 `CGTN英语 => CGTN` 可对 `CGTN 英语` 进行映射）
3. 优化：正则表达式，增加 `CCTV 5P` 频道匹配

### 2024-8-5更新：

1. 修改部分 `opencc` 及 `update.php` 代码，兼容 `PHP 7.0` 以上版本
2. 新增 `入库前处理频道名` 选项（ `DIYP` 跟 `超级直播` 用户保持默认 `是` 即可）
3. 更新 `manage.php` ，打开设置页面时检查定时任务运行情况
4. 优化正则表达式，增加 `CCTV 5PLUS` 频道匹配
5. 新增 `docker-compose.yml` ，可持久化 `adata.db` 跟 `config.php` 文件

- 亦可使用以下命令，需确保 `当前目录` 包含上述文件
  ```bash
  docker run -d \
   --name php-epg \
   -v ./adata.db:/htdocs/epg/adata.db \
   -v ./config.php:/htdocs/epg/config.php \
   -p 5678:80 \
   --restart always \
   taksss/php-epg:latest
  ```

### 2024-7-31更新：

1. 部分用户反馈 `phpLiteAdmin-ng` 在 `php7.x` 上打开较慢，已换回之前的 `phpLiteAdmin 1.9.71`
2. 修复 `phpLiteAdmin 1.9.71` 部分节目不显示的问题：
   - 将部分 `substr` 、 `strlen` 替换为 `mb_substr` 、 `mb_strlen`
3. 修复未进入“更多设置”页面，`Ctrl+S` 会清空限定频道列表的问题

### 2024-7-30更新：

1. 支持**繁体中文频道匹配**（感谢 [overtrue/php-opencc](https://github.com/overtrue/php-opencc) ）
2. 支持生成**指定频道节目单**（可通过 `.txt` 或 `.m3u` **节目源导入**）
3. 调整页面布局，将部分设置放至“**更多设置**”页面中
4. 生成 .xml.gz 时，默认只包含预告数据（可在设置中更改）
5. 修改频道忽略字符串逻辑，将忽略空格设为可选（默认开启）
6. EPG 地址列表支持 `Ctrl + /` 设置（取消）注释
7. 重构部分代码，加快数据生成速度，降低内存占用
8. 改用 [phpLiteAdmin-ng](https://github.com/emanueleg/pla-ng/) ，修复节目显示不完全的问题
9. 修复频道包含 `&` 字符，报 `Warning` 的问题
10. 修复 cron.php 首次执行在特定情况下异常的问题

>
> 功能基本完善，没大改动的话会比较少更新了。
> 

### 2024-7-26更新：

1. 自定义频道名支持**多对一映射**
2. 支持**整合 xmltv**，并**生成 .xml.gz 文件**，降低硬盘占用
3. 支持查看、搜索数据库频道列表，方便填写映射
4. 修复 cron.php 在第三天失效的问题
5. 删除 epg_lovetv 表，超级直播数据直接从对应 DIYP 中生成，减少数据冗余
6. 优化更新当天数据的逻辑，避免更新时无法获取节目表

### 2024-7-21更新：

1. **支持 `超级直播` 格式**
2. **重构代码，基镜像改为 `alpine-apache-php` ，镜像大小从 155M 下降到 23M**
3. 支持解析 M3U4U 等非 .xml/.gz 结尾 EPG 地址
4. 数据分批插入，降低内存占用
5. 修复部分界面显示异常问题
6. 修复设置页面刷新，提示“是否重新提交表单”问题
7. 增加接口测试说明

### 2024-7-18更新：

1. 提供 Docker🐳 镜像（基于 php:7.4-apache ，支持 x86-64 跟 arm64 ）
2. 支持定时更新数据库
3. EPG 源支持添加注释
4. 支持更改登录密码 【默认为空！！！】
5. 支持查看定时任务日志
6. 支持查看数据库更新日志
7. 配置页面支持 Ctrl+S 保存
8. 更新部署流程

### 2024-7-14更新：

1. 改用 Docker Compose🐳 部署
2. 更新部署流程

### 2024-7-13更新：

1. 优化自带正则表达式
2. 更新默认返回数据，供回放使用
3. 增加 TiviMate 示例图片

### 2024-7-13初始版本：

1. 支持标准 xmltv 和 DIYP&百川 格式
2. 包含网页设置页面
3. 支持多个 EPG 源
4. 可配置数据保存天数
5. 支持设置频道忽略字符串
6. 支持频道映射，支持正则表达式
7. 内置 phpLiteAdmin 方便管理数据库

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

   >
   > 默认端口为 `5678` ，根据需要自行修改。
   > 

- `adata.db` 跟 `config.php` 文件持久化：
  - 下载 `源代码` 后，在 `项目根目录` 执行 `docker-compose up -d` 部署（后续升级无需重新下载源代码）
  - 或执行以下命令 ，需确保 `当前目录` 存在 `adata.db` 及 `config.php` 文件
    ```bash
    docker run -d \
      --name php-epg \
      -v ./adata.db:/htdocs/epg/adata.db \
      -v ./config.php:/htdocs/epg/config.php \
      -p 5678:80 \
      --restart always \
      taksss/php-epg:latest
     ```


## 使用步骤 🛠️

1. 在浏览器中打开 `http://{服务器IP地址}:5678/epg/manage.php`

2. **默认密码为空**，根据需要自行设置

3. 添加 `EPG 源地址`， GitHub 源确保能够访问，点击 `更新配置` 保存

4. 点击 `更新数据库` 拉取数据，点击 `数据库更新日志` 查看日志，点击 `查看数据库` 查看具体条目

5. 设置 `定时任务` ，点击 `更新配置` 保存，点击 `定时任务日志` 查看定时任务时间表

6. 点击 `更多设置` ，选择是否 `生成xml文件` 、`生成方式` ，设置 `限定频道节目单`

7. 用浏览器测试各个接口的返回结果是否正确：

    - `xmltv` 接口： `http://{服务器IP地址}:5678/epg/index.php`
    
    - `DIYP&百川` 接口： `http://{服务器IP地址}:5678/epg/index.php?ch=CCTV1`
    
    - `超级直播` 接口： `http://{服务器IP地址}:5678/epg/index.php?channel=CCTV1`

8. 将 **`http://{服务器IP地址}:5678/epg/index.php`** 填入 `DIYP`、`TiviMate` 等软件的 `EPG 地址栏`

    - ⚠️ 直接使用 `docker run` 运行的话，可以将 `:5678/epg/index.php` 替换为 `:5678/epg`。
    - ⚠️ 部分软件不支持跳转解析 `xmltv` 文件，可直接使用 **`:5678/epg/t.xml.gz`** 或 **`:5678/epg/t.xml`** 访问。

>
> **快捷键：**
>
> - `Ctrl + S`：保存设置
> 
> - `Ctrl + /`：对选中 EPG 地址设置（取消）注释
> 

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
