# AI 工作台

跨平台桌面应用，管理本地 AI Skills 并浏览历史会话。支持 OpenCode 与 Claude Code。

## 功能

- **技能管理** — 新建/编辑/删除 Skills，YAML frontmatter 解析，Markdown 实时预览，中英翻译，ZIP 导入导出
- **来源切换** — 一键切换 OpenCode (`~/.config/opencode/skills`) 与 Claude Code (`~/.claude/skills`) 技能目录
- **会话浏览** — 查看 OpenCode 和 Claude Code 的历史会话，含完整对话记录、思考过程、工具调用
- **会话导出** — 将对话导出为独立 HTML 文件

## 技术栈

- Electron 28 + React 18
- better-sqlite3（OpenCode 会话数据）
- react-markdown + remark-gfm

## 快速开始

```bash
npm install
npm start
```

## 打包

```bash
npm run make
```

macOS 产物位于 `out/make/zip/darwin/arm64/`。

## 项目结构

```
src/
├── main/                     # Electron 主进程
│   ├── index.js              # 窗口管理、IPC 处理
│   ├── preload.js            # contextBridge API
│   ├── skillsManager.js      # 技能 CRUD、ZIP、翻译
│   ├── dbManager.js          # OpenCode SQLite 会话读取
│   └── claudeSessionManager.js  # Claude Code JSONL 会话读取
└── renderer/                 # React 渲染进程
    ├── App.jsx               # 主组件、状态编排
    ├── App.css               # 全局样式
    └── components/
        ├── Header.jsx        # 顶栏、模式切换
        ├── SkillListItem.jsx # 技能列表项
        ├── SkillDetail.jsx   # 技能详情/编辑/翻译
        ├── SessionList.jsx   # 会话列表
        ├── SessionDetail.jsx # 会话详情/导出
        ├── SettingsModal.jsx # 设置面板
        ├── EmptyState.jsx    # 空状态
        ├── Toast.jsx         # 通知提示
        └── ConfirmModal.jsx  # 确认弹窗
```

## 会话数据

| 来源 | 存储位置 | 格式 |
|------|---------|------|
| OpenCode | `~/.local/share/opencode/opencode.db` | SQLite |
| Claude Code | `~/.claude/projects/` + `~/.claude/history.jsonl` | JSONL |
