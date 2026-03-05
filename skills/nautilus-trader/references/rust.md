# Nt_Dev - Rust

**Pages:** 1

---

## FFI 内存约定

**URL:** https://nautilus.aloen.to/developer_guide/ffi

**Contents:**
- FFI 内存约定
- 在 FFI 边界上采用 fail-fast 的 panic 处理​
- CVec 的生命周期​
- 在 Python 侧创建的 Capsule​
- 在 Rust 侧创建的 Capsule（PyO3 绑定）​
- 为什么不再提供通用的 cvec_drop​
- 基于 Box 的 *_API 包装（Rust 所拥有的对象）​

NautilusTrader 对外暴露了若干 与 C 兼容（C-compatible） 的类型，便于将编译后的 Rust 代码被 Cython 生成的 C 扩展或其他本地语言所调用。其中最重要的是 CVec —— 它是对 Rust Vec<T> 的一个轻量封装（thin wrapper），并以**值传递（by value）**的方式跨越 FFI 边界。

下面的规则非常严格；任何违反都会导致未定义行为（通常表现为双重释放 double-free 或内存泄漏）。

Rust 的 panic 绝不能在 extern "C" 函数边界处展开（unwind）到 C 或 Python 代码中。将 unwind 传播到 C/Python 是未定义行为，可能破坏外部栈或留下未完全释放的资源。为保证 fail-fast 架构，我们在每个导出的符号外层都包裹了一层 crate::ffi::abort_on_panic，该函数会执行主体代码并在发生 panic 时调用 process::abort()。在中止之前会先记录 panic 信息，这样既保留了调试输出，又避免了未定义行为。

在添加新的 FFI 函数时，请在实现周围调用 abort_on_panic(|| { … })（或使用等效的 helper），以维持该保证。

如果忘记执行步骤 3，分配将会在进程生命周期内泄漏；如果执行了两次，程序将发生 double-free 并可能崩溃。

若干 Cython 的辅助函数会使用 PyMem_Malloc 分配临时的 C 缓冲区，将其封装为 CVec，并把地址放入 PyCapsule 返回。每个此类 capsule 都会注册一个 destructor（例如 capsule_destructor 或 capsule_destructor_deltas），负责释放缓冲区与 CVec 本身。因此调用方切忌手动释放这些内存——否则会造成双重释放。

当 Rust 代码将堆上分配的值传递给 Python 时，必须使用 PyCapsule::new_with_destructor，以便 Python 在 capsule 不再可达时能正确释放该分配。closure/析构器的职责是重建原始的 Box<T> 或 Vec<T> 并让其析构。

不要使用 PyCapsule::new(…, None)；该变体不会注册析构器，会导致内存泄漏（除非接收方手动取出并释放指针，而这是我们从不依赖的方式）。代码库已经统一遵循这一规则——新增的 FFI 模块也必须遵守相同模式。

早期版本中存在一个通用的 cvec_drop，它始终将缓冲区视为 Vec<u8>。若对其它类型使用该函数，会在释放时产生大小不匹配（size mismatch），从而破坏分配器的元数据。由于该 helper 在项目中没有被引用，为避免误用已将其移除。

当 Rust 核心需要将一个较为“复杂”的值（例如 OrderBook、SyntheticInstrument 或 TimeEventAccumulator）交给外部代码时，会在堆上使用 Box::new 分配该值，并返回一个小型的 repr(C) 包装结构，其唯一字段就是该 Box。

每个构造函数（*_new）必须在旁边导出一个对应的 *_drop。

Python/Cython 绑定必须保证 *_drop 被恰好调用一次。可接受的两种做法：

• 把指针封装到通过 PyCapsule::new_with_destructor 创建的 PyCapsule 中，传入一个在析构时调用 drop helper 的析构器。

• 在 Python 侧的 __del__/__dealloc__ 中显式调用该 helper。这是大多数 v1 Cython 模块的传统做法：

无论采用哪种方式，务必记住：忘记调用 drop 会导致整个结构泄漏，而调用两次会导致 double-free 并崩溃。

新增的 FFI 代码在合并前必须遵循此模板。

**Examples:**

Example 1 (rust):
```rust
Python::attach(|py| {    // 在堆上分配值    let my_data = MyStruct::new();    // 将其移动到 capsule 中并注册一个析构器    let capsule = pyo3::types::PyCapsule::new_with_destructor(py, my_data, None, |_, _| {})        .expect("capsule creation failed");    // ... 将 `capsule` 传回 Python ...});
```

Example 2 (rust):
```rust
#[repr(C)]pub struct OrderBook_API(Box<OrderBook>);#[unsafe(no_mangle)]pub extern "C" fn orderbook_new(id: InstrumentId, book_type: BookType) -> OrderBook_API {    OrderBook_API(Box::new(OrderBook::new(id, book_type)))}#[unsafe(no_mangle)]pub extern "C" fn orderbook_drop(book: OrderBook_API) {    drop(book); // 释放堆上分配}
```

Example 3 (python):
```python
cdef class OrderBook:    cdef OrderBook_API _mem    def __cinit__(self, ...):        self._mem = orderbook_new(...)    def __del__(self):        if self._mem._0 != NULL:            orderbook_drop(self._mem)
```

---
