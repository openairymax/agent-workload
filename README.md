# AirymaxRT User-Space Engineering (agent-workload)

> **Role**: The **user-space engineering** super-repo within the Airymax dual-engineering structure (renamed from agent-runtim in v0.1.4). Its kernel-space counterpart is [agent-linux](https://atomgit.com/openairymax/agent-linux) (AirymaxOS, Linux 6.6 + sched_tac + eBPF + io_uring + Rust). The two repos have zero build-time cross-references and collaborate through the Shared Contract layer (SC) and Semantic Source layer (SS) per the IRON-9 four-tier sharing model.

**Language:** English | [简体中文](README_zh.md)

**Version:** 0.1.6b

## Structure

```
agent-workload/                     ← User-space engineering super-repo (since v0.1.3, renamed from agent-runtim in v0.1.4)
├── agentrt/      [management repo]  # Core runtime: daemon fleet + CMake build system
│                                    #   (includes cmake/ build modules, scripts/ installers)
├── ecosystem/    [management repo]  # Ecosystem: skills/plugins/prompts/markets/agents …
├── products/     [management repo]  # Products: memoryrovol/desktop/docker …
└── sdk/          [management repo]  # Developer SDK: sdk-python/go/rust/ts + cli + tui
```

- This repo is **container-nature**: the root directory only holds 4 management-repo submodule pointers plus its own project documentation, with no first-party source code directories.
- `agentrt/cmake/` (build system modules), `agentrt/scripts/` (official installers: install.sh / install.ps1), and `agentrt/LICENSES/` (SPDX licenses) are first-party facilities of the agentrt management repo and ship with it.

## Engineering Boundary

| Engineering | Super-repo | Contents | Build |
|-------------|-----------|----------|-------|
| User-space | agent-workload | agentrt + ecosystem + products + sdk | agentrt CMake is the runtime build entry; ecosystem/products/sdk integrate via service-level composition (install-time copy + runtime protocol cooperation) |
| Kernel-space | agent-linux | kernel + services + system + cloudnative | Kbuild + SC contract layer |

## Submodules

`git submodule update --init --recursive` fetches all 4 management repos and their leaf repos.
