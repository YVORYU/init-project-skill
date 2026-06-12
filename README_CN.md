# 项目初始化技能（init-project）

一个用于初始化「氛围编程」（Vibcoding，即 AI 驱动开发）项目环境的技能。它不会编写任何源代码，也不会安装任何依赖，而是为 AI 助手在后续开发中所依赖的「文档与规范层」搭建一套完整骨架。

## 一、技能简介

本技能通过与用户的简短问答，收集项目的基本信息（项目名、编程语言、技术栈、项目类型等），随后自动生成一套标准化的项目文档与 AI 记忆系统，包含：

- **CLAUDE.md** —— AI 行为规则文件（每次会话自动加载，是最关键的文件）
- **README.md** —— 项目说明入口
- **ROADMAP.md** —— 开发路线图
- **.gitignore** —— 通用版本控制忽略规则
- **memory/** 目录 —— AI 记忆系统（包含索引、用户偏好、架构决策三份文档）

## 二、触发时机

当用户表达以下任一意图时，AI 助手会自动调用本技能：

- 「初始化项目」「init project」
- 「搭建项目骨架」「scaffold」「set up project」
- 「第一阶段」「Phase 1」
- 从零开始一个全新项目

## 三、目录结构

```
init-project/
├── SKILL.md                    # 技能主指令文件（供 AI 读取）
├── README.md                   # 说明文档（英文，默认入口）
├── README_CN.md                # 本说明文档（中文）
├── scripts/
│   └── init-memory.sh          # 创建 AI 记忆目录与文件的脚本
├── references/
│   ├── claude-template.md      # CLAUDE.md 模板（含占位符）
│   ├── readme-template.md      # README.md 模板（含占位符）
│   ├── roadmap-template.md     # ROADMAP.md 模板（含占位符）
│   └── vibecoding-template.md  # VIBECODING.md 模板
└── assets/
    └── gitignore_template      # 通用 .gitignore 模板
```

## 四、工作流程

第一步：**用户访谈**
通过提问工具收集项目名、一句话描述、编程语言、技术栈、项目类型等必填信息；可选项包括版本控制策略、开发模式、是否使用表情符号等。

第二步：**生成核心文档**
读取 `references/` 目录下的模板文件，将其中的 `{{占位符}}` 替换为用户的回答，然后写入项目根目录。

第三步：**创建记忆系统**
执行 `bash scripts/init-memory.sh` 脚本，初始化 `memory/` 目录及内部文件。

第四步：**可选的版本控制初始化**
若用户启用了版本控制，则执行 `git init` 并完成首次提交；若提供了远程仓库地址，则一并完成关联与推送。

## 五、占位符映射

| 占位符 | 取值来源 |
|---|---|
| `{{PROJECT_NAME}}` | 用户提供的项目名称 |
| `{{PROJECT_DESCRIPTION}}` | 一句话项目描述 |
| `{{LANGUAGE}}` | 编程语言 |
| `{{TECH_STACK}}` | 完整技术栈 |
| `{{PROJECT_TYPE}}` | 命令行工具 / 网页应用 / 库 / 移动应用 等 |
| `{{CODE_STANDARDS}}` | 根据语言匹配的代码规范 |
| `{{TEST_STANDARDS}}` | 根据语言匹配的测试规范 |
| `{{PROJECT_ROOT}}` | 项目目录名 |

### 编程语言与规范对照表（节选）

| 语言 | 代码规范 | 测试规范 |
|---|---|---|
| Python | 谷歌风格文档字符串 + PEP 484 类型注解 | pytest，覆盖率高于 80% |
| TypeScript / JavaScript | ESLint + Prettier，严格模式 | Vitest 或 Jest，覆盖率高于 80% |
| Java | Javadoc + Spring Boot 约定 | JUnit 5，覆盖率高于 80% |
| Go | gofmt + 标准项目布局 | go test，覆盖率高于 80% |
| Rust | rustfmt + clippy 静态检查 | cargo test |
| 其他语言 | 遵循该语言社区通用规范 | 该语言的标准测试框架 |

完整对照表请参见 [SKILL.md](file:///c:/Users/YV/.claude/skills/init-project/SKILL.md)。

## 六、所依赖的工具

本技能需要 AI 助手具备以下工具能力：

- **写入文件** —— 创建文档
- **编辑文件** —— 调整内容
- **读取文件** —— 加载模板
- **提问工具** —— 进行用户访谈
- **命令行** —— 执行脚本与版本控制命令

## 七、本技能不做的事

为保持职责单一，本技能明确不包含以下工作：

- 创建任何源代码文件
- 安装依赖或工具链
- 配置构建系统
- 创建框架特定文件
- 配置开发环境或编辑器

以上工作交由后续开发阶段（第二阶段及以后）完成。本技能只负责搭建 AI 助手有效工作所必需的「规范与文档层」。

## 八、重要约定

1. 除非用户明确要求，否则**不使用表情符号**
2. 模板内容存放于 `references/` 目录，不在主指令文件中硬编码
3. 用户偏好统一记录到 `memory/user-preferences.md`
4. 关键架构决策（含日期）记录到 `memory/decisions.md`
5. `CLAUDE.md` 是核心文件 —— 它决定了后续所有 AI 行为
6. **Vibcoding 配置存放位置**：所有与 vibcoding 相关的配置（包括但不限于 Claude 命令白名单、hooks、skills、subagent 定义等）**必须**放在项目根目录的 `.claude/` 文件夹下。全局配置（如 `%USERPROFILE%\.claude\` 或 `~/.claude/`）仅在用户明确要求时才使用。

---

英文版请参见 [README.md](file:///c:/Users/YV/.claude/skills/init-project/README.md)。
