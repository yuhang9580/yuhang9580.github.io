## 三种Fn特征
闭包捕获变量有三种途径, 恰好对应函数参数的三种传入方式:
1. 转移所有权
2. 可变借用
3. 不可变借用
因此, 相应的 `Fn` 特征也有三种

### `FnOnce`
改类型的闭包会拿走被捕获变量的所有权, `Once`顾名思义, 说明该闭包只能运行一次, 以下是报错代码:
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

因为`F`没有实现`Copy`特征, 所以会报错, 需要增加约束, 实现了`Copy`的闭包:
```rust
fn fn_once<F>(func: F)
where
    F: FnOnce(usize) -> bool + Copy,// 改动在这里
{
    println!("{}", func(3));
    println!("{}", func(4));
}

fn main() {
    let x = vec![1, 2, 3];
    fn_once(|z|{z == x.len()})
}
```

### `FnMut`
`FnMut`以可变借用的方式捕获了环境中的值, 因此可以修改该值, 如下的错误代码为:
```rust
fn main(){
    let mut s = String::new();
    let update_string = |str| s.push_str(str);
    update_string("hello");
    println!("{:?}", s);
}
```

需要将`update_string`修改为可变变量, 代码如下:
```rust
fn main(){
    let mut s = String::new();
    let mut update_string = |str| s.push_str(str);
    update_string("hello");
    println!("{:?}", s);
}
```

以下为一个更复杂的例子:
```rust
fn main(){
    let mut  s = String::new();
    let update_string = |str| s.push_str(str);
    exec(update_string);
    println!("{:?}", s);
}

fn exec<'a,F:FnMut(&'a str)> (mut f:F){
    f("hello")
}
```

## move关键字
如果想要强制闭包去的捕获变量的所有权, 可以在参数列表前添加`move`关键字, 这种用法通常用于闭包声明周期大于捕获变量的声明周期时, 例如将闭包返回或移入其他线程.
```rust
use std::{thread, time::Duration};

fn main(){
    let v = vec![1,2,3];
    let handle = thread::spawn(move || {
        println!("Here's a vector: {:?}", v);
        for i in 0..=10{
            thread::sleep(Duration::from_secs(2));
            println!("Sleep for {} times", i);
        }
    });
    handle.join().unwrap();
    println!("abc");
}
```

