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

### 2. 安装 CLAUDE.md 配置

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

### 3. 配置 settings.json（可选）

`settings.json` 包含 Claude Code 的环境变量、权限控制、插件配置等。

**使用前注意：**
- 需要添加你的 `ANTHROPIC_API_KEY`（DeepSeek API 密钥）
- 根据你的系统修改 `shellPath`（默认使用 Git Bash）
- 可以自定义 `statusLine` 状态栏显示

**配置步骤：**

1. **复制模板：**
   ```bash
   # Windows
   Copy-Item "settings.json" "$env:USERPROFILE\.claude\settings.json"
   
   # macOS/Linux
   cp settings.json ~/.claude/settings.json
   ```

2. **设置环境变量（DeepSeek API 密钥）：**
   ```bash
   # Windows (PowerShell)
   $env:ANTHROPIC_API_KEY = "your-deepseek-api-key"
   
   # macOS/Linux
   export ANTHROPIC_API_KEY="your-deepseek-api-key"
   ```

3. **配置 Shell 路径（Windows）：**
   ```json
   {
     "env": {
       "shellPath": "C:\\Program Files\\Git\\bin\\bash.exe"
     }
   }
   ```

4. **自定义权限（可选）：**
   根据你的需求修改 `permissions` 部分的 `allow` 和 `deny` 列表。

**DeepSeek API 配置说明：**

| 配置项 | 说明 | 示例值 |
|--------|------|--------|
|  | DeepSeek API 基础 URL |  |
|  | DeepSeek API 密钥 |  |
|  | 最强模型（对应 Claude Opus） |  |
|  | 平衡模型（对应 Claude Sonnet） |  |
|  | 快速模型（对应 Claude Haiku） |  |

**模型映射关系：**

| Claude 模型 | DeepSeek 模型 | 说明 |
|------------|--------------|------|
| Claude Opus |  | 最强性能，适合复杂任务 |
| Claude Sonnet |  | 平衡性能与速度 |
| Claude Haiku |  | 快速响应，适合简单任务 |

**配置说明：**

| 配置项 | 说明 | 是否必须 |
|--------|------|----------|
| `ANTHROPIC_API_KEY` | DeepSeek API 密钥 | ✅ 必须 |
| `ANTHROPIC_BASE_URL` | API 基础 URL | ✅ 已配置（DeepSeek） |
| `shellPath` | Shell 可执行文件路径 | ❌ 可选（Windows 建议配置） |
| `LANG/LC_ALL` | 系统编码设置 | ❌ 可选（推荐 UTF-8） |
| `PYTHONUTF8` | Python UTF-8 模式 | ❌ 可选（推荐启用） |
| `permissions` | 工具权限控制 | ❌ 可选（有默认配置） |
| `enabledPlugins` | 插件启用配置 | ❌ 可选 |

**模型配置：**

| 模型角色 | 配置项 | 默认值 |
|----------|--------|--------|
| Opus（最强模型） | `ANTHROPIC_DEFAULT_OPUS_MODEL` | `deepseek-v4-pro[1m]` |
| Sonnet（平衡模型） | `ANTHROPIC_DEFAULT_SONNET_MODEL` | `deepseek-v4-flash` |
| Haiku（快速模型） | `ANTHROPIC_DEFAULT_HAIKU_MODEL` | `deepseek-v4-flash` |

### 4. 验证安装

```bash
# 检查配置是否生效
cat ~/.claude/CLAUDE.md | head -20

# 检查 settings.json 格式
python -m json.tool ~/.claude/settings.json
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

## 反思与教训

### 为什么会混入 switch.py 代码？

**问题**：在更新 `CLAUDE.md` 时，意外混入了 `switch.py` 代码，导致文件损坏。

**原因分析**：
1. **Read 工具中断**：读取包含 UTF-8 中文的 `CLAUDE.md` 时，`Read` 工具中断失败
2. **改用 Bash 工具**：使用 `python -c "..."` 读取文件，但使用了双引号包裹多行 Python 代码
3. **Bash 解析错误**：双引号 `"` 会解析 `$`、`` ` ``、`"` 等特殊字符，导致 Python 代码执行异常
4. **输出混乱**：Bash 输出被截断或混淆，误以为输出是 `CLAUDE.md` 的内容
5. **写入错误**：将错误的输出（包含 `switch.py` 代码）写入了 `CLAUDE.md`

**根本原因**：
- **不是工具问题**：`Read` 工具中断是已知的 Windows 编码问题（有解决方案）
- **是操作问题**：使用了不安全的命令格式（双引号包裹多行）
- **缺少验证**：未验证输出内容是否正确，直接写入文件

**正确做法**：
```bash
# ✅ 正确：使用 Here Document，Bash 不解析内容
python << 'EOF'
with open('file.txt', 'r', encoding='utf-8') as f:
    content = f.read()
    print(content[:500])
EOF

# ✅ 正确：验证输出后再写入
# 1. 先读取并查看内容
# 2. 确认是目标文件的内容
# 3. 确认无误后再写入或修改
```

**预防措施**：
| 场景 | 推荐方案 | 避免方案 |
|------|----------|----------|
| 读取中文文件 | `python << 'EOF'` | `Read` 工具（易中断） |
| 多行 Python 代码 | `python << 'EOF'` | `python -c "..."` |
| 单行简单命令 | `python -c '...'` | `python -c "..."` |
| 文件内容处理 | 先写入临时文件再执行 | 直接在命令行写多行 |

**核心原则**：
1. **当 Read 工具中断时**：不要慌张，使用 `python << 'EOF'` 读取文件
2. **验证输出内容**：确认是目标文件的内容后再写入
3. **避免双引号包裹多行**：使用 Here Document 或单引号
4. **应急处理流程**：Read 中断 → 使用 Here Document → 验证内容 → 确认后写入

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
