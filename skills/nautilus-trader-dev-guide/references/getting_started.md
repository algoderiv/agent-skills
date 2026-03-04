# Nt_Dev - Getting Started

**Pages:** 1

---

## 环境搭建

**URL:** https://nautilus.aloen.to/developer_guide/environment_setup

**Contents:**
- 环境搭建
- 快速设置​
- 构建​
- 更快的构建（可选）​
- 服务​
- Nautilus CLI 开发指南​
- 介绍​
- 安装​
- 命令​
  - Database​

建议在开发时使用 PyCharm Professional，因为它支持 Cython 语法的解析。作为替代，也可以使用安装了 Cython 扩展的 Visual Studio Code。

uv 是我们推荐用来管理 Python 虚拟环境和依赖的工具。

pre-commit 用于在提交时自动运行各类检查、格式化和 lint 工具。

NautilusTrader 日益依赖 Rust，因此系统中也应安装 Rust （参见 安装指南）。

NautilusTrader 必须能在 Linux、macOS 和 Windows 上编译并运行。请注意代码与脚本的可移植性（例如使用 std::path::Path 来处理路径，避免在 shell 脚本中使用只有 Bash 支持的写法等）。

下面的步骤以类 UNIX 系统为主（仅需执行一次以准备开发环境）。

如果你在频繁开发和迭代，采用 debug 编译通常已经足够，而且比完整优化的构建要快得多。 要以 debug 模式安装，请运行：

在打开 Pull Request 之前，请在本地运行格式化与 lint 测试，确保 CI 在首次运行时通过：

务必确保 Rust 编译器报告 0 错误——构建失败会拖慢所有人的开发流程。

由于 .cargo/config.toml 被纳入版本控制，建议将其本地修改设置为被 Git 忽略（skip worktree）：

在修改了任何 .rs、.pyx 或 .pxd 文件后，可通过下列命令重新编译：

如果你在频繁迭代，采用 debug 模式编译通常已足够且速度显著更快。 要以 debug 模式编译，请运行：

使用 cranelift 后端可以显著缩短开发、测试与 IDE 检查的编译时间。但 cranelift 目前需要 nightly toolchain 并做一些额外配置。先安装 nightly toolchain：

在 workspace 的 Cargo.toml 中启用 nightly 特性并为 dev/test profile 使用 "cranelift" 后端。下面的 patch 可以用 git apply <patch> 应用；在推送前可以用 git apply -R <patch> 撤销。

在运行类似 make build-debug 的命令时传入 RUSTUP_TOOLCHAIN=nightly，并在所有 rust-analyzer 的设置中包含该环境以加速构建与 IDE 检查。

你可以使用仓库中 .docker 目录下的 docker-compose.yml 来启动 Nautilus 的开发环境，此操作会启动以下服务：

如果只需要启动部分服务（例如 postgres），可以只指定要启动的服务：

注意：此容器化配置仅用于开发环境。生产环境请使用更严格、更安全的部署方案。

在服务启动后，需要使用 psql 登录并创建名为 nautilus 的 Postgres 数据库。 运行命令并输入 docker 服务配置中的 POSTGRES_PASSWORD：

以 postgres 管理员身份登录后，执行 CREATE DATABASE 命令创建目标数据库（本例使用 nautilus）：

Nautilus CLI 是一个用于与 NautilusTrader 生态系统交互的命令行工具，提供管理 PostgreSQL 数据库和处理交易相关操作的命令集合。

Nautilus CLI 仅在类 UNIX 系统上受支持。

可以通过仓库中的 Makefile 目标安装 Nautilus CLI，该目标底层使用 cargo install，安装后会将 nautilus 可执行文件放入系统 PATH（前提是 Rust 的 cargo 已正确配置）。

运行 nautilus --help 可查看 CLI 的结构与可用命令组：

这些命令用于初始化 PostgreSQL 数据库。使用时需提供正确的连接配置，可以通过命令行参数或在项目根目录（或当前工作目录）放置一个 .env 文件来指定。

Rust analyzer 是流行的 Rust 语言服务器，支持多种 IDE。建议为 rust-analyzer 配置与 make build-debug 相同的环境变量，以加快编译速度。下面给出已验证可用的 VSCode 与 Astro Nvim 配置示例。更多信息请见相关 PR 或 rust-analyzer 配置文档。

（参考：相关 PR：nautechsystems/nautilus_trader#2524；rust-analyzer 文档：configuration）

可将下面的设置加入 VSCode 的 settings.json：

可将下面内容加入 Astro LSP 的配置文件：

**Examples:**

Example 1 (bash):
```bash
uv sync --active --all-groups --all-extras
```

Example 2 (bash):
```bash
make install
```

Example 3 (bash):
```bash
make install-debug
```

Example 4 (bash):
```bash
pre-commit install
```

---
