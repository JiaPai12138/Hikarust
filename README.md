# ollvm

## Usage
1. Put ollvm.dll inside your cargo (where Cargo.toml located).
2. Add this to your Cargo.toml. (Due to this you'll need nightly toolchain.)
```
cargo-features = ["profile-rustflags"]  # put at top

rustflags = [
    "-Zllvm-plugins=ollvm.dll",
    "-Cpasses=irobf(irobf-indbr,irobf-icall,irobf-indgv,irobf-cff,irobf-cse,irobf-cie,irobf-cfe)",
]
```

## Features
 - 混淆过程间相关
 - 间接跳转,并加密跳转目标(`-mllvm -irobf-indbr`)
 - 间接函数调用,并加密目标函数地址(`-mllvm -irobf-icall`)
 - 间接全局变量引用,并加密变量地址(`-mllvm -irobf-indgv`)
 - 字符串(c string)加密功能(`-mllvm -irobf-cse`)
 - 过程相关控制流平坦混淆(`-mllvm -irobf-cff`)
 - 分割基本块(`-mllvm -irobf-split`)
 - 整数常量加密(`-mllvm -irobf-cie`) (Win64-MT-19.1.3-obf1.6.0 or later)
 - 浮点常量加密(`-mllvm -irobf-cfe`) (Win64-MT-19.1.3-obf1.6.0 or later)
 - Microsoft CXXABI RTTI Name 擦除器 (实验性功能!) [需要指定配置文件路径 以及 配置文件`randomSeed`字段(32字节，不足会在后面补0，超过会截断)] (`-mllvm -irobf-rtti`) (Win64-MT-20.1.7-obf1.7.0 or later)
 - 全部 (`-mllvm -irobf-indbr -mllvm -irobf-icall -mllvm -irobf-indgv -mllvm -irobf-cse -mllvm -irobf-cff -mllvm -irobf-split -mllvm -irobf-cie -mllvm -irobf-cfe -mllvm -irobf-rtti`)
 - 或直接通过配置文件管理(`-mllvm -arkari-cfg="配置文件路径|Your config path"`) (Win64-MT-20.1.7-obf1.7.0 or later)

## References
[0xlane/ollvm-rust](https://github.com/0xlane/ollvm-rust)<br>[KomiMoe/Hikari](https://github.com/komimoe/Hikari)
