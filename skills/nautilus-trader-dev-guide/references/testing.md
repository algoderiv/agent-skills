# Nt_Dev - Testing

**Pages:** 1

---

## 测试

**URL:** https://nautilus.aloen.to/developer_guide/testing

**Contents:**
- 测试
- 运行测试​
  - Python 测试​
  - Rust 测试​
  - IDE 集成​
- 测试风格（Test style）​
- 等待异步生效（Waiting for asynchronous effects）​
- Mock 策略​
- 代码覆盖率（Code coverage）​
- 排除覆盖的代码​

自动化测试是交易平台的可执行规范（executable specifications）。健壮的测试套件能记录预期行为，让贡献者在重构时更有信心，并在回归进入生产前将其拦截。 测试同时还是活生生的示例：它们能澄清复杂流程并为 CI 提供快速反馈，从而及早暴露问题。

性能测试有助于推进那些性能敏感组件的演进。

使用 pytest（我们的主要测试运行器）来执行测试。使用参数化测试和 fixtures（例如 @pytest.mark.parametrize）可以避免重复代码并提高可读性。

等待后台工作完成时，尽量使用轮询辅助函数，例如来自 nautilus_trader.test_kit.functions 的 await eventually(...) 和来自 nautilus_common::testing 的 wait_until_async(...)，而非随意的 sleep。这样可以更快暴露失败并减少 CI 的不稳定性，因为轮询会在条件满足时立即返回，或在超时时给出有用的错误信息。

使用轻量级的协作者（collaborators）作为 mock，以保持测试套件简单并避免引入沉重的 mocking 框架。 在某些需要方便工具支持的场景下，我们仍会使用 MagicMock。

我们使用 coverage 生成覆盖率报告，并将报告发布到 Codecov。

追求较高的覆盖率，但不要以牺牲合理的错误处理或导致“测试破坏设计”（test induced damage）为代价。

有些分支在不改变生产行为的前提下无法覆盖。例如，防御性 if-else 结构中的某个最终分支可能只会在出现意外值时触发；请保留这些检查，以便未来的改动可以覆盖到它们。

设计时的异常（design-time exceptions）通常也不易测试，因此不应将 100% 覆盖率作为硬性目标。

对于那些测试成本高且价值有限的代码，我们使用 pragma: no cover 注释来 从覆盖率中排除。 常见示例包括：

这类测试维护成本高且在重构时需频繁更新，价值有限。 确保抽象方法的具体实现仍然得到充分覆盖。当不再适用时应移除 pragma: no cover，并将其使用限制在上述合理场景中。

使用默认的测试配置即可调试 Rust 测试。

若需生成带调试符号的完整测试套件以供后续分析，请运行 make cargo-test-debug 而不是 make cargo-test。

在 IntelliJ IDEA 中，对于参数化的 #[rstest] 用例，请修改运行配置，使其读取如下命令： test --package nautilus-model --lib data::bar::tests::test_get_time_bar_start::case_1 (移除 -- --exact，并在结尾追加 ::case_n，其中 n 从 1 开始)。该变通方法参见此处的讨论：issue 说明。

在 VS Code 中，你可以直接选择要调试的具体测试用例。

该工作流允许你在 VS Code 的 Jupyter notebook 中同时调试 Python 和 Rust 代码。

安装以下 VS Code 扩展：Rust Analyzer、CodeLLDB、Python、Jupyter。

该命令会创建所需的 VS Code 调试配置并为 Python 调试器启动一个 debugpy 服务。

默认情况下，setup_debugging() 会在 nautilus_trader 根目录上一层位置寻找 .vscode 文件夹。 如果你的工作区布局不同，请调整目标位置。

运行在 notebook 中调用 Rust 函数的单元格。调试器将在 Python 与 Rust 的断点处暂停。

setup_debugging() 会创建以下 VS Code 调试配置：

打开并运行示例 notebook：debug_mixed_jupyter.ipynb。

**Examples:**

Example 1 (bash):
```bash
make pytest# oruv run --active --no-sync pytest --new-first --failed-first# or simplypytest
```

Example 2 (bash):
```bash
make test-performance# oruv run --active --no-sync pytest tests/performance_tests --benchmark-disable-gc --codspeed
```

Example 3 (bash):
```bash
make cargo-test# orcargo nextest run --workspace --features "python,ffi,high-precision,defi" --cargo-profile nextest
```

Example 4 (bash):
```bash
cd nautilus_trader && make build-debug-pyo3
```

---
