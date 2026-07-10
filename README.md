# Codex AGENTS Guidelines

一份面向 Windows 与 PowerShell 开发环境的 `AGENTS.md` 模板，用于约束 Codex 在项目中的命令执行方式和通用开发行为。

[English](README.en.md)

## 主要内容

模板包含两组规则：

- **PowerShell 执行规范**：统一使用 PowerShell Core 7 或更高版本，启用严格错误处理，检查原生程序退出码，安全处理路径与文本编码，并明确本地和远程 Shell 的边界。
- **通用开发行为规范**：遇到任何不确定时停止并询问，优先使用简单实现，只修改任务直接涉及的内容，并通过实际检查验证结果。

## 核心原则

1. **不确定就询问**：不得静默选择一种解释或自行扩大任务范围。
2. **简单优先**：不添加未要求的功能、配置或抽象。
3. **精准修改**：保持现有风格，保留用户改动，不顺带重构无关内容。
4. **证据优先**：先定义成功标准，再通过测试或运行检查验证。

## 使用方法

在新项目的根目录中下载模板：

```powershell
$ErrorActionPreference = 'Stop'
Set-StrictMode -Version Latest
Invoke-WebRequest `
    -Uri 'https://raw.githubusercontent.com/juanmao1z/codex-agents-guidelines/main/AGENTS.md' `
    -OutFile '.\AGENTS.md'
```

如果项目已有 `AGENTS.md`，请审阅本模板并手动合并需要的规则，不要直接覆盖项目现有约束。项目级规则应继续补充所用语言、框架、测试命令、目录边界和发布流程。

## 适用范围与取舍

这份模板刻意偏向谨慎性与可验证性。“存在任何不确定就停止询问”可以减少错误假设，但会增加交互次数；对于强调自主执行的工作流，可以根据团队风险偏好适当放宽。

PowerShell 规则针对本地 Windows 编排。进入 WSL、SSH、Docker 容器或其他远程 Linux 环境后，可以使用目标环境的 Shell，但应保持 Shell 边界清晰。

## 参考与致谢

本项目参考了以下公开资料：

- [AGENTS.md](https://agents.md/)：面向编程代理的开放项目指令格式。
- [multica-ai/andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills)：关于先思考、保持简单、精准修改和目标驱动执行的行为规范。
- [Andrej Karpathy 对 LLM 编程问题的观察](https://x.com/karpathy/status/2015883857489522876)：关于错误假设、过度设计和无关修改等常见问题的讨论。

本项目是在上述思想基础上的独立改编，并增加了 Windows/PowerShell 执行约束。它与 Andrej Karpathy、Multica AI 或 AGENTS.md 项目不存在官方隶属或背书关系。

## License

本项目采用 [MIT License](LICENSE)。
