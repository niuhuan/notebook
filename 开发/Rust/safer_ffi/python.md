
https://github.com/getditto/safer_ffi

## 文件夹结构

```shell
prffi/
  |-prffi-rust/
  | |-Cargo.toml
  | |-src/
  |   |-lib.rs
  |   |-bin/
  |     |-generate-headers.rs
  |
  |-_ffi_build.py
  |-main.py
```

## rust项目

1. `cargo build` 生成静态库
2. `cargo run --features headers --bin generate-headers`  生成头文件

`Cargo.toml`

```toml
[package]
name = "prffi"
version = "0.1.0"
edition = "2021"

[lib]
crate-type = [
    "staticlib", # Ensure it gets compiled as a (static) C library
    # "cdylib",     # If you want a shared/dynamic C library (advanced)
    "lib", # For `generate-headers` and other downstream rust dependents
]

[[bin]]
name = "generate-headers"
required-features = ["headers"]  # Do not build unless generating headers.

[dependencies]

[dependencies.safer-ffi]
version = "0.1.12"
features = ["python-headers"]

[features]
headers = ["safer-ffi/headers"]
```

`lib.rs`

```rust
use ::safer_ffi::prelude::*;

#[derive_ReprC]
#[repr(C)]
#[derive(Debug, Clone, Copy)]
pub struct Point {
    x: f64,
    y: f64,
}

#[ffi_export]
fn mid_point(a: &Point, b: &Point) -> Point {
    Point {
        x: (a.x + b.x) / 2.,
        y: (a.y + b.y) / 2.,
    }
}

#[ffi_export]
fn print_point(point: &Point) {
    println!("{:?}", point);
}

#[cfg(feature = "headers")]
pub fn generate_headers() -> ::std::io::Result<()> {
    ::safer_ffi::headers::builder()
        .with_language(safer_ffi::ඞ::Language::Python)
        .to_file("prffi.cffi")?
        .generate()?;

    ::safer_ffi::headers::builder()
        .to_file("prffi.h")?
        .generate()?;
    Ok(())
}
```

`generate-headers.rs`
```rust
fn main() -> ::std::io::Result<()> {
    ::prffi::generate_headers()
}
```

## Python项目

1. 运行_ffi_build.py生成.so文件
2. 运行main.py调用.so文件

`_ffi_build.py`

```shell
from cffi import FFI

ffi = FFI()

def prepare_compilation():
    with open("prffi-rust/prffi.cffi") as content:
        ffi.cdef(content.read())

    with open("prffi-rust/prffi.h") as content:
        ffi.set_source("_ffi", content.read(),
            library_dirs = ["prffi-rust/target/debug"],
            libraries = ['prffi']
        )

if __name__ == "__main__":
    prepare_compilation()
    ffi.compile(verbose=True)
```

`main.py`

```python
from cffi import FFI
ffi = FFI()

read_header_file = lambda x: open(x, "r")
with read_header_file("prffi-rust/prffi.cffi") as content:
    ffi.cdef(content.read())

DL = ffi.dlopen("_ffi.cpython-312-darwin.so")
Point = ffi.new("Point_t *", [1, 2])

DL.print_point(Point)
```
