# Image Background Remover — MVP 需求文档

> 版本：v0.1 | 日期：2026-03-24 | 状态：草稿

---

## 一、产品定位

一个在线图片去背景工具，用户上传图片，自动去除背景，下载透明 PNG。

目标关键词：**image background remover**

核心价值：免费、快速、无需注册、不存储用户图片。

---

## 二、技术架构

**前端框架：** React + Vite

**部署平台：** Cloudflare Pages + Cloudflare Pages Functions

**去背景 API：** Remove.bg（HTTP API）

**数据存储：** 无（图片处理完即丢弃，不落盘）

### 请求链路

```
用户上传图片
    ↓
前端（React）发送到 /api/remove-bg
    ↓
Cloudflare Pages Function 调用 Remove.bg API
    ↓
返回透明 PNG
    ↓
前端展示对比图 + 提供下载
```

---

## 三、页面结构（MVP）

### 3.1 首页（唯一页面）

**区域 1：Hero 区**
- 标题：Image Background Remover
- 副标题：Remove image backgrounds instantly. Free, fast, no sign-up.
- 上传入口（拖拽 / 点击上传）

**区域 2：处理区（上传后显示）**
- 左：原图预览
- 右：去背景结果图（处理中时显示 loading）
- 下载按钮：Download PNG

**区域 3：说明区**
- 支持格式：JPG、PNG、WEBP
- 文件大小限制：≤ 10MB
- 隐私说明：图片不会被存储

---

## 四、功能需求

### 4.1 图片上传
- 支持点击上传和拖拽上传
- 支持格式：JPG / PNG / WEBP
- 文件大小限制：10MB
- 超出限制时前端提示错误

### 4.2 去背景处理
- 前端将图片以 multipart/form-data 发送到 Worker
- Worker 转发至 Remove.bg API
- API Key 存储在环境变量中，不暴露给前端
- 处理超时：30 秒

### 4.3 结果展示
- 原图与结果图左右对比展示
- 结果图背景用棋盘格纹理表示透明区域
- 处理中显示 loading 动画

### 4.4 下载
- 点击下载按钮，直接下载透明背景 PNG
- 文件名：`removed-bg.png`

### 4.5 错误处理
- 文件格式不支持 → 前端提示
- 文件过大 → 前端提示
- API 调用失败 → 提示"处理失败，请重试"

---

## 五、非功能需求

- 不存储任何用户图片
- 无需用户注册或登录
- 页面加载速度：首屏 < 2s
- 移动端适配（响应式布局）

---

## 六、MVP 范围外（后续迭代）

- 批量处理
- 图片编辑（替换背景、调整边缘）
- 用户账号系统
- API 接口对外开放
- 付费套餐

---

## 七、部署方案

### Cloudflare Pages（前端 + Functions）
- 连接 GitHub 仓库，自动部署
- 构建命令：`npm run build`
- 输出目录：`dist`
- 环境变量：`REMOVE_BG_API_KEY`

---

## 八、开发任务拆解

- [x] 初始化项目（React + Vite）
- [x] 编写前端 UI（上传、预览、下载）
- [x] 编写 Cloudflare Pages Function（调用 Remove.bg）
- [x] 推送到 GitHub
- [ ] 部署到 Cloudflare Pages
- [ ] 配置 Remove.bg API Key
- [ ] 绑定自定义域名（可选）
