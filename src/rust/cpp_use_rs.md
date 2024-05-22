# c++调用rust

## 1，创建rust项目
```sh
cargo new cpp_call_rs; cd cpp_call_rs
```

## 2，修改cargo.toml
- 添加以下内容:
```toml
[lib]
crate-type = ["staticlib"]
```

## 3，写rust代码(lib.rs)，删除main.rs
```rust
#[no_mangle]
pub extern "C" fn rust_add(a: i32, b: i32) -> i32 {
    a + b
}
```
然后cargo build --release


## 4，写cpp代码
```cpp
#include <iostream>
namespace rust {
    extern "C" {
    int rust_add(int a, int b);
    }
}  // namespace rust

int main()
{
    int result = rust::rust_add(3, 4);
    std::cout << "The result is: " << result << std::endl;
    return 0;
}

```

## 5，编译cpp项目
```sh
/data/data/com.termux/files/usr/bin/g++ main.cpp \
-lcpp_call_rs \
-L$(pwd)/cpp_call_rs/target/release \
-o cpp_call_rs
```
执行`./cpp_call_rs`即可
