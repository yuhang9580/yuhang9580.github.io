# 三种Fn特征
闭包捕获变量有三种途径, 恰好对应函数参数的三种传入方式:
1. 转移所有权
2. 可变借用
3. 不可变借用
因此, 相应的 `Fn` 特征也有三种

# Chapter 2

```rust
fn main() {
    let v = vec![0; 3];   // 默认值为 0，初始长度为 3
    let v_from = Vec::from([0, 0, 0]);
    assert_eq!(v, v_from);
}
```


```cpp
#include<iostream>
int main(void)
{
    std::cout<<"hello"<<std::endl;
    return 0;
}

```