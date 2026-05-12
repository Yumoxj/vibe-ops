# 清理 Agent 激活目录

控制器读取目标范围并确定深度，根据本目录决定激活哪些 agent。

## 始终激活

控制器在派发子代理前始终运行基础结构分析（未使用的 imports、console.log、注释掉的代码）。

## 条件 Agent

### 简化分析器

**Agent 文件：** `agents/simplification-analyzer.md`
**激活级别：** 所有深度（Surface、Standard、Deep）

始终激活。这是基准清理 agent。

### 废代码分析器

**Agent 文件：** `agents/dead-code-analyzer.md`
**激活级别：** Standard 或 Deep

满足以下条件时激活：
- 目标范围超过 5 个文件
- 项目使用的语言有成熟的废代码检测工具（TypeScript、JavaScript、Python、Go）
- 用户明确要求废代码检测

不激活：目标 < 5 个文件，或仅表面清理。

### 重复代码分析器

**Agent 文件：** `agents/duplicate-analyzer.md`
**激活级别：** 仅 Deep

满足以下条件时激活：
- 目标范围超过 15 个文件
- 代码库由多个贡献者长期维护
- 用户明确要求重复代码合并

不激活：Surface 或 Standard 深度，或单模块范围。

## 派发规则

- 多个 agent 激活时并行派发
- 每个 agent 接收相同的目标范围
- 控制器合并并去重跨 agent 的发现
