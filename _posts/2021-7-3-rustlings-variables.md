---
layout: post
title:  rustlings-variables
date:   2021-07-03 17:15:31 +0800
categories:
- Rust/rustlings
---

> rustlings系列题解

# variables1
```rust
// variables1.rs
// Make me compile! Execute the command `rustlings hint variables1` if you want a hint :)

// About this `I AM NOT DONE` thing:
// We sometimes encourage you to keep trying things on a given exercise,
// even after you already figured it out. If you got everything working and
// feel ready for the next exercise, remove the `I AM NOT DONE` comment below.

fn main() {
    //x = 5;
    let x = 5;
    println!("x has the value {}", x);
}
```

少了变量绑定。将let加上。

在Rust中允许先变量绑定后初始化，但是编译器会警告。这种做法一般也比较少的用。编译器禁止使用未经初始化的变量，因为这会产生未定义行为，当数据被相同的名称不变地绑定时，它还会冻结。在不可变绑定超出作用域之前，无法修改已冻结的数据。

Rust程序(大部分)由一系列语句构成。Rust有多种语句。最普遍的语句类型有两种：一种是绑定变量，另一种是表达式带上分号：

```rust
let x = 5;  //变量绑定
//表达式
x;
x + 1;
15;
```

代码块也是表达式，所以它们在赋值操作中可以充当右值(r-values)。代码块中的最后一条表达式将赋给左值(l-value)。需要注意的是，如果代码块最后一条表达式结尾处有分号，那么返回值将变成 ()。即单元值。(代码块中的最后一条语句是代码块中实际执行的最后一条语句，而不一定是代码块中最后一行的语句。)

```rust
let x = 5u32;

let y = {
    let x_squared = x * x;
    let x_cube = x_squared * x;

    // 将此表达式赋给 `y`
    x_cube + x_squared + x
};
let z = {
    // 分号结束了这个表达式，于是将 `()` 赋给 `z`
    2 * x;
};

println!("x is {:?}", x);
println!("y is {:?}", y);
println!("z is {:?}", z);

```

输出：
```rust
x is 5
y is 155
z is ()
```

println!调用了一个Rust宏,并不是调用函数。若是调用函数，就是println。宏是后面的内容，这里不多说。相对于print！，println！自带换行，并将文本输出到控制台。编译器会检查格式化的正确性。

println!大体用法如下：
```rust
println!("Hello, world!");
println!("{} days", 31);  //若不加后缀，31就自动成为i32类型
```

输出：
```rust
"Hello, world!"
31 days
```

变量代替字符串有多种写法
```rust
println!("{0}, this is {1}. {1}, this is {0}", "Alice", "Bob"); //位置参数
println!("{subject} {verb} {object}",                           //命名参数
            object = "the lazy dog",
            subject = "the quick brown fox",
            verb = "jump over");
```

输出：
```rust
Alice, this is Bob. Bob, this is Alice
the quick brown fox jumps over the lazy dog
```

其他用法
```rust
println!("{} of {:b} people know binary, the other half don't", 1, 2); //类型指定
println!("{number:>width$}", number=1, width=6);               //按指定宽度来右对齐文本
println!("{number:>0width$}", number=1, width=6);          //可以在数字左边补0。语句输出"000001" 
```

输出：
```rust
1 of 10 people know binary, the other half don't
     1
000001
```


# variables2
```rust
// variables2.rs
// Make me compile! Execute the command `rustlings hint variables2` if you want a hint :)


fn main() {
    //let x;
    let x = 10;
    if x == 10 {
        println!("Ten!");
    } else {
        println!("Not ten!");
    }
}

```

Rust中不允许只声明不赋值，Rust编译器会对代码作基本的静态分支流程分析，确保变量在使用之前一定被初始化。x没有绑定任何值，这样的代码会引起很多内存不安全的问题，比如计算结果非预期、程序崩溃，所以Rust编译器必须报错。

此处未对变量进行显式的类型声明，则整型默认为i32类型。若是浮点数则默认为i64类型。

后续会专门讲解if分支。

# variables3
```rust
// variables3.rs
// Make me compile! Execute the command `rustlings hint variables3` if you want a hint :)

fn main() {
    let mut x = 3;
    println!("Number {}", x);
    x = 5; // don't change this line
    println!("Number {}", x);
}
```

let声明后的变量默认是不可变的。Rust是静态强类型语言，这就决定了在运行程序前Rust便需要知道所有变量的类型，而且不会自动将一个变量类型转为另一个类型。但是Rust可以根据上下文推断类型。
```rust
let mut inferred_type = 12; //根据下一行的赋值推断为i64类型
inferred_type = 4294967296i64;   
```

扯远了。。我们想创建可修改的变量就加mut即可。mut修饰的变量具有可变性，mut修饰对应内存可以修改。所以上面x前加mut。

虽然mut的变量可变，但是类型不可变。下面的实例会报错。

```rust
let mut mutable = 12; // Mutable `i32`
mutable = 21; //不报错
//下面这句会报错，因为变量的类型并不能改变。
mutable = true;
```

但是我们可以通过变量遮蔽来修改类型，都是后话。
```rust
let mutable = true;
```

# variables4
```rust
// variables4.rs
// Make me compile! Execute the command `rustlings hint variables4` if you want a hint :)

fn main() {
    let x: i32;
    println!("Number {}", x);
}
```

这个也是典型的光声明没初始化。不过通过这里我们可以提一下Rust类型的声明。

数字可以通过后缀或默认方式来声明类型，如下：

```rust
let logical: bool = true;
let a_float: f64 = 1.0;  //常规说明
let an_integer = 5i32; //后缀说明
//否则会按默认方式决定类型。
let default_float = 3.0; //f64
let default_integer = 7;   //i32
```

Rust提供多种原生类型，包括
标量类型
* 有符号整型(signed integers)：i8、i16、i32、i64 和 isize(指针宽度)
* 无符号整型(unsigned integers)：u8、u16、u32、u64 和 usize(指针宽度)
* 浮点类型(floating point)： f32、f64
* char(字符)：单个 Unicode 字符，如 'a'，'α' 和 '∞'(每个都是 4 字节)
* bool(布尔型)：只能是 true 或 false
* 单元类型(unit type)：()。其唯一可能的值就是()这个空元组
  
复合类型
* 数组(array)：如 [1, 2, 3]
* 元组(tuple)：如 (1, true)

之后会慢慢接触到。

# variables5
```rust
// variables5.rs
// Make me compile! Execute the command `rustlings hint variables5` if you want a hint :)


fn main() {
    let number = "T-H-R-E-E"; // don't change this line
    println!("Spell a Number : {}", number);
    number = 3;
    println!("Number plus two is : {}", number + 2);
}

```

这里即是考察了前文所述的变量遮蔽。我们无法对number进行修改，就算加mux，我们也不能将number从String类型变为i32类型。所以这里采用变量遮蔽的方式。

```rust
fn main() {
    let number = "T-H-R-E-E";
    println!("Spell a Number : {}", number);
    let number :i32 = 5;
    println!("Number plus two is : {}", number + 2);
}
```

# variables6
```rust
// variables6.rs
// Make me compile! Execute the command `rustlings hint variables6` if you want a hint :)

const NUMBER = 3;
fn main() {
    println!("Number {}", NUMBER);
}

```

这里简单考察了const。变量可以给出类型说明，有常规说明和后缀说明，在没有给出说明的情况就按默认方式决定类型(let)。

但const必须指定变量类型，不能省略。而且const命名方式倾向于全部大写，否则编译器会给出警告。

Rust有两种常量，可以在任意作用域声明，包括全局作用域。它们都需要显式的类型声明：
* const：不可改变的值
* static：具有'static生命周期的，可以是可变的变量(要加mut)

有个特例就是string字面量。它可以不经改动就被赋给一个static变量，因为它的类型标记：&'static str就包含了所要求的生命周期'static。其他的引用类型都必须特地声明，使之拥有'static 生命周期。这两种引用类型的差异似乎也无关紧要，因为无论如何，static 变量都得显式地声明。

示例：
```rust
//全局变量是在在所有其他作用域之外声明的。
static LANGUAGE: &'static str = "Rust";
const  THRESHOLD: i32 = 10;

fn is_big(n: i32) -> bool {
    // 在一般函数中访问常量
    n > THRESHOLD
}

fn main() {
    let n = 16;
    // 在 main 函数（主函数）中访问常量
    println!("This is {}", LANGUAGE);
    println!("The threshold is {}", THRESHOLD);
    println!("{} is {}", n, if is_big(n) { "big" } else { "small" });
}
```
