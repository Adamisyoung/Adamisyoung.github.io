---
layout: post
title:  rustlings-modules
date:   2021-07-04 19:15:31 +0800
categories:
- Rust/rustlings
---

> rustlings系列题解

Rust的模块系统：
* 包：Cargo 的一个功能，它允许你构建、测试和分享 crate。
* Crates：一个模块的树形结构，它形成了库或二进制项目。
* 模块和use：允许你控制作用域和路径的私有性。
* 路径：一个命名例如结构体、函数或模块等项的方式
  
# module1
```rust
// modules1.rs
// Make me compile! Execute `rustlings hint modules1` for hints :)


mod sausage_factory {
    fn make_sausage() {
        println!("sausage!");
    }
}

fn main() {
    sausage_factory::make_sausage();
}
```
先不说题目，我们谈谈模块系统的事。

模块系统的第一部分就是包和crate。crate root是一个源文件，Rust编译器以它为起始点，并构成crate的根模块。包中所包含的内容由几条规则来确立：
* 一个包中至多只能包含一个库crate(library crate)；
* 包中可以包含任意多个二进制crate(binary crate)；
* 包中至少包含一个crate，无论是库的还是二进制的。

我们定义一个模块，是以mod关键字为起始，然后指定模块的名字，并且用花括号包围模块的主体。在模块内，我们还可以定义其他的模块，模块还可以保存一些定义的其他项，比如结构体、枚举、常量、特性、或者函数。

Rust使用路径的方式在模块树中找到一个项的位置。路径有两种：
* 绝对路径：从crate根开始，以crate名或者字面值crate开头。
* 相对路径：从当前模块开始，以self、super或当前模块的标识符开头。

绝对路径和相对路径都后跟一个或多个由双冒号::分割的标识符。题目中是以相对路径的方式访问的。

这道题编译不通过的一个原因就是因为Rust中默认所有项(函数、方法、结构体、枚举、模块和常量)都是私有的。所以添加pub关键字即可通过。
```rust
mod sausage_factory {
    pub fn make_sausage() { //使用 `pub` 修饰语来改变默认可见性。
        println!("sausage!");  
    }
}
```

# module2
```rust
  
// modules2.rs
// Make me compile! Execute `rustlings hint modules2` for hints :)


mod delicious_snacks {
    use self::fruits::PEAR as fruit;
    use self::veggies::CUCUMBER as veggie;

    mod fruits {
        pub const PEAR: &'static str = "Pear";
        pub const APPLE: &'static str = "Apple";
    }

    mod veggies {
        pub const CUCUMBER: &'static str = "Cucumber";
        pub const CARROT: &'static str = "Carrot";
    }
}

fn main() {
    println!(
        "favorite snacks: {} and {}",
        delicious_snacks::fruit,
        delicious_snacks::veggie
    );
}
```

这道题引入路径的方式是用use。在作用域中增加use和路径类似于在文件系统中创建软连接，通过添加use self::fruits::PEAR as fruit，现在fruit在当前的作用域就是有效的名称了。as关键字的作用是提供一个新的名称，类似于重命名。

但是上述题目代码会报错，因为当使用use关键字将名称导入作用域时，在新作用域中可用的名称是私有的。所以结合pub和use，将名称重导出即可解决问题。
```rust
pub use self::fruits::PEAR as fruit;
pub use self::veggies::CUCUMBER as veggie;  
```

