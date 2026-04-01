# NPU Occupancy Web

一个可部署到 GitHub Pages 的静态网页，用于管理 NPU 机器的 24 小时占用时间段。

## 功能

1. 机器信息录入（名称、型号、位置、备注）
2. 24 小时占用看板（按机器、按日期查看）
3. 用户预约时间段（输入占用目的，用户名首次进入时输入）
4. 冲突校验（时间段重叠会提示）
5. 管理员（用户名 `xukenan`）可删除机器、取消任意用户已占用小时
6. 空闲时段悬浮可「快速占用该小时」

## 本地运行

直接双击 `index.html` 即可，或使用任意静态服务器打开。

## 部署到 GitHub Pages

### 方式一：仓库根目录部署

1. 把 `npu-occupancy-web` 目录中的文件放到仓库根目录（或设置 Pages 根目录为该目录）。
2. 在 GitHub 仓库中进入 `Settings` -> `Pages`。
3. `Build and deployment` 选择 `Deploy from a branch`。
4. 分支选择 `main`，目录选择 `/ (root)` 或 `/npu-occupancy-web` 对应的发布目录。
5. 保存后等待部署完成，访问 GitHub 提供的网址。

### 方式二：使用 `docs` 目录

1. 把本目录内容复制到仓库 `docs/`。
2. GitHub Pages 目录选择 `/docs`。

## 数据存储方式

### 方式 A：Supabase（在线，可多端同步）

可跨浏览器/跨设备共享数据。

1. 在 [Supabase](https://supabase.com/) 创建项目。
2. 打开 SQL Editor，执行 `supabase.sql`。
3. 编辑 `config.js`：

```js
window.NPU_APP_CONFIG = {
  storageMode: "supabase",
  supabaseUrl: "https://<your-project>.supabase.co",
  supabaseAnonKey: "<your-anon-key>"
};
```

`Project Settings -> API` 中复制 URL 与 anon key。

### 方式 B：本地模式（不连在线数据库）

适合自有主机、内网或不想依赖 Supabase 云端的场景：数据存在**当前浏览器**的 `localStorage`，**不同电脑/浏览器之间不同步**。

编辑 `config.js`：

```js
window.NPU_APP_CONFIG = {
  storageMode: "local",
  supabaseUrl: "",
  supabaseAnonKey: ""
};
```

无需执行 `supabase.sql`。若部署环境**完全不能访问外网 CDN**，可暂时注释 `index.html` 里加载 `@supabase/supabase-js` 的那一行（本地模式下不会调用 Supabase SDK）。

### 部署

按上文 GitHub Pages 或任意静态服务器部署即可；页面顶部会显示当前后端状态。
