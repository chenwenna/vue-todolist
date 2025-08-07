# Vue TodoList 📝

一个功能完整的Vue3待办事项应用，具有五彩缤纷的渐变界面和拖拽功能。

## ✨ 功能特性

- 🎨 **五彩缤纷的渐变界面** - 11种颜色的动态背景渐变
- ➕ **添加待办事项** - 支持回车键和点击按钮
- ✅ **标记完成状态** - 点击复选框标记任务完成
- 🗑️ **删除待办事项** - 点击垃圾桶图标删除任务
- 🔄 **拖拽排序** - 使用拖拽手柄重新排序任务
- 💾 **数据持久化** - 使用localStorage自动保存数据
- 📊 **统计信息** - 显示总计、已完成、待完成任务数量
- 📱 **响应式设计** - 完美适配移动端和桌面端

## 🛠️ 技术栈

- **Vue 3** - 渐进式JavaScript框架
- **Vite** - 快速的前端构建工具
- **SortableJS** - 拖拽排序库
- **CSS3** - 现代CSS特性（渐变、动画、毛玻璃效果）

## 🚀 本地开发

### 安装依赖
```bash
npm install
```

### 启动开发服务器
```bash
npm run dev
```

### 构建生产版本
```bash
npm run build
```

### 预览生产版本
```bash
npm run preview
```

## 🌐 部署到GitHub Pages

### 方法一：自动部署（推荐）

1. **创建GitHub仓库**
   - 在GitHub上创建新仓库，命名为 `vue-todolist`
   - 将本地代码推送到仓库

2. **推送代码**
   ```bash
   git init
   git add .
   git commit -m "Initial commit"
   git branch -M main
   git remote add origin https://github.com/你的用户名/vue-todolist.git
   git push -u origin main
   ```

3. **启用GitHub Pages**
   - 进入仓库的 Settings → Pages
   - Source 选择 "Deploy from a branch"
   - Branch 选择 "gh-pages"
   - 点击 Save

4. **自动部署**
   - GitHub Actions会自动构建并部署到 `gh-pages` 分支
   - 几分钟后可通过 `https://你的用户名.github.io/vue-todolist/` 访问

### 方法二：手动部署

1. **构建项目**
   ```bash
   npm run build
   ```

2. **部署到gh-pages分支**
   ```bash
   npm install -g gh-pages
   gh-pages -d dist
   ```

## 📁 项目结构

```
vue-todolist/
├── .github/
│   └── workflows/
│       └── deploy.yml          # GitHub Actions部署配置
├── src/
│   ├── App.vue                 # 主组件
│   └── main.js                 # 应用入口
├── index.html                  # HTML模板
├── package.json                # 项目配置
├── vite.config.js              # Vite配置
└── README.md                   # 项目说明
```

## 🎨 界面预览

- 🌈 动态渐变背景，11种颜色循环变化
- 🔮 毛玻璃效果的半透明容器
- ✨ 彩虹色标题和按钮
- 💫 光波扫过效果的待办事项
- 📈 彩色渐变的统计信息

## 📄 许可证

MIT License

## 🤝 贡献

欢迎提交Issue和Pull Request！

---

**享受使用这个五彩缤纷的TodoList应用！** 🎉