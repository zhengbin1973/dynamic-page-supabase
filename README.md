# 投资仪表盘 - Supabase PostgreSQL 版

数据存储在 Supabase 云端 PostgreSQL 数据库，重启/休眠后数据不会丢失。

## 部署步骤

### 第一步：创建 Supabase 项目（免费）

1. 打开 https://supabase.com → 点 **Start your project**
2. 用 **GitHub 账号登录**
3. 点 **New Project**
4. 填写：
   - **Name**：`investment-dashboard`（随便取）
   - **Database Password**：设一个密码，**记下来！** 后面要用
   - **Region**：选 **Northeast Asia (Tokyo)** 或 **Southeast Asia (Singapore)**（离中国近，延迟低）
5. 点 **Create new project**，等待 1-2 分钟初始化完成

### 第二步：获取数据库连接字符串

1. 在 Supabase 控制台，左侧点 **Project Settings**（齿轮图标）
2. 点 **Database**
3. 找到 **Connection string** 区域
4. 选 **URI** 标签
5. 复制连接字符串，格式类似：
   ```
   postgresql://postgres.[项目ID]:[密码]@aws-0-[区域].pooler.supabase.com:6543/postgres
   ```
6. 把 `[YOUR-PASSWORD]` 替换成你在第一步设的数据库密码

> ⚠️ **重要**：直接复制的是带占位符的，一定要把密码替换进去！

### 第三步：创建 GitHub 仓库并上传文件

需要上传 **3 个文件**：

| 文件 | 说明 |
|---|---|
| `app.py` | 主程序 |
| `requirements.txt` | 依赖清单 |
| `Dockerfile` | Docker 配置 |

1. 打开 https://github.com/new
2. 名称填 `investment-dashboard-supabase`
3. 选 **Public**，点 Create
4. 点 **uploading an existing file**，把 3 个文件拖进去，Commit

### 第四步：部署到 Render

1. 打开 https://render.com → 用 **GitHub 登录**
2. 点 **New** → **Web Service**
3. 连接 GitHub，选 `investment-dashboard-supabase` 仓库
4. 配置：
   - **Name**：`investment-dashboard`
   - **Runtime**：选 **Docker**
   - **Instance Type**：Free
5. 点 **Advanced** → 添加环境变量：

   | Key | Value |
   |---|---|
   | `DATABASE_URL` | 第二步复制的连接字符串（已替换密码） |

6. 点 **Create Web Service**

### 第五步：等待部署

首次构建 Docker 镜像需要 3-5 分钟，状态变成 **Live** 就成功了。

点击上面的链接访问仪表盘，首次访问会自动初始化数据库并写入演示数据。

## 与之前版本的区别

| | SQLite 版 | Supabase 版 |
|---|---|---|
| 数据存储 | 本地文件 / Render 临时磁盘 | Supabase 云端 PostgreSQL |
| 数据持久性 | Render 免费版可能丢失 | ✅ 永久保存 |
| 免费额度 | 无限制 | 500 MB 存储，足够个人用 |
| 多设备同步 | ❌ | ✅ 换电脑也能访问同一份数据 |
| 离线使用 | ✅ | ❌ 需要网络连接 |

## 环境变量说明

| 变量名 | 说明 | 示例 |
|---|---|---|
| `DATABASE_URL` | PostgreSQL 连接字符串 | `postgresql://postgres:xxx@xxx.supabase.com:6543/postgres` |
