# Claude Code 配置中心

> **项目定位**：管理和迭代 Claude Code 配置文件，积累使用经验和最佳实践
>
> **适用场景**：Windows / macOS / Linux 环境下的 Claude Code 使用
>
> **更新方式**：遇到新问题 → 优化配置 → 提交更新 → 同步到本地

---

## 📋 目录

- [快速开始](#快速开始)
- [配置文件说明](#配置文件说明)
- [使用经验积累](#使用经验积累)
- [更新流程](#更新流程)
- [贡献指南](#贡献指南)

---

## 快速开始

### 1. 克隆仓库

```bash
git clone https://github.com/bksheng/claude-code-config.git
cd claude-code-config
```

### 2. 安装配置

**Windows:**
```powershell
# 备份现有配置
Copy-Item "$env:USERPROFILE\.claude\CLAUDE.md" "$env:USERPROFILE\.claude\CLAUDE.md.bak"

# 复制新配置
Copy-Item "CLAUDE.md" "$env:USERPROFILE\.claude\CLAUDE.md"
```

**macOS/Linux:**
```bash
# 备份现有配置
cp ~/.claude/CLAUDE.md ~/.claude/CLAUDE.md.bak

# 复制新配置
cp CLAUDE.md ~/.claude/CLAUDE.md
```

### 3. 验证安装

```bash
# 检查配置是否生效
cat ~/.claude/CLAUDE.md | head -20
```

---

## 配置文件说明

### CLAUDE.md

主配置文件，包含以下模块：

| 模块 | 说明 | 适用场景 |
|------|------|----------|
| **Overview** | 项目概述和开发环境 | 新项目初始化 |
| **Project Structure** | 目录结构规范 | 项目创建 |
| **Development Workflow** | 开发工作流 | 日常开发 |
| **AI Collaboration Rules** | AI 协作核心规则 | **所有项目** |
| **Recovery** | 回滚操作 | 问题恢复 |
| **Tools & Scripting** | 工具使用规范 | 脚本编写 |

### 核心规则（AI Collaboration Rules）

1. **备份规则** - 先备份再改动
2. **Plan 模式规范** - 复杂任务先规划
3. **权限与安全** - 安全操作规范
4. **代码风格** - 编码规范
5. **防幻觉与验证** - 验证机制
6. **Windows UTF-8 编码处理** - 中文环境编码问题 ✅
7. **工作目录管理** - 目录切换和路径处理 ✅

---

## 使用经验积累

### 经验记录模板

遇到新问题后，按以下格式记录：

```markdown
## 经验记录：[简短描述]

### 问题现象
[描述遇到的问题]

### 环境信息
- OS: [操作系统]
- Claude Code 版本: [版本号]
- 相关工具: [工具名称]

### 解决方案
[具体的解决步骤]

### 配置更新
[对 CLAUDE.md 的修改内容]

### 参考链接
[相关 Issue、文档链接]
```

### 现有经验列表

| 编号 | 经验主题 | 相关配置 | 状态 |
|------|----------|----------|------|
| 001 | Windows UTF-8 编码问题 | `AI Collaboration Rules` → `6. Windows UTF-8 编码处理` | ✅ 已收录 |
| 002 | 工作目录管理 | `AI Collaboration Rules` → `7. 工作目录管理` | ✅ 已收录 |
| 003 | [待添加] | | |

---

## 更新流程

### 个人使用更新

```bash
# 1. 遇到问题，优化本地配置
# 编辑 ~/.claude/CLAUDE.md

# 2. 测试验证
# 使用 Claude Code 验证新配置是否解决问题

# 3. 复制到仓库
cp ~/.claude/CLAUDE.md /path/to/claude-code-config/

# 4. 提交更新
cd /path/to/claude-code-config/
git add CLAUDE.md
git commit -m "feat: 添加 [问题描述] 的处理规则"

# 5. 推送到 GitHub
git push origin main
```

### 同步最新配置

```bash
# 1. 拉取最新配置
cd /path/to/claude-code-config/
git pull origin main

# 2. 复制到本地
cp CLAUDE.md ~/.claude/CLAUDE.md

# 3. 验证
cat ~/.claude/CLAUDE.md | head -20
```

---

## 贡献指南

### 提交新经验

1. **Fork 仓库** 或创建分支
2. **更新 CLAUDE.md** 添加新的处理规则
3. **更新 README.md** 在经验列表中添加记录
4. **提交 PR** 描述问题和解决方案

### 提交规范

```
feat: 添加 [功能/规则描述]
fix: 修复 [问题描述]
docs: 更新 [文档描述]
refactor: 重构 [模块描述]
```

---

## 文件结构

```
claude-code-config/
├── CLAUDE.md              # 主配置文件
├── README.md              # 本文件
├── LICENSE                # MIT 许可证
├── CHANGELOG.md           # 更新日志
├── docs/                  # 详细文档
│   ├── encoding-guide.md  # 编码问题详细指南
│   ├── workflow-guide.md  # 工作流指南
│   └── troubleshooting.md # 故障排查
└── examples/              # 示例配置
    ├── python-project.md  # Python 项目配置
    ├── web-project.md     # Web 项目配置
    └── minimal.md         # 最小配置
```

---

## 相关资源

- [Claude Code 官方文档](https://code.claude.com/docs)
- [Claude Code Windows 编码问题解决方案](https://github.com/bksheng/claude-code-windows-encoding-guide)
- [Anthropic GitHub Issues](https://github.com/anthropics/claude-code/issues)

---

## 许可证

MIT License

---

*最后更新：2026-07-05*
*维护者：bksheng*
