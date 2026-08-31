# AirymaxRT 用户态工程（agent-workload）

> **定位**：Airymax 双工程结构中的**用户态工程**大管理仓（v0.1.4 由
> agent-runtim 改名）。与之对应的**内核态工程**为 [agent-linux]
> （AirymaxOS，Linux 6.6 + sched_tac + eBPF + io_uring + Rust）。两者构建
> 层面零互相引用，通过共享契约层（SC）与语义同源层（SS）协作（IRON-9
> 四层共享模型）。

**语言：** [English](README.md) | 简体中文

**版本：** 0.1.7

## 结构

```
agent-workload/                     ← 用户态工程（大管理仓，v0.1.3 起，v0.1.4 由 agent-runtim 改名）
├── agentrt/      [管理仓]           # 核心运行时：daemon 群 + CMake 构建系统
│                                   #   （含直属 cmake/ 构建模块、scripts/ 安装器）
├── ecosystem/    [管理仓]           # 生态：skills/plugins/prompts/markets/agents …
├── products/     [管理仓]           # 产品：memoryrovol/desktop/docker …
└── sdk/          [管理仓]           # 开发者 SDK：sdk-python/go/rust/ts + cli + tui
```

- 本仓为**容器性质**：根目录仅收编 4 个管理仓子模块指针 + 自身工程文档，
  不含直属源码目录。
- `agentrt/cmake/`（构建系统模块）、`agentrt/scripts/`（官方安装器
  install.sh/install.ps1）、`agentrt/LICENSES/`（SPDX 许可）为 agentrt
  管理仓直属设施，随仓入库。

## 工程边界

| 工程 | 大管理仓 | 内容 | 构建 |
|------|----------|------|------|
| 用户态 | agent-workload | agentrt + ecosystem + products + sdk | agentrt CMake 为运行时构建入口；ecosystem/products/sdk 服务化整合（安装期拷贝 + 运行时协议协作） |
| 内核态 | agent-linux | kernel + services + system + cloudnative | Kbuild + SC 契约层 |

## 子模块

`git submodule update --init --recursive` 拉取全部 4 个管理仓及其叶子仓。
