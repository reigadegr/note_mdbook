# rust调用c++

### 步骤 1: 编写C++代码

首先，你需要有一个C++的源文件，比如 `add.cpp`，内容如下：

```cpp
// add.cpp
extern "C" {
    int add(int a, int b) {
        return a + b;
    }
}
```

注意：我们使用 `extern "C"` 来确保C++函数遵循C的链接规则，这样Rust才能正确调用。

### 步骤 2: 编译C++代码为静态链接库

使用 `clang++` 编译C++代码为静态链接库：

```sh
# 先编译成目标文件
g++ -c -fPIC add.cpp

# 然后打包成静态库
ar rcs libadd.a add.o
```

生成libadd.a这个库

### 步骤 3: 在Rust中调用C++代码

在Rust中，你需要使用 `extern` 关键字来声明外部函数。创建一个新的Rust项目：

```sh
cargo new rust_call_cpp; cd rust_call_cpp
```

然后，在 `src/main.rs` 中添加以下代码：

```rust
extern "C" {
    fn add(a: i32, b: i32) -> i32;
}

fn main() {
    let result = unsafe { add(10, 20) };
    println!("The result is: {}", result);
}
```

### 步骤 4: 配置Cargo.toml

你需要在 `Cargo.toml` 文件中添加一个构建脚本，以便在构建Rust项目时链接到动态链接库：

```toml
[package]
name = "rust_call_cpp"
version = "0.1.0"
edition = "2021"

build = "build.rs"
```

然后创建 `build.rs` 文件，内容如下：

```rust
fn main() {
     println!("cargo:rustc-link-search=native=/data/data/com.termux/files/home/cpprust");
    println!("cargo:rustc-link-lib=static=add");
}
```

其中 `/data/data/com.termux/files/home/cpprust` 为编译后的libadd.a的dirname

### 步骤 5: 构建并运行Rust项目

最后，构建并运行你的Rust项目：

```sh
cargo run
```

如果一切顺利，你将看到输出结果 "The result is: 30"。
