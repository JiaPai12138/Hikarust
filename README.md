# ollvm plugin / 混淆插件

This plugin is extracted from the [Arkari](https://github.com/KomiMoe/Hikari) project.
该插件提取自 [Arkari](https://github.com/KomiMoe/Hikari) 项目。

> ⚠️ Note: This project has only been tested on **Windows x86**. Other platforms have not been tested.
> ⚠️ 注意：该项目当前仅在 **Windows x86** 下测试，其他平台尚未测试。

![effect.png](assets/effect.png)

---

## Usage / 使用方法

1. Put `ollvm.dll` inside your cargo project root (where `Cargo.toml` is located).
   将 `ollvm.dll` 放置在 Cargo 工程根目录下 (即 `Cargo.toml` 所在位置)。

2. Add the following to your `Cargo.toml`.
   (⚠️ You need the **nightly toolchain** to use this feature.)
   在 `Cargo.toml` 中添加如下配置。
    (⚠️ 注意：此功能需要 **nightly 工具链**。)

```toml
cargo-features = ["profile-rustflags"]  # put at top / 需放在文件开头

rustflags = [
    "-Zllvm-plugins=ollvm.dll",
    "-Cpasses=irobf(irobf-indbr,irobf-icall,irobf-indgv,irobf-cff,irobf-cse,irobf-cie,irobf-cfe)",
]
```

---

## Features / 功能说明

* **Inter-procedural obfuscation**
  混淆过程间相关

* **Indirect branch obfuscation**: Replace direct jumps with indirect jumps and encrypt jump targets
  **间接跳转混淆**：将直接跳转替换为间接跳转，并加密跳转目标
  `-mllvm -irobf-indbr`

* **Indirect function call obfuscation**: Replace direct calls with indirect calls and encrypt target addresses
  **间接函数调用混淆**：将直接调用替换为间接调用，并加密目标函数地址
  `-mllvm -irobf-icall`

* **Indirect global variable reference obfuscation**: Replace direct references with indirect references and encrypt variable addresses
  **间接全局变量引用混淆**：将直接引用替换为间接引用，并加密变量地址
  `-mllvm -irobf-indgv`

* **C string encryption**
  **C 字符串加密功能**
  `-mllvm -irobf-cse`

* **Control flow flattening (procedure-related)**
  **过程相关控制流平坦化混淆**
  `-mllvm -irobf-cff`

* **Basic block splitting**
  **分割基本块**
  `-mllvm -irobf-split`

* **Integer constant encryption**
  **整数常量加密**
  (Requires `Win64-MT-19.1.3-obf1.6.0` or later)
   (需 `Win64-MT-19.1.3-obf1.6.0` 或更高版本)
  `-mllvm -irobf-cie`

* **Floating-point constant encryption**
  **浮点常量加密**
  (Requires `Win64-MT-19.1.3-obf1.6.0` or later)
   (需 `Win64-MT-19.1.3-obf1.6.0` 或更高版本)
  `-mllvm -irobf-cfe`

* **Microsoft CXXABI RTTI name erasure (experimental feature)**
  **Microsoft CXXABI RTTI 名称擦除器 (实验性功能)**
  Requires specifying a configuration file path, and the configuration file must contain a `randomSeed` field (32 bytes. If shorter, it will be padded with zeros; if longer, it will be truncated).
  需要指定配置文件路径，并在配置文件中提供 `randomSeed` 字段 (32 字节，不足会在后面补 0，超过会截断)。
  (Requires `Win64-MT-20.1.7-obf1.7.0` or later)
   (需 `Win64-MT-20.1.7-obf1.7.0` 或更高版本)
  `-mllvm -irobf-rtti`

* **Enable all features at once**
  **启用全部功能**
  `-mllvm -irobf-indbr -mllvm -irobf-icall -mllvm -irobf-indgv -mllvm -irobf-cse -mllvm -irobf-cff -mllvm -irobf-split -mllvm -irobf-cie -mllvm -irobf-cfe -mllvm -irobf-rtti`

* **Configuration file management**
  **通过配置文件管理**
  `-mllvm -arkari-cfg="配置文件路径|Your config path"`
  (Requires `Win64-MT-20.1.7-obf1.7.0` or later)
   (需 `Win64-MT-20.1.7-obf1.7.0` 或更高版本)

---

## Rust Dynamic Loading / Rust 动态加载

When dynamically loading LLVM pass plugins, you must switch to the **nightly channel**.
在 Rust 中动态加载 LLVM Pass 插件时，必须切换到 **nightly 通道**。

See: [Allow loading of LLVM plugins (when dynamically built Rust)](https://github.com/rust-lang/rust/pull/82734)
参考：[Allow loading of LLVM plugins (when dynamically built Rust)](https://github.com/rust-lang/rust/pull/82734)

---

## References / 参考

- [Allow loading of LLVM plugins [when dynamically built rust]](https://github.com/rust-lang/rust/pull/82734)
- [Installing from Source](https://github.com/rust-lang/rust/blob/master/INSTALL.md)
- [rustc dev guide](https://rustc-dev-guide.rust-lang.org/building/how-to-build-and-run.html)
- [Windows rust使用LLVM pass](https://bbs.kanxue.com/thread-274453.htm)
- [Orust Mimikatz Bypass Kaspersky](https://b1n.io/posts/orust-mimikatz-bypass-kaspersky/)
- [Building LLVM with CMake](https://llvm.org/docs/CMake.html#developing-llvm-passes-out-of-source)
- [llvm-tutor](https://github.com/banach-space/llvm-tutor)
- [String encryption failed](https://github.com/joaovarelas/Obfuscator-LLVM-16.0/issues/8)

## Special Thanks / 特别鸣谢

- [Arkari](https://github.com/KomiMoe/Hikari)
- [0xlane/ollvm-rust](https://github.com/0xlane/ollvm-rust)
- [wyxather/ollvm](https://github.com/wyxather/ollvm)
