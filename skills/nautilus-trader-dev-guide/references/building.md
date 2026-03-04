# Nt_Dev - Building

**Pages:** 2

---

## 基准测试

**URL:** https://nautilus.aloen.to/developer_guide/benchmarking

**Contents:**
- 基准测试
- 工具概览​
- 目录布局​
- 编写 Criterion 基准​
- 编写 iai 基准​
- 在本地运行基准​
  - 生成 flamegraph​
    - Linux​
    - macOS​
- 模板​

本指南说明 NautilusTrader 如何衡量 Rust 代码性能、何时使用不同的基准工具，以及在新增基准测试（benches）时应遵循的约定。

NautilusTrader 使用两种互补的基准框架：

大多数热点路径从两种测量中都能受益——Criterion 提供易于理解的时间与置信区间，iai 则带来精确的指令级比较。

每个 crate 的性能测试都放在各自的本地 benches/ 目录下：

需要在该 crate 的 Cargo.toml 中显式列出每个 benchmark，以便 cargo bench 能发现它们：

iai 要求基准函数不接受参数并返回一个值（该返回值可以被忽略）。函数应尽量小巧，以确保测得的指令数有意义。

Criterion 会把 HTML 报告写入 target/criterion/，在浏览器中打开 target/criterion/report/index.html 即可查看。

cargo-flamegraph 可生成基准的采样调用栈（flamegraph），在 Linux 上基于 perf，在 macOS 上使用 DTrace。

在 Linux 上需要安装 perf。在 Debian/Ubuntu 上可以使用：

如果出现与 perf_event_paranoid 相关的错误，需要在当前会话中放宽内核的 perf 限制（需要 root）：

通常设置为 1 即足够；如果需要可以恢复为 2（默认值），或通过 /etc/sysctl.conf 做永久更改。

在 macOS 上，DTrace 需要 root 权限，因此必须使用 sudo 运行 cargo flamegraph：

使用 sudo 运行会在 target/ 目录中生成由 root 用户拥有的文件。这可能会导致后续运行 cargo 命令时出现权限错误。

解决方法是手动删除这些 root 拥有的文件，或运行 sudo cargo clean 来清除整个 target/ 目录。

由于 [profile.bench] 保留了完整的调试符号，生成的 SVG 会显示可读的函数名，同时不会让生产构建变得臃肿（生产构建仍通过 [profile.release] 使用 panic = "abort"）。

注意 基准二进制使用工作区 Cargo.toml 中自定义的 [profile.bench] 进行编译。该配置继承自 release-debugging，在保持完全优化的同时保留调试符号，从而使 cargo flamegraph 或 perf 等工具生成的人类可读的调用栈。

可直接复制使用的起始模板文件位于 docs/dev_templates/：

将模板复制到 benches/，根据需要调整导入和名称，然后开始测量！

**Examples:**

Example 1 (typescript):
```typescript
crates/<crate_name>/└── benches/    ├── foo_criterion.rs   # Criterion 组    └── foo_iai.rs         # iai 微基准
```

Example 2 (json):
```json
[[bench]]name = "foo_criterion"             # benches/ 下文件名（不含扩展名）path = "benches/foo_criterion.rs"harness = false                    # 关闭默认的 libtest harness
```

Example 3 (rust):
```rust
use std::hint::black_box;use criterion::{Criterion, criterion_group, criterion_main};fn bench_my_algo(c: &mut Criterion) {    let data = prepare_data(); // 重量级的准备工作只做一次    c.bench_function("my_algo", |b| {        b.iter(|| my_algo(black_box(&data)));    });}criterion_group!(benches, bench_my_algo);criterion_main!(benches);
```

Example 4 (rust):
```rust
use std::hint::black_box;fn bench_add() -> i64 {    let a = black_box(123);    let b = black_box(456);    a + b}iai::main!(bench_add);
```

---

## Rust 风格指南

**URL:** https://nautilus.aloen.to/developer_guide/rust

**Contents:**
- Rust 风格指南
- Cargo 清单（manifest）约定​
- 版本管理建议​
- 特性（feature）约定​
- 模块组织​
- 代码风格与约定​
  - 文件头要求​
  - 代码格式化​
    - 字符串格式​
  - 日志（Logging）​

Rust 语言非常适合实现平台和系统的关键核心部分。 它强类型系统、所有权模型与编译期检查在设计上消除了内存错误和数据竞争， 而零成本抽象与无垃圾回收机制则提供了接近 C 的性能，这对高频交易这类对延迟和吞吐敏感的工作负载至关重要。

所有 Rust 文件必须包含标准化的版权头：

运行 make format 时，rustfmt 会自动处理导入（import）的格式化。 该工具会将导入按组（标准库、外部 crate、本地导入）划分，并在每个组内按字母顺序排序。

优先使用带变量名的内联格式字符串，而非位置参数：

这种方式使得错误消息更具可读性与自说明性，尤其在变量较多时更明显。

主要模式：对可能失败的函数使用 anyhow::Result<T>：

自定义错误类型：对领域特定错误使用 thiserror：

错误传播：使用 ? 运算符实现清晰的错误传播。

构造错误：偏好用 anyhow::bail! 做早期返回：

注意：在需要闭包上下文（如 ok_or_else()）而无法早期返回时，使用 anyhow::anyhow!。

在 new() 与 new_checked() 之间保留一致约定：

始终在与正确性检查相关的 .expect() 信息中使用 FAILED 常量：

对描述性常量使用 SCREAMING_SNAKE_CASE：

优先使用 ahash crate 提供的 AHashMap 和 AHashSet，替代标准库的 HashMap 和 HashSet：

何时使用标准 HashMap/HashSet：

按字母顺序组织 re-export，并放在 lib.rs 文件末尾：

所有模块必须有模块级文档，并以简短描述开始：

对于带有特性开关的模块，要清晰地记录这些特性：

所有结构体和枚举字段必须有以句号结尾的文档注释：

针对单行错误与 panic 的文档，使用句子式大小写并遵循下列约定：

对于多行的错误与 panic 文档，使用句子式大小写，并以要点形式列出每项，句尾以句号结束：

对于 Safety 文档，使用 SAFETY: 前缀，简要说明为何该 unsafe 操作是安全的：

对于内联的 unsafe 块，在 unsafe 代码上方直接添加 SAFETY: 注释：

通过 Cython 和 PyO3 提供 Python 绑定，使用户无需 Rust 工具链即可在 Python 中直接导入 NautilusTrader 的 crate。

在通过 PyO3 向 Python 暴露 Rust 函数时：

在处理 PyO3 绑定时，必须理解并避免 Rust 的 Arc 引用计数与 Python 垃圾回收器之间产生的引用环。 本节记录了在持有回调的结构体中处理 Python 对象的最佳实践。

问题：在持有回调的结构体中使用 Arc<PyObject> 会产生循环引用：

解决思路：使用普通的 PyObject 并通过 clone_py_object() 在获取 GIL 的上下文中安全克隆：

clone_py_object() 的实现要点：

这种方法允许 Rust 与 Python 的垃圾回收机制协同工作，避免因引用环导致的内存泄漏。

为了在 Cython 与 Rust 之间实现互操作，编写 unsafe Rust 代码是必要的。跳出安全 Rust 的边界使我们能实现许多 Rust 语言自身的底层特性，正如 C 和 C++ 用于实现它们自己的标准库一样。

在使用 Rust 的 unsafe 功能时务必非常谨慎——unsafe 只是启用一小部分额外的语言能力，它改变了接口与调用者之间的契约，将部分正确性保证的责任从编译器转移给我们。目标是在利用 unsafe 带来能力的同时，避免任何未定义行为（undefined behavior）。 关于 Rust 语言设计者对未定义行为的定义，请参见语言参考。

为保持正确性，任何使用 unsafe 的代码都必须遵循我们的策略：

Crate 级 lint ——每个导出 FFI 符号的 crate 都应启用 #![deny(unsafe_op_in_unsafe_fn)]。即便位于 unsafe fn 内部，每次指针解除引用或其他危险操作也必须用独立的 unsafe { … } 包裹。

CVec 合约 ——对于跨 FFI 边界的原始向量，请阅读 FFI Memory Contract。外部代码会成为该分配的所有者，必须恰好调用一次对应的 vec_drop_* 函数。

**Examples:**

Example 1 (rust):
```rust
// -------------------------------------------------------------------------------------------------//  Copyright (C) 2015-2025 Nautech Systems Pty Ltd. All rights reserved.//  https://nautechsystems.io////  Licensed under the GNU Lesser General Public License Version 3.0 (the "License");//  You may not use this file except in compliance with the License.//  You may obtain a copy of the License at https://www.gnu.org/licenses/lgpl-3.0.en.html////  Unless required by applicable law or agreed to in writing, software//  distributed under the License is distributed on an "AS IS" BASIS,//  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.//  See the License for the specific language governing permissions and//  limitations under the License.// -------------------------------------------------------------------------------------------------
```

Example 2 (rust):
```rust
// Preferred - inline format with variable namesanyhow::bail!("Failed to subtract {n} months from {datetime}");// Instead of - positional argumentsanyhow::bail!("Failed to subtract {} months from {}", n, datetime);
```

Example 3 (rust):
```rust
pub fn calculate_balance(&mut self) -> anyhow::Result<Money> {    // Implementation}
```

Example 4 (rust):
```rust
#[derive(Error, Debug)]pub enum NetworkError {    #[error("Connection failed: {0}")]    ConnectionFailed(String),    #[error("Timeout occurred")]    Timeout,}
```

---
