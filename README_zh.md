# AirymaxRT 用户态工程

[English](README.md) | 简体中文

**当前版本：** [v0.1.15](https://atomgit.com/openairymax/agentrt/releases/tag/v0.1.15)

本仓库汇集 Airymax 平台的**用户态**工程：智能体运行时、面向开发者的 SDK、
智能体与技能生态，以及基于它们交付的产品。如果你希望在自己的机器上运行
AI 智能体，或把运行时嵌入自己的应用，从这里开始即可。

与之配套的**内核态**工程是基于 Linux 6.6 并引入 `sched_tac`、eBPF、io_uring
的智能体操作系统 AirymaxOS，位于
[agent-linux](https://atomgit.com/openairymax/agent-linux)。两侧工程各自独立
构建，构建期互不引用，仅通过带版本约束的契约层协作，因此可以单独采用其中任意一侧。

## 目录构成

| 目录 | 内容 |
|------|------|
| [`agentrt/`](agentrt/) | 运行时本体：微内核原语、认知循环、记忆、安全穹顶、协议栈、网关与守护进程集群，使用 CMake 构建。 |
| [`sdk/`](sdk/) | 各语言绑定与开发者工具——Python、Go、Rust、TypeScript SDK，以及命令行工具与终端界面。 |
| [`ecosystem/`](ecosystem/) | 可直接使用的智能体、技能、Prompt 库与市场工具。 |
| [`products/`](products/) | 交付形态的产品：桌面应用、容器镜像与商业记忆提供方。 |

上述每个目录本身是聚合仓库，其内部组件以 git submodule 形式引入，并被固定到
同一次发布所对应的确切提交。

## 快速开始

多数用户应直接安装预构建包，无需自行编译：

```bash
curl -fsSL "https://api.atomgit.com/api/v5/repos/openairymax/agentrt/contents/scripts/install.sh?ref=main" \
  | python3 -c 'import json,sys,base64;sys.stdout.buffer.write(base64.b64decode(json.load(sys.stdin)["content"]))' \
  | bash
```

Windows（PowerShell）：

```powershell
irm https://atomgit.com/openairymax/agentrt/releases/download/latest/install.ps1 | iex
```

完整的平台支持矩阵、源码构建方式与许可说明见[运行时 README](agentrt/README_zh.md)。

## 仓库结构

```
agent-workload/
├── agentrt/       # AI 智能体运行时（构建入口：CMake）
├── sdk/           # 多语言 SDK、CLI、终端界面
├── ecosystem/     # 智能体、技能、Prompt、市场
└── products/      # 桌面应用、容器镜像、记忆提供方
```

本层不含源码目录。递归拉取全部组件：

```bash
git clone --recursive https://atomgit.com/openairymax/agent-workload.git
```

## 各部分如何协作

`agentrt` 是运行时唯一的构建入口。SDK、生态与产品目录以服务化的方式集成：
它们与运行时一同安装，并通过运行时对外发布的协议（HTTP、WebSocket、stdio、
JSON-RPC 2.0）与之交互。`sdk/`、`ecosystem/`、`products/` 均无需与 `agentrt/`
一起编译。

## 许可

运行时、SDK 与生态采用 **AGPL-3.0-or-later OR Apache-2.0** 双许可证。各组件
目录下均带有自己的 `LICENSE` 文件；`products/` 下的商业记忆提供方以独立的
SPHARX Ltd. 最终用户许可协议分发。

Copyright (c) 2025-2026 **SPHARX Ltd.** All Rights Reserved.
