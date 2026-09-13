# AirymaxRT — User-Space Engineering

English | [简体中文](README_zh.md)

**Current release:** [v0.1.15](https://atomgit.com/openairymax/agentrt/releases/tag/v0.1.15)

This repository collects the **user-space** side of the Airymax platform: the
runtime, the developer SDKs, the agent/skill ecosystem, and the shippable
products built on top of them. If you want to run AI agents on your own
machine or embed the runtime into your own application, this is the place to
start.

The companion **kernel-space** engineering — the AirymaxOS agent operating
system built on Linux 6.6 with `sched_tac`, eBPF and io_uring — lives in
[agent-linux](https://atomgit.com/openairymax/agent-linux). The two sides are
built independently and never reference each other at build time; they
interoperate only through a versioned contract layer, so you can adopt either
one alone.

## What's in here

| Directory | Contents |
|-----------|----------|
| [`agentrt/`](agentrt/) | The runtime itself: micro-kernel primitives, cognitive loop, memory, security dome, protocol stack, gateway, and the daemon fleet. Built with CMake. |
| [`sdk/`](sdk/) | Language bindings and developer tools — Python, Go, Rust and TypeScript SDKs, plus the command-line interface and the terminal UI. |
| [`ecosystem/`](ecosystem/) | Ready-to-use agents, skills, prompt libraries and marketplace tooling that run on the runtime. |
| [`products/`](products/) | Packaged deliverables: desktop application, container images, and the commercial memory provider. |

Each directory is an aggregate repository whose own components are checked out
as git submodules, pinned to the exact commits that were released together.

## Getting started

Most users should install a prebuilt package rather than build from source:

```bash
curl -fsSL "https://api.atomgit.com/api/v5/repos/openairymax/agentrt/contents/scripts/install.sh?ref=main" \
  | python3 -c 'import json,sys,base64;sys.stdout.buffer.write(base64.b64decode(json.load(sys.stdin)["content"]))' \
  | bash
```

On Windows (PowerShell):

```powershell
irm https://atomgit.com/openairymax/agentrt/releases/download/latest/install.ps1 | iex
```

See the [runtime README](agentrt/README.md) for the full platform matrix,
source builds, and licensing.

## Repository layout

```
agent-workload/
├── agentrt/       # AI agent runtime (build entry point: CMake)
├── sdk/           # Language SDKs, CLI, terminal UI
├── ecosystem/     # Agents, skills, prompts, markets
└── products/      # Desktop app, container images, memory provider
```

There is no source code at this level. To fetch everything, including the
nested components:

```bash
git clone --recursive https://atomgit.com/openairymax/agent-workload.git
```

## How the pieces fit together

`agentrt` is the only build entry point for the runtime. The SDK, ecosystem
and product directories integrate at the service level: they are installed
alongside the runtime and talk to it over its published protocols (HTTP,
WebSocket, stdio, JSON-RPC 2.0). Nothing in `sdk/`, `ecosystem/` or
`products/` needs to be compiled together with `agentrt/`.

## License

The runtime, SDKs and ecosystem are dual-licensed under
**AGPL-3.0-or-later OR Apache-2.0**. Individual components carry their own
`LICENSE` file; the commercial memory provider under `products/` is distributed
under a separate SPHARX Ltd. end-user licence.

Copyright (c) 2025-2026 **SPHARX Ltd.** All Rights Reserved.
