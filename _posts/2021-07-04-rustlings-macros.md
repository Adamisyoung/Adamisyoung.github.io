---
title:  rustlings-macros
categories:
- Rust/rustlings
tags: 
---

> rustlings系列题解

# macros1
```rust
// macros1.rs
// Make me compile! Execute `rustlings hint macros1` for hints :)


macro_rules! my_macro {
    () => {
        println!("Check out my macro!");
    };
}

fn main() {
    my_macro();
}

```

这个题目的问题是调用宏的时候没加！，现在这样是相当于调用名为my_macro()的函数。只要加个!就能解决。

但是我们也详细说说macro_rules!，这个将来会被淘汰的宏类型。

我们已经用过很多种宏了。但是还没完全探索什么是宏以及它是如何工作的。

宏(Macro)指的是Rust中一系列的功能：声明(Declarative)宏，使用 macro_rules!，和三种过程(Procedural)宏：

* 自定义#[derive]宏: 在结构体和枚举上指定通过derive属性添加的代码
* 类属性(Attribute)：宏定义可用于任意项的自定义属性
* 类函数宏：看起来像函数不过作用于作为参数传递的token

从根本上来说，宏是一种为写其他代码而写代码的方式，即所谓的元编程。元编程对于减少大量编写和维护的代码是非常有用的，它也扮演了函数的角色。但宏有一些函数所没有的附加能力。一个函数标签必须声明函数参数个数和类型。相比之下，宏只接受一个可变参数：用一个参数调用println!("hello")或用两个参数调用println!("hello {}", name)。而且，宏可以在编译器翻译代码前展开，例如，宏可以在一个给定类型上实现trait。而函数则不行，因为函数是在运行时被调用，同时trait需要在编译时实现。

实现一个宏而不是函数的消极面是宏定义要比函数定义更复杂，因为你正在编写生成Rust代码的Rust代码。由于这样的间接性，宏定义通常要比函数定义更难阅读、理解以及维护。

宏和函数的最后一个重要的区别是：在调用宏之前 必须定义并将其引入作用域，而函数则可以在任何地方定义和调用。

Rust最常用的宏形式是声明宏(macro_rules!)。声明宏允许我们编写一些类似Rust match表达式的代码。match表达式是控制结构，其接收一个表达式，与表达式的结果进行模式匹配，然后根据模式匹配执行相关代码。宏也将一个值和包含相关代码的模式进行比较；此种情况下，该值是传递给宏的Rust源代码字面值，模式用于和传递给宏的源代码进行比较，同时每个模式的相关代码则用于替换传递给宏的代码。所有这一切都发生于编译时。

我们以vec!宏为例，
```rust
let v: Vec<u32> = vec![1, 2, 3];
```

下面是vec!稍微简化版的定义
```rust
#[macro_export]
macro_rules! vec {
    ( $( $x:expr ),* ) => {
        {
            let mut temp_vec = Vec::new();
            $(
                temp_vec.push($x);
            )*
            temp_vec
        }
    };
}
```

标准库中实际定义的 vec! 包括预分配适当量的内存的代码。这部分为代码优化，为了让示例简化，此处并没有包含在内。

无论何时导入定义了宏的包，#[macro_export]注解说明宏应该是可用的。如果没有该注解，这个宏不能被引入作用域。接着使用macro_rules!和宏名称开始宏定义，且所定义的宏并**不带**感叹号。名字后跟随着的大括号表示宏定义体，在该例中宏名称是vec。

vec!宏的结构和match表达式的结构类似。此处有一个单边模式($($x:expr),* ) ，后跟 => 以及和模式相关的代码块。如果模式匹配，该相关代码块将被执行。假设这是这个宏中唯一的模式，则只有这一种有效匹配，其他任何匹配都是错误的。更复杂的宏会有多个单边模式。

首先，一对括号包含了全部模式。接下来是后跟一对括号$()，其通过替代代码捕获了符合括号内模式的值。括号内则是 $x:expr，其匹配 Rust的任意表达式或给定 $x名字的表达式。

$()之后的逗号说明一个逗号分隔符可以有选择的出现代码之后，这段代码与在 $()中所捕获的代码相匹配。紧随逗号之后的 * 说明该模式匹配零个或多个 * 之前的任何模式。

当以vec![1, 2, 3]; 调用宏时，$x模式与三个表达式 1、2 和 3 进行了三次匹配。

在 $()* 部分中所生成的temp_vec.push()为在匹配到模式中的 $() 每一部分而生成。 $x 由每个与之相匹配的表达式所替换。当以vec![1, 2, 3]; 调用该宏时，替换该宏调用所生成的代码会是下面这样：

```rust
let mut temp_vec = Vec::new();
temp_vec.push(1);
temp_vec.push(2);
temp_vec.push(3);
temp_vec
```

# macros2

```rust
fn main() {
    my_macro!();
}

macro_rules! my_macro {
    () => {
        println!("Check out my macro!");
    };
}
```

这道题的问题所在是作用域。之前已经提过了。

```rust
macro_rules! my_macro {
    () => {
        println!("Check out my macro!");
    };
}

fn main() {
    my_macro!();
}
```

编译通过。

# macros3
```rust
  
// macros3.rs
// Make me compile, without taking the macro out of the module!
// Execute `rustlings hint macros3` for hints :)
mod macros {
    macro_rules! my_macro {
        () => {
            println!("Check out my macro!");
        };
    }
}

fn main() {
    my_macro!();
}

```
还是作用域的问题。这次是封装在mod里。所以我们要在mod前添加上属性#[macro_use]即可。

# macros4
```rust
// macros4.rs
// Make me compile! Execute `rustlings hint macros4` for hints :)

// I AM NOT DONE

macro_rules! my_macro {
    () => {
        println!("Check out my macro!");
    }
    ($val:expr) => {
        println!("Look at this other macro: {}", $val);
    }
}

fn main() {
    my_macro!();
    my_macro!(7777);
}
```

不同的匹配模式后要加分号。

# quiz4
```rust
// quiz4.rs
// This quiz covers the sections:
// - Modules
// - Macros

// Write a macro that passes the quiz! No hints this time, you can do it!

// I AM NOT DONE

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_my_macro_world() {
        assert_eq!(my_macro!("world!"), "Hello world!");
    }

    #[test]
    fn test_my_macro_goodbye() {
        assert_eq!(my_macro!("goodbye!"), "Hello goodbye!");
    }
}

```
这道题就是让我们自己写一个可实现的宏。根据测试代码可知，宏名为my_macro，作用是将输入的字符串拼接到"Hello "后。

所以代码如下：

```rust
macro_rules! my_macro {
    ($val:expr) => {
        "Hello ".to_string() + $val
    };
}

```
编译通过。

更多的宏的类型我们会在实战涉及到。
