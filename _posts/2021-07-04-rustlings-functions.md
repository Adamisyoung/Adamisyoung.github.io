---
layout: post
title:  rustlings-functions
date:   2021-07-04 00:14:31 +0800
categories:
- Rust/rustlings
---

> rustlings系列题解

# function1
```rust
// functions1.rs
// Make me compile! Execute `rustlings hint functions1` for hints :)
fn call_me

fn main() {
    call_me();
}
```

函数遍布于Rust中，用fn声明。Rust代码中的函数和变量名使用snake case规范风格。与C不同，Rust不关心函数定义于何处，只要定义了就行。上述是缺少对函数的定义。我们观察到call_me()没有参数。
```rust
fn call_me() { 
    print!("123!");
}
```

其实函数的完整形式为
```rust
fn call_me() -> () {
    print!("123!");
}
```

如果函数返回一个值，返回类型必须在箭头 -> 之后指定。

这个"()"表示单元类型，即它本身，用于返回值，表示什么都不返回。该函数没返回值，所以将单元值放置在返回类型的位置上。单元值可以省略。

第二行的print!是一个表达式，Rust中有分号的是语句(声明语句和表达式语句)，返回值为()，用来表示没有返回。而没有分号的是表达式，返回值就是自身。

# function2

```rust
// functions2.rs
// Make me compile! Execute `rustlings hint functions2` for hints :)

fn main() {
    call_me(3); 
}

//fn call_me(num:) {
fn call_me(num:i32) {   
    for i in 0..num {
        println!("Ring! Call number {}", i + 1);
    }
}

```

上面的代码，我们就可以发现，我们在main里使用了这个函数，却在后面定义。这在Rust里是允许的。

函数的变量需要标注类型，就和变量一样。上面的函数无返回值，但是有参数，参数类型未指定。

在函数签名中，必须声明每个参数的类型，这是 Rust 设计中一个经过慎重考虑的决定：要求在函数定义中提供类型注解，意味着编译器不需要你在代码的其他地方注明类型来指出你的意图。所以类型不能忽略。

# function3
```rust
// functions3.rs
// Make me compile! Execute `rustlings hint functions3` for hints :)

fn main() {
    //call_me();
    call_me(1);
}
fn call_me(num: u32) {
    for i in 0..num {
        println!("Ring! Call number {}", i + 1);
    }
}

```

上述call_me函数缺少实参。日常中不怎么区分实参和形参。随便填个满足类型需求的数字即可。

# function4
```rust
// functions4.rs
// Make me compile! Execute `rustlings hint functions4` for hints :)

// This store is having a sale where if the price is an even number, you get
// 10 Rustbucks off, but if it's an odd number, it's 3 Rustbucks off.

fn main() {
    let original_price = 51;
    println!("Your sale price is {}", sale_price(original_price));
}
fn sale_price(price: i32) -> i32{
    if is_even(price) {
        price - 10
    } else {
        price - 3
    }
}
fn is_even(num: i32) -> bool {
    num % 2 == 0
}
```

sale_price缺少返回值类型的说明。填上i32即可。这里函数最后的表达式将作为返回值。也可在函数内使用return语句来提前返回值。

例子:
```rust
//一个返回布尔值的函数
fn is_divisible_by(lhs: u32, rhs: u32) -> bool {
    //边界情况，可以提前返回
    if rhs == 0 {
        return false;
    }
    //这是一个表达式，这里可以不用 `return` 关键字
    lhs % rhs == 0
}

```

return甚至也可在循环或if内部使用。

# fuction5
```rust
// functions5.rs
// Make me compile! Execute `rustlings hint functions5` for hints :)

fn main() {
    let answer = square(3);
    println!("The answer is {}", answer);
}
fn square(num: i32) -> i32 {
    num * num;
}

```

square函数有返回值，但是却在num * num后加了分号，由表达式变为了语句。返回值为()。所以只需要将分号去掉即可。

Rust是一门基于表达式的语言
```rust
let x = (let y = 6);
```

上面这条语句会报错，因为let y = 6是一条语句，它没有返回值，无法绑定到x。
