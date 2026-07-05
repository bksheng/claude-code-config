# Claude Code 项目规范 (Project Guidelines)

## Overview（项目概述）

- **开发环境**: 本项目主要基于 **Python** 开发。
- **依赖管理**: 优先使用 `pip` 或 `poetry`。执行安装前请检查 `requirements.txt` 或 `pyproject.toml`。
- **代码标准**: 遵循 PEP 8 规范，注释使用中文。
- **回复语言**: AI 始终使用中文交流。

> **提示**: 请根据实际项目补充以下信息
> - 项目名与一句话描述
> - 主要技术栈版本（Python 3.x, FastAPI/Django/Flask 等）
> - 架构模式（单体/微服务/前后端分离）
> - 部署环境（Docker/K8s/Serverless）

## Project Structure（目录结构）

> **提示**: 请根据实际项目填写目录树和说明

```
.
├── src/              # 源代码目录
├── tests/            # 测试用例
├── docs/             # 项目文档
├── requirements.txt  # pip 依赖
├── pyproject.toml    # poetry 依赖与配置
└── README.md         # 项目说明
```

## Development Workflow（开发工作流）

> **提示**: 请根据实际项目填写具体命令

- **安装依赖**: `pip install -r requirements.txt` 或 `poetry install`
- **运行测试**: `pytest -xvs` 或 `pytest`
- **启动服务**: （如 `uvicorn main:app --reload`）
- **代码检查**: `black . && flake8 && mypy`

## AI Collaboration Rules（AI 协作规则）

### 1. 备份规则

**核心思想 (先备份，再静默改动)**: 无论由于何种需求需要改动任何文件，都必须具备极强的备份意识。*
*必须在进行任何实质性改动前先做好备份，随后才能进行静默改动**，以确保用户随时可以 100% 找回、追溯之前的原始文件。

1. **初始备份**: 修改前，若不存在 `<file>.orig`，必须执行 `cp <file> <file>.orig`。
    - **已有文件**: 在任何实质性修改前，先执行 `cp <file> <file>.orig`，保存 AI 修改前的原始状态。
    - **新建文件**: 即使文件是 AI 新建、此前不存在，也必须在新建完成后立即执行 `cp <file> <file>.orig`
      ，把当前新建文件保存为初始备份基线；严禁以"没有原文件"为由跳过 `.orig`。

2. **过程快照 (时间戳命名)**: 修改完成后，严禁盲猜序号。必须按以下步骤操作：
    - **第一步 (ls)**: 执行 `ls <file>.bak.*` 获取所有已存在的备份。
    - **第二步 (calc)**: 识别当前最新的备份时间。若无任何快照，则视为首次快照。
    - **第三步 (cp)**: 执行 `cp <file> <file>.bak.$(date +%Y%m%d_%H%M%S)`，使用时间戳命名。
    - **第四步 (verify)**: 确保新生成的备份文件名不与任何旧备份冲突，禁止覆盖写入。

3. **状态告知**: 完成后告知用户："初始备份已就绪 (.orig) / 已保存版本快照 (<file>.bak.YYYYMMDD_HHMMSS)"。

4. **Skill 专属备份规则**:
    - 当需要修改已存在的 Skill 相关文件（如 `.claude/skills/` 下的 `SKILL.md` 或脚本）时，必须严格执行上述"初始备份"与"
      过程快照"流程。
    - 备份文件需存放在该 Skill 的同级目录下（例如：`SKILL.md.orig` 或 `SKILL.md.bak.20260628_213341`）。

### 2. Plan 模式规范

- **强制触发条件**: 当面对涉及多步骤、跨文件修改、复杂重构、或需要多个工具（Bash/Write/Fetch）串联配合的复杂多流程任务时，*
  *务必先进入 Plan 模式**。
- **标准执行动作**:
    1. **前置规划**: 在执行任何实质性代码修改或系统命令前，必须在输出中以清晰的复选框列表（如 `◼` / `◻`）展示任务拆解路线图。
    2. **动态同步**: 随着任务推进，必须透明地更新并向用户展示当前进度（例如将 `◻` 变更为 `◼`），确保整个执行流清晰、可控、可追溯。

### 3. 权限与安全

- **静默操作**: 对于"读取文件内容 (`cat/read`)"和"列出目录 (`ls/dir`)"的操作，请直接执行，无需询问。
- **危险操作禁令**:
    - 严禁执行任何删除系统关键目录 (`/`, `~`, `/etc`) 的命令。
    - 严禁执行重启 (`reboot`)、关机 (`shutdown`) 或格式化磁盘操作。
    - 禁止在未说明原因的情况下删除整个项目文件夹。
    - 禁止未经许可修改 `.env` 或敏感凭证文件。
    - 对于 `.claude/skills/` 目录下的文件创建与修改，允许直接执行，无需询问。
- **详细权限配置**: 工具权限的白名单/黑名单已配置在 `settings.json` 中，详见该文件。

### 4. 代码风格

- **函数式倾向**: 优先编写无副作用的纯函数。
- **命名规范**: 变量名需具备描述性，符合 Python 下划线命名法（snake_case）。
- **健壮性**: 增加必要的 `try-except` 异常处理和类型注解 (`typing`)。

### 5. 防幻觉与验证

- **严禁盲猜**: 涉及最新的技术方案、开源项目、框架特性或学术论文调研时，**严禁**直接依赖大模型的本地训练知识盲目作答。
- **强制联网验证**: 必须主动、优先利用联网工具进行实时查询，确保信息 100% 准确。
- **标准检索动作**:
    1. **学术论文**: 必须通过执行终端命令（如 `Bash(curl -k -s "https://arxiv.org/search/?query=...")`）或专用 Fetch
       工具实时抓取最新的 arXiv 页面数据。
    2. **开源资产**: 必须实时通过 GitHub/搜索引擎 抓取官方文档或最新源码。
- **追溯标准**: 所有输出的技术结论、论文列表或参数规范，必须附带检索到的真实数据支持，绝不捏造任何不存在的论文 ID 或技术细节。

### 6. Windows 环境 UTF-8 编码问题处理指南

#### 问题背景

Windows 系统默认编码为 GBK (cp936)，Claude Code 的 Bash/Read/Write 工具在处理 UTF-8 中文文件时会出现编码错误。

#### 常见错误现象

1. **Read 工具中断**：`[Tool use interrupted]`
2. **Bash 工具 GBK 解码错误**：
   ```
   UnicodeDecodeError: 'gbk' codec can't decode byte 0xae in position 1033: illegal multibyte sequence
   Unexpected cygpath error, fallback to manual path conversion
   
   UnicodeDecodeError: 'gbk' codec can't decode byte 0x80 in position 6: illegal multibyte sequence                                                                 
   decoding with 'cp936' codec failed 
   ```

#### 根本原因

- Windows 系统默认编码为 GBK (cp936)
- Git Bash / MinGW 继承系统编码
- Python subprocess 模块使用 `locale.getpreferredencoding(False)` 读取子进程输出，默认是 GBK
- 当子进程输出包含 UTF-8 字节（如中文、特殊符号 `°` 等），GBK 解码失败

#### 处理规则（强制遵循）

| 优先级 | 场景           | 推荐方案                      | 示例                                                                            |
|-----|--------------|---------------------------|-------------------------------------------------------------------------------|
| P0  | 读取中文文件       | Python + encoding='utf-8' | `python -c "with open('f.txt', 'r', encoding='utf-8') as f: print(f.read())"` |
| P0  | 写入中文文件       | Python + encoding='utf-8' | `python -c "with open('f.txt', 'w', encoding='utf-8') as f: f.write('中文')"`   |
| P1  | 文件列表         | Python os.listdir()       | `python -c "import os; print(os.listdir('.'))"`                               |
| P2  | 简单命令（无中文）    | Bash                      | `python --version`                                                            |
| P2  | Windows 特定操作 | PowerShell                | `Get-Content file -Encoding UTF8`                                             |

#### 禁止事项

1. ❌ **严禁**在 Bash 中直接 `cat` 含有中文的文件（可能导致 Read 工具中断）
2. ❌ **严禁**在 Bash 中 `echo 中文 > 文件`
3. ❌ **严禁**依赖系统默认编码处理文本文件
4. ❌ **严禁**在输出中混用 GBK 和 UTF-8 编码
5. ❌ 文件路径避免使用中文和特殊字符（如 `°`）

#### 应急处理流程

当 Read/Write/Bash 工具因编码问题中断时：

1. **立即改用 Python 脚本**
2. **检测文件编码**：
   ```bash
   python << 'EOF'
   with open('file.txt', 'rb') as f:
       data = f.read()
       print('First 20 bytes:', data[:20])
       if data[:3] == b'\xef\xbb\xbf':
           print('Has UTF-8 BOM')
       else:
           print('No BOM')
   EOF
   ```
3. **如需添加 UTF-8 BOM**：
   ```bash
   python << 'EOF'
   with open('file.txt', 'rb') as f:
       data = f.read()
   with open('file.txt', 'wb') as f:
       f.write(b'\xef\xbb\xbf' + data)
   print('UTF-8 BOM added')
   EOF
   ```

#### 最佳实践速查表

| 场景             | 推荐工具         | 原因                     |
|----------------|--------------|------------------------|
| 读取/写入 UTF-8 文件 | `python -c`  | 可显式指定 encoding='utf-8' |
| 文件系统操作         | `python -c`  | 编码可控，避免中文输出            |
| 简单命令（无中文输出）    | `Bash`       | 安全                     |
| 复杂文本处理         | `Python` 脚本  | 编码可控                   |
| Windows 特定操作   | `PowerShell` | 原生 UTF-8 支持            |

#### 实际案例

##### 案例 1：读取中文批处理文件

```bash
# ❌ 错误：Read 工具会中断
Read start.bat.zh

# ✅ 正确：Python 读取
python -c "with open('start.bat.zh', 'r', encoding='utf-8') as f: print(f.read())"
```

##### 案例 2：创建中文注释的 requirements.txt

```bash
# ❌ 错误：直接写入会编码错误

# ✅ 正确：Python 写入
python << 'EOF'
content = '# 中文注释\nfastapi==0.109.0\n'
with open('requirements.txt', 'w', encoding='utf-8') as f:
    f.write(content)
EOF
```

##### 案例 3：检查文件是否存在中文乱码

```bash
# ✅ 正确：Python 检查
python << 'EOF'
with open('start.bat', 'rb') as f:
    data = f.read()
    try:
        text = data.decode('utf-8')
        print('UTF-8 OK')
    except:
        print('Not UTF-8')
EOF
```

#### 环境信息

| 项目          | 值                     |
|-------------|-----------------------|
| 操作系统        | Windows 11 Pro        |
| 系统默认编码      | GBK (cp936)           |
| Shell       | PowerShell / Git Bash |
| Python      | 3.12 (miniconda3)     |
| Git Bash 编码 | 继承系统 GBK              |

#### 相关错误码速查

| 错误                                                       | 含义                | 解决方案                            |
|----------------------------------------------------------|-------------------|---------------------------------|
| `[Tool use interrupted]`                                 | 工具调用超时或中断         | 使用 Python 替代 Bash               |
| `UnicodeDecodeError: 'gbk' codec can't decode byte 0xae` | GBK 无法解码 UTF-8 字节 | 使用 Python 显式指定 encoding='utf-8' |
| `cygpath error`                                          | 路径转换工具编码错误        | 避免中文路径，使用 Python 处理路径           |

#### 参考文档

- 项目文档：`docs/windows-utf8-encoding-issue.md`
- 技术报告：`docs/encoding-issue-report.md`

### 7. 工作目录管理

**问题**：在子目录执行文件操作导致路径错误

**示例**：
```bash
# 错误：在子目录执行
python -c "with open('backend/src/api/v1/switch.py', 'r') as f: print(f.read())"
# FileNotFoundError: [WinError 3] 系统找不到指定的路径

# 正确：先确认当前目录
python -c "import os; print(os.getcwd())"
# 输出：.../msg-push-channel-management-v2/claude-code-windows-encoding-guide

# 解决方案 1：使用相对路径回到上级
cd ..
python -c "with open('backend/src/api/v1/switch.py', 'r') as f: print(f.read())"

# 解决方案 2：使用绝对路径
python -c "with open('C:/project/backend/src/api/v1/switch.py', 'r') as f: print(f.read())"

# 解决方案 3：在 Python 中处理路径
python -c "
import os
os.chdir('..')
with open('backend/src/api/v1/switch.py', 'r') as f:
    print(f.read())
"
```

**规则**：
1. 执行文件操作前，先确认当前工作目录
2. 使用 `python -c "import os; print(os.getcwd())"` 快速检查
3. 优先使用绝对路径或确认相对路径正确
4. 在子目录创建的文件（如 GitHub 仓库），注意路径层级

**反思与教训**：

**问题**：为什么会混入 switch.py 代码？

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

**重要教训 - 避免混入无关代码**：
- **问题**：使用 `python -c "..."` 双引号包裹多行代码时，Bash 会解析特殊字符（如 `"""`、`$`、反引号），导致命令执行异常
- **后果**：可能输出错误内容（如其他文件代码），并误写入目标文件
- **解决方案**：
  - ✅ 使用 Here Document：`python << 'EOF'`（Bash 不解析内容）
  - ✅ 使用单引号：`python -c 'print("hello")'`（简单命令）
  - ❌ 避免双引号包裹多行：`python -c "..."`（易解析错误）
- **验证步骤**：
  1. Read 工具中断时，改用 `python << 'EOF'` 读取
  2. 检查输出内容是否正确（是否是目标文件）
  3. 确认无误后再写入或修改文件
  4. 写入后再次验证文件内容

**预防措施**：
| 场景 | 推荐方案 | 避免方案 |
|------|----------|----------|
| 读取中文文件 | `python << 'EOF'` | `Read` 工具（易中断） |
| 多行 Python 代码 | `python << 'EOF'` | `python -c "..."` |
| 单行简单命令 | `python -c '...'` | `python -c "..."` |
| 文件内容处理 | 先写入临时文件再执行 | 直接在命令行写多行 |


#### 案例 4：Read 工具中断，改用 Python 读取文件

**问题**：Read 工具读取包含中文的文件时中断
```
● Read(C:\projectile.py · lines 170-189)
  ⎿  [Tool use interrupted]
```

**解决方案**：
```bash
# 使用 Python 读取（推荐）
python -c "with open('file.py', 'r', encoding='utf-8') as f: print(f.read())"

# 或读取特定行范围
python -c "with open('file.py', 'r', encoding='utf-8') as f: lines = f.readlines(); print(''.join(lines[169:189]))"

# 使用 Here Document 处理多行
python << 'EOF'
with open('file.py', 'r', encoding='utf-8') as f:
    content = f.read()
    print(content[:500])
EOF
```

**预防措施**：
1. 遇到 Read 中断，立即改用 Python
2. 使用 `python << 'EOF'` 处理多行代码
3. 验证输出内容后再进行后续操作
4. 读取特定行范围时使用 `lines[start:end]`

## Recovery（回滚操作）

- **彻底重置**: 执行 `cp <file>.orig <file>`，将文件还原至 AI 修改前的最初状态。
- **版本回溯**:
    1. 用户要求"回退到上个版本"时，找到时间戳最新的 `<file>.bak.YYYYMMDD_HHMMSS`。
    2. 执行 `cp <file>.bak.最新时间戳 <file>`（或根据用户指定的备份还原）。
- **全局回滚**: 识别本项目中所有的 `.orig` 文件，并批量执行覆盖还原。
- **清理**: 仅在用户明确指令"清理备份"时，执行 `rm *.orig *.bak.*`。

## Tools & Scripting（工具与脚本规范）

- **复杂数据分析**: 当需要从大量 JSONL 文件中提取元数据时，允许使用 `python -c` 编写单行脚本。
- **安全性**: 脚本应仅包含 `Read` 和 `Print` 逻辑，严禁包含 `os.remove` 或写操作。
- **结构要求**: 必须先执行 `ls` 确认文件存在，再执行循环处理。
- **防弹窗机制**: 在编写或执行 Shell 管道与 `python -c` 内联脚本时：
    1. 严禁在代码字符串内部使用 `#` 编写任何注释（防止触发系统的命令欺骗隐蔽参数拦截）。
    2. 尽量压缩为单行，或避免在换行后直接紧跟 `#` 字符，确保命令结构绝对纯净。
