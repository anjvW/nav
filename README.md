# ANJV·未来导航中心

**欢迎通过修改 `public/navdata.json` 并提交 PR，向作者推荐你喜欢的网站！**

一个基于 Vue3 + Element Plus 的高颜值多级分类导航页，支持自定义网站、分类、科技风格UI、响应式布局，并可自动部署到 GitHub Pages。

## 功能特性
- 多级分类导航，支持任意自定义分类和网站
- 支持网站图标、描述、分类图标、批量后台打开
- 搜索功能，支持网站名和描述模糊搜索
- 卡片高度自适应，超出部分可滚动
- 响应式设计，适配PC和移动端
- 科技风格UI，深色渐变、发光、圆角、半透明
- 一键部署到 GitHub Pages

## 快速开始

### 1. 安装依赖
```bash
npm install
```

### 2. 本地开发
```bash
npm run dev
```

### 3. 构建生产包
```bash
npm run build
```

### 4. 部署到 GitHub Pages
推送到 main 分支后，GitHub Actions 会自动构建并发布到 gh-pages 分支。

## 数据结构与自定义

所有导航数据存放在 `public/navdata.json`，结构如下：
```json
{
  "categories": [
    {
      "name": "分类名称",
      "sites": [
        {
          "name": "网站名称",
          "url": "https://example.com",
          "icon": "https://example.com/favicon.ico",
          "description": "网站描述"
        }
      ]
    }
  ]
}
```

你可以直接编辑 `public/navdata.json`，添加/修改分类和网站。

## 目录结构
```
├── public/
│   └── navdata.json   # 导航数据
├── src/
│   ├── App.vue           # 主页面（全部逻辑在此）
│   └── main.js           # 入口
├── .github/workflows/deploy.yml # 自动部署
├── vite.config.js        # Vite 配置
├── package.json
└── README.md
```

## 常见问题
- **GitHub Pages 访问不到数据？**
  - axios 路径需为 `/nav/navdata.json`，确保 navdata.json 在 public/ 目录下。
- **Action 403 权限？**
  - workflow 需加 `permissions: contents: write`。
- **自定义UI/功能？**
  - 直接修改 `src/App.vue`，所有前端逻辑和样式都在一个文件中。

---

> 作者：anjv
> 
> 欢迎自定义、二次开发、star！
