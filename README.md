# Hexo 博客项目

这是一个基于 Hexo 框架的个人博客项目，用于记录和分享技术文章、学习笔记等内容。

## 环境要求

- Node.js (v10.0 或更高版本)
- npm 或 yarn

## 安装步骤

1. **克隆项目**
   ```bash
   git clone <repository-url>
   cd <project-directory>
   ```

2. **安装依赖**
   ```bash
   npm install
   ```

3. **本地预览**
   ```bash
   hexo server
   ```
   然后在浏览器中访问 `http://localhost:4000` 查看博客。

## 部署步骤

### 方法一：使用 Hexo 部署命令

1. **配置部署信息**
   在 `_config.yml` 文件中配置部署信息：
   ```yaml
   deploy:
     type: git
     repo: <your-github-repo-url>
     branch: gh-pages
   ```

2. **安装部署插件**
   ```bash
   npm install hexo-deployer-git --save
   ```

3. **生成静态文件并部署**
   ```bash
   hexo clean
   hexo generate
   hexo deploy
   ```

### 方法二：手动部署

1. **生成静态文件**
   ```bash
   hexo clean
   hexo generate
   ```

2. **将 `public` 目录下的文件上传到你的 GitHub Pages 仓库**

## 常用命令

- **新建文章**：`hexo new "文章标题"`
- **新建页面**：`hexo new page "页面名称"`
- **生成静态文件**：`hexo generate` 或 `hexo g`
- **启动本地服务器**：`hexo server` 或 `hexo s`
- **部署到远程仓库**：`hexo deploy` 或 `hexo d`
- **清理缓存**：`hexo clean`

## 目录结构

```
.
├── _config.yml        # Hexo 配置文件
├── _config.landscape.yml  # 主题配置文件
├── db.json            # 数据库文件（自动生成）
├── node_modules/      # 依赖包目录
├── package.json       # 项目配置文件
├── public/            # 生成的静态文件目录
├── scaffolds/         # 模板文件目录
├── source/            # 源文件目录
│   ├── _posts/        # 文章目录
│   ├── categories/    # 分类目录
│   ├── tags/          # 标签目录
│   └── picture/       # 图片资源目录
└── themes/            # 主题目录
    └── next/          # Next 主题
```

## 注意事项

- 文章使用 Markdown 格式编写，存放在 `source/_posts/` 目录下
- 图片资源建议放在 `source/picture/` 目录下，使用相对路径引用
- 部署前请确保 `_config.yml` 文件中的配置正确
- 如需修改主题配置，请编辑 `themes/next/_config.yml` 文件

## 主题说明

本项目使用 [Hexo Next](https://github.com/theme-next/hexo-theme-next) 主题，这是一个简洁、优雅的 Hexo 主题。

## 许可证

MIT
