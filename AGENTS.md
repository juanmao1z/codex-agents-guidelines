# PowerShell 执行规范

本规范适用于 Codex 在本地 Windows 上执行的所有命令，以及 Codex 创建或修改的 PowerShell 脚本。

- 必须使用 `pwsh`（PowerShell Core 7 或更高版本）作为本地 Windows Shell 主机。不得主动调用 `powershell.exe` 或 `cmd.exe`，不得在 PowerShell 命令层混入 Bash 语法。
- 可以从 pwsh 调用 `git`、`rg`、`python`、`node`、`docker` 等原生 CLI；“统一使用 PowerShell”是指统一 Shell 主机，而不是禁止原生工具。
- Codex 生成或执行的每个 PowerShell 脚本和命令块，第一条可执行语句必须是 `$ErrorActionPreference = 'Stop'`，随后执行 `Set-StrictMode -Version Latest`。
- `$ErrorActionPreference` 不代替原生程序退出码检查。每次调用原生 CLI 后必须立即检查 `$LASTEXITCODE`；只允许显式说明并处理的预期非零状态码。
- 调用原生程序时使用调用运算符 `&`、参数数组或 splatting。禁止使用 `Invoke-Expression` 拼接并执行命令。
- 新建文本文件统一使用 UTF-8；默认使用无 BOM 的 UTF-8，目标格式明确要求 BOM 时除外。PowerShell 文本 cmdlet 支持 `-Encoding` 时必须显式指定 `-Encoding UTF8`。
- 修改现有文本文件前必须确认并保留原编码，除非任务明确要求转换为 UTF-8。不得对二进制文件应用文本编码。
- 文件路径优先使用 `-LiteralPath`、`Join-Path` 和 `Resolve-Path`，避免路径被当作通配符处理。
- 数据解析优先使用 PowerShell 对象管道和结构化格式，例如 `ConvertFrom-Json`、`Import-Csv`、`ConvertFrom-Csv` 和 `[xml]`。只有在没有结构化输出时才解析供人阅读的文本。
- 对 WSL、SSH、Docker 容器或远程 Linux，允许使用目标环境的 Shell，但必须明确 Shell 边界；本地编排仍使用 pwsh，不能在同一命令层混用 PowerShell 与 Bash 转义规则。
- 宣称执行成功前，必须核对 PowerShell 错误、原生程序退出码和必要的运行结果，并明确报告任何规范例外。

# 通用开发行为规范

- 对需求、范围、风险、实现方式或预期结果存在任何不确定时，必须立即停止并询问用户；在获得明确答复前，不得自行假设、选择解释或继续实施。
- 优先采用满足需求的最简单实现。不增加未要求的功能、配置项或一次性抽象。
- 修改必须严格对应用户请求。保持现有代码风格，不顺带重构、格式化或清理无关代码，并保留用户已有改动。
- 开始前明确可验证的成功标准。完工前运行与风险相匹配的测试或运行检查，不得在缺少证据时宣称成功。

# 代码注释规范

- 新增或修改代码时，默认为模块、类、函数、方法、公开常量和其他对外接口添加 Doxygen 风格文档注释；注释应与代码一同维护。
- 使用语言原生的注释语法包裹 Doxygen 内容，例如 `/** ... */` 或 `///`；必须保持代码可被目标语言编译器或解释器正常解析。
- 接口注释应优先说明用途、行为契约、前置条件和边界条件，并按需使用 `@brief`、`@param`、`@tparam`、`@return`、`@throws`/`@exception`、`@see` 等标准标签。
- `@param`、`@tparam`、`@return` 和 `@throws` 等标签必须与实际参数、返回值和异常行为一致；实现行为变化时同步更新注释。
- 普通实现注释可以使用简短的 `//` 或语言等价形式，但仅用于解释非显而易见的原因、算法或约束；不要为每一行机械添加注释，也不要重复代码本身已经表达的内容。
- 对不直接支持 C/C++ 注释语法的语言，使用该语言合法的文档注释形式，并保留相同的 Doxygen 标签语义。

# Git 使用规范

- 操作前检查当前分支和工作区状态，并保留用户已有改动。
- 不得执行可能丢失内容的命令，如 `git reset --hard`、`git clean`、`git checkout --` 和强制推送。
- 未经明确要求，不得切换分支、暂存、提交、推送、变基或修改历史。
- 提交时只暂存本次任务相关文件，并在提交前检查暂存区差异。
- 遇到冲突、敏感信息或不确定的改动范围时，停止操作并询问用户。

# Subagent 使用规范

- 仅当任务包含多个相互独立、可并行且可单独验收的子任务时使用 subagent；简单任务、强顺序任务或需要同时修改同一文件的任务不使用。
- 主 agent 负责划分范围、整合结果和最终验证；未经用户明确授权，subagent 不得执行提交、推送、删除等具有外部影响或不可逆的操作。
