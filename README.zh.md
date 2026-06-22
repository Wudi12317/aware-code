# AwareCode

**AI 代理编码感知协议。** 写代码前扫描项目上下文和现有模式，遇到错误时学习根因，修改后审查质量、同步文档、输出总结。

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Agent Skills](https://img.shields.io/badge/Agent%20Skills-Compatible-6A4EFF)](https://agentskills.io)

AwareCode 是一个元技能（meta-skill）：它规范的是**代码怎么写**，而不是**写什么代码**。它与领域技能（React、Godot、SQL 等）协同工作，确保每次代码变更都感知上下文、从错误中学习、并保持文档健康。

## 功能

- **上下文扫描** — 写代码前读取 AGENTS.md、package.json、现有文件和项目约定
- **错误记忆** — 分析根因、记录到 session 日志、避免重复犯错
- **质量审查** — 每次变更后检查正确性、性能、安全和测试覆盖
- **文档同步** — 代码变更时同步更新注释、README 和 CHANGELOG
- **操作总结** — 每次变更后输出结构化总结（做了什么、为什么、权衡取舍）

## 安装

### 通过 npx skills（最简）

```bash
npx skills add Wudi12317/aware-code
```

### 全局安装（手动）

```bash
git clone https://github.com/Wudi12317/aware-code.git
cp -r aware-code ~/.config/opencode/skills/aware-code
```

### 项目级安装

```bash
cp -r aware-code .opencode/skills/aware-code
```

### Windows

```powershell
git clone https://github.com/Wudi12317/aware-code.git
Copy-Item -Recursive aware-code "$env:USERPROFILE\.config\opencode\skills\aware-code"
```

### 验证

启动 opencode 后询问：

```
有哪些 skill 可用？
```

列表中应出现 `aware-code`。

## 使用方法

AwareCode 会在编码任务中自动激活。你也可以手动调用：

```
使用 aware-code skill 审查我上次的变更
```

每次变更后技能会输出清晰的结构化总结。如果你偏好其他语言或格式，可以 fork 后修改阶段 5（总结阶段）。

## 目录结构

```
aware-code/
├── SKILL.md                  # 主协议（五阶段）
├── LICENSE                   # MIT
├── README.md                 # 英文说明
├── README.zh.md              # 中文说明（本文件）
├── CHANGELOG.md              # 发布历史
├── references/
│   ├── patterns.md           # 编码模式和约定检查清单
│   ├── errors.md             # 错误记忆模板和根因目录
│   ├── performance.md        # 性能优化（按领域分类）
│   ├── security.md           # 安全 checklist（按漏洞分类）
│   └── workflow.md           # 与 OpenCode 配合的高级工作流
└── examples/
    └── basic.md              # 使用场景示例
```

## 兼容性

AwareCode 遵循 [Agent Skills](https://agentskills.io) 标准，兼容所有支持 `SKILL.md` 发现的工具：

- OpenCode
- Claude Code
- OpenAI Codex
- Cursor
- Windsurf
- GitHub Copilot
- 其他支持 Agent Skills 格式的工具

## 许可证

MIT
