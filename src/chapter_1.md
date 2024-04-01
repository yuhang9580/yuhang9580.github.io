## 三种Fn特征
闭包捕获变量有三种途径, 恰好对应函数参数的三种传入方式:
1. 转移所有权
2. 可变借用
3. 不可变借用
因此, 相应的 `Fn` 特征也有三种

## `FnOnce`
改类型的闭包会拿走被捕获变量的所有权, `Once`顾名思义, 说明该闭包只能运行一次:
```rust
fn fn_once<F>(func: F)
where
    F: FnOnce(usize) -> bool,
{
    println!("{}", func(3));
    println!("{}", func(4));
}

fn main() {
    let x = vec![1, 2, 3];
    fn_once(|z|{z == x.len()})
}
```

