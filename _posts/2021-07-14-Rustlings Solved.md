---
title: Rustlings题解(一)
copyright: true
mathjax: true
categories:
- Rust/Rustlings
tags: 
---

> Rustlings练习题的题解，可能不太完整。Rustlings最初由Carol Nichols编写，其中包含一些练习，旨在帮助Rust入门的开发人员习惯于阅读和编写Rust代码。

闲话少说，直接开始。

# variables

## variables1

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


## variables2

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

## variables3

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

## variables4

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

## variables5

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

## variables6

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

# functions

## function1

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

## function2

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

## function3

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

## function4

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

## function5

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

# if

## if1

```rust
// if1.rs

pub fn bigger(a: i32, b: i32) -> i32 {
    // Complete this function to return the bigger number!
    // Do not use:
    // - another function call
    // - additional variables
    // Execute `rustlings hint if1` for hints
}

// Don't mind this for now :)
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn ten_is_bigger_than_eight() {
        assert_eq!(10, bigger(10, 8));
    }

    #[test]
    fn fortytwo_is_bigger_than_thirtytwo() {
        assert_eq!(42, bigger(32, 42));
    }
}
```

这里大家能注意到fn前多了一个pub。Rust中有两种简单的访问权：公共(public)和私有(private)。这个pub就代表公共。后续会详细讲解。

还有下面的mod。Rust中的组织单位是模块(Module)，#[cfg(test)]是模块的对应注解。这些先略过，我们主要关注目前的具体问题。

Rust的if表达式和C/C++差不多。只是多注意返回值就可以了。assert_eq!函数将bigger的返回值和10进行比较，所以if花括号里的表达式也是i32。

因为if是个表达式，它有返回值，所以可以放在let的右侧，绑定在变量上。实例如下：

```rust
fn main() {
    let condition = true;
    let number = if condition {
        5
    } else {
        6
    };

    println!("The value of number is: {}", number);
}
```

```rust
pub fn bigger(a: i32, b: i32) -> i32 {
    // Complete this function to return the bigger number!
    // Do not use:
    // - another function call
    // - additional variables
    // Execute `rustlings hint if1` for hints
    if a > b { 
        a 
    }
    else{ 
        b 
    }

}
```  

需要注意的是if-else分支必须完备，如果单只有一个if，其他分支未覆盖全的话，编译器会认定有不走if的情况，从而报错。这个在C中有可能是警告。因为在C中如果返回值是整数或指针类型，就会默认返回0并抛出警告，若不能强制返回0的话才会报错。

Rust编译器 yyds

## if2

```rust
// if2.rs

// Step 1: Make me compile!
// Step 2: Get the bar_for_fuzz and default_to_baz tests passing!
// Execute the command `rustlings hint if2` if you want a hint :)

pub fn fizz_if_foo(fizzish: &str) -> &str {
    if fizzish == "fizz" {
        "foo"
    } else {
        1
    }
}
// No test changes needed!
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn foo_for_fizz() {
        assert_eq!(fizz_if_foo("fizz"), "foo")
    }

    #[test]
    fn bar_for_fuzz() {
        assert_eq!(fizz_if_foo("fuzz"), "bar")
    }

    #[test]
    fn default_to_baz() {
        assert_eq!(fizz_if_foo("literally anything"), "baz")
    }
}

```

if2相较于if1就是多了分支，无需赘述。

```rust
pub fn fizz_if_foo(fizzish: &str) -> &str {
    if fizzish == "fizz" {
        "foo"
    } else if fizzish == "fuzz" {
        "bar"
    } else if fizzish == "literally anything" {
        "baz"
    } else {
        "1"
    }  
}
```

# Quiz1

```rust
// quiz1.rs
// This is a quiz for the following sections:
// - Variables
// - Functions

// Mary is buying apples. One apple usually costs 2 Rustbucks, but if you buy
// more than 40 at once, each apple only costs 1! Write a function that calculates
// the price of an order of apples given the order amount. No hints this time!

// Put your function here!
// fn ..... {

// Don't modify this function!
#[test]
fn verify_test() {
    let price1 = calculate_apple_price(35);
    let price2 = calculate_apple_price(40);
    let price3 = calculate_apple_price(65);

    assert_eq!(70, price1);
    assert_eq!(80, price2);
    assert_eq!(65, price3);
}
```

这个程序逻辑很简单，就是对购买数量进行一个if判断，返回值不同而已。

```rust
pub fn calculate_apple_price(prices:i32) -> i32 {
    if prices < 40 { 2 * prices }
    else { prices }
}
```


# move_semantics

## move_semantics1

```rust
// move_semantics1.rs
// Make me compile! Execute `rustlings hint move_semantics1` for hints :)

fn main() {
    let vec0 = Vec::new();

    //let v = vec![1, 2, 3];
    let mut v = vec![1, 2, 3];
    println!("{} has length {} content `{:?}`", "vec1", vec1.len(), vec1);

    vec1.push(88);

    println!("{} has length {} content `{:?}`", "vec1", vec1.len(), vec1);
}

fn fill_vec(vec: Vec<i32>) -> Vec<i32> {
    let mut vec = vec;

    vec.push(22);
    vec.push(44);
    vec.push(66);
    vec
}
```

Rust标准库中包含一系列被称为集合(collections)的非常有用的数据结构。大部分其他数据类型都代表一个特定的值，不过集合可以包含多个值。不同于内建的数组和元组类型，这些集合指向的数据是储存在堆上的，这意味着数据的数量不必在编译时就已知，并且还可以随着程序的运行增长或缩小。每种集合都有着不同功能和成本，而根据当前情况选择合适的集合，这是一项始终成长的技能。

有三个被广泛使用的集合：
* vector：允许我们一个挨着一个地储存一系列数量可变的值；
* string：字符的集合；
* 哈希map：允许我们将值与一个特定的键值(key)相关联。这是一个叫做map的更通用的数据结构的特定实现

创建一个Vector：

```rust
let v: Vec<i32> = Vec::new();
```

加了类型注解却未加入任何值，现在Rust并不知道我们储存什么元素。这就是泛型的实现。其实我们很少需要类型注解。所以为了方便，Rust提供了vec!宏，这个宏会根据我们提供的值来创建一个新的Vec。

```rust
let v = vec![1, 2, 3];
```

上题中，创建了一个未知类型的Vec，但是不报错。因为在下一行，该Vec就作为fill_vec函数的实参传入，而该函数形参类型是i32，返回的是Vec<i32>。所以编译器自动推断出类型。上面代码的问题就在改变Vec上。缺少一个mut。改变Vec也是需要mut的。更新Vec就是push()。


## move_semantics2

```rust
// move_semantics2.rs
// Make me compile without changing line 13!
// Execute `rustlings hint move_semantics2` for hints :)

fn main() {
    let vec0 = Vec::new();

    let mut vec1 = fill_vec(vec0);

    // Do not change the following line!
    println!("{} has length {} content `{:?}`", "vec0", vec0.len(), vec0);

    vec1.push(88);

    println!("{} has length {} content `{:?}`", "vec1", vec1.len(), vec1);
}

fn fill_vec(vec: Vec<i32>) -> Vec<i32> {
    let mut vec = vec;

    vec.push(22);
    vec.push(44);
    vec.push(66);

    vec
}

```

这里涉及到借用权变更的问题。一旦程序获取了一个有效的引用，借用检查器将会执行所有权和借用规则来确保vector内容的这个引用和任何其他引用保持有效。

所有权系统是Rust最为与众不同的特性，它让Rust无需垃圾回收(garbage collector)即可保障内存安全。因此，理解Rust中所有权如何工作是十分重要的。

所有运行的程序都必须管理其使用计算机内存的方式。一些语言中具有垃圾回收机制，在程序运行时不断地寻找不再使用的内存；在另一些语言中，程序员必须亲自分配和释放内存。Rust则选择了第三种方式：通过所有权系统管理内存，编译器在编译时会根据一系列的规则进行检查。在运行时，所有权系统的任何功能都不会减慢程序。

栈和堆不需要多说，前者中所有数据都必须占用已知且固定的大小。在编译时大小未知或大小可能变化的数据，要改为存储在堆上。当向堆放入数据时，你要请求一定大小的空间。操作系统在堆的某处找到一块足够大的空位，把它标记为已使用，并返回一个表示该位置地址的指针。将数据推入栈中并不被认为是分配。因为指针的大小是已知并且固定的，你可以将指针存储在栈上，不过当需要实际数据时，必须访问指针。访问堆上的数据比访问栈上的数据慢，因为必须通过指针来访问。跟踪哪部分代码正在使用堆上的哪些数据，最大限度的减少堆上的重复数据的数量，以及清理堆上不再使用的数据确保不会耗尽空间，这些问题正是所有权系统要处理的。

从根本上来说，所有权的存在就是为了管理堆数据。

所有权三条规则：
* Rust中的每一个值都有一个被称为其所有者(owner)的变量。
* 值有且只有一个所有者。
* 当所有者(变量)离开作用域，这个值将被丢弃。
作用域在前面已经提过了。

```rust
{                      //s在这里无效,它尚未声明
    let s = "hello";   //从此处起，s是有效的

    //使用 s
}                      //此作用域已结束，s不再有效
```

当s进入作用域，它是有效的，一直持续到离开作用域为止。到这里为止，和其他编程语言都没什么区别。接下来我们借用一个能存储在堆上的数据结构来摸索一下。

我们已经见过字符串字面值，字符串值被硬编码进程序里，这是很方便的。不过他们并不适合使用文本的每一种场景。原因之一就是他们是不可变的。另一个原因是并不是所有字符串的值都能在编写代码时就知道：例如，要是想获取用户输入并存储，为此，Rust有第二个字符串类型，即String。这个类型被分配到堆上，所以能够存储在编译时未知大小的文本。可以使用from函数基于字符串字面值来创建String，如下：

```rust
let mut s = String::from("hello");
s.push_str(", world!"); //push_str()在字符串后追加字面值
println!("{}", s); //将打印 `hello, world!`
```

上面的例子中，我们可以发现，String是可变的。这和硬编码的字面值形成鲜明对比。就字符串字面值来说，我们在编译时就知道其内容，所以文本被直接硬编码进最终的可执行文件中。这使得字符串字面值快速且高效。不过这些特性都只得益于字符串字面值的不可变性。不幸的是，我们不能为了每一个在编译时大小未知的文本而将一块内存放入二进制文件中，并且它的大小还可能随着程序运行而改变。

而对于String类型，为了支持一个可变，可增长的文本片段，需要在堆上分配一块在编译时未知大小的内存来存放内容。这意味着：
* 必须在运行时向操作系统请求内存
* 需要一个当我们处理完String时将内存返回给操作系统的方法
  
第一部分即当调用String::from时，它的实现请求其所需的内存。这在编程语言中是非常通用的。然而，第二部分实现起来就各有区别了。在有垃圾回收(GC)的语言中，GC记录并清楚不再使用的内存，而我们并不需要关心它。没有GC的话，识别出不再使用的内存并调用代码显式释放就是我们的责任了，跟请求内存的时候一样。从历史的角度上说正确处理内存回收曾经是一个困难的编程问题。如果忘记回收了会浪费内存。如果过早回收了，将会出现无效变量。如果重复回收，这也是个bug。我们需要精确的为一个allocate配对一个free。

Rust采取了一个不同的策略：内存在拥有它的变量离开作用域后就被自动释放。上面的例子中，当变量离开作用域，Rust为我们调用一个特殊的函数。这个函数叫做drop，在drop里String的作者可以放置释放内存的代码。Rust在结尾的 } 处自动调用了drop。这种机制在C++中，生命周期结束释放资源的模式称为RAII。这个模式对编写 Rust 代码的方式有着深远的影响。

变量和数据有多种交互方式。

```rust
let x = 5;
let y = x;
```
这里是将5绑定到x；接着生成一个值x的拷贝并绑定到y。因为整数是有已知固定大小的简单值，所以这两个5被放入了栈中。而下面的这个版本
```rust
let s1 = String::from("hello");
let s2 = s1;
```

这看起来与上面的代码非常类似，但事实上并不完全是这样。String是由三部分组成：一个指向存放字符串内容内存的指针，一个长度，和一个容量。这一组数据是存储在栈上。右侧则是堆上存放内容的内存部分。长度表示String的内容当前使用了多少字节的内存。容量是String从操作系统总共获取了多少字节的内存。长度与容量的区别是很重要的，不过在当前上下文中并不重要，所以现在可以忽略容量。当我们将s1赋值给s2，String的数据被复制了，这意味着我们从栈上拷贝了它的指针、长度和容量。我们并没有复制指针指向的堆上数据。则这就意味着**两个变量同时指向堆上同一处内存**。之前我们提到过当变量离开作用域后，Rust自动调用drop函数并清理变量的堆内存。则当s2和s1离开作用域，他们都会尝试释放相同的内存(double free)。

所以，为了确保内存安全，与其尝试拷贝被分配的内存，Rust则认为s1不再有效，因此Rust不需要在s1离开作用域后清理任何东西。

```rust
let s1 = String::from("hello");
let s2 = s1;

println!("{}, world!", s1);
```

上面这段代码会报错，因为s1已经是无效的引用了。这种行为被称之为**Move**。浅拷贝的一种。这里隐含了一个设计选择：Rust永远也不会自动创建数据的**深拷贝**，任何自动的复制都可以被认为运行时性能影响最小。

让我们回到上面这道题。

vec0传入函数后，浅拷贝到了函数内部的Vec中，则vec0即失效。当我们再调用vec0.len()的时候就会报错。此时vec1是新的owner，这中间发生了所有权的转移。

所以解决方法就是由浅拷贝改为**深拷贝**如果我们确实需要深度复制堆上的数据，而不仅仅是栈上的数据，可以使用一个叫做 clone 的通用函数。这中间也有一些不太寻常的事情。

让我们回到上面的例子中

```rust
let x = 5;
let y = x;
println!("x = {}, y = {}", x, y);
```

这段代码似乎没有调用clone，但x和y都有效。原因是像整型这样的在编译时已知大小的类型被整个存储在栈上，所以拷贝其实际的值是快速的。这意味着没有理由在创建变量y后使x无效。换句话说，这里没有深浅拷贝的区别，所以这里调用clone并不会与通常的浅拷贝有什么不同。Rust有一个叫做Copy trait的特殊注解，可以用在类似整型这样的存储在栈上的类型上：如果一个类型拥有Copy trait，则一个旧的变量在将其赋值给其他变量后仍然可用。但如果想在变量离开作用域的时候使用还是会出错的。任何简单标量值的组合可以是Copy的，即不需要分配内存或某种形式资源的类型是Copy的。具体类别如下：
* 所有整数类型，比如u32
* 布尔类型，bool，它的值是true和false
* 所有浮点数类型，比如f64
* 字符类型，char
* 元组，当且仅当其包含的类型也都是Copy的时候。比如(i32, i32)是Copy的，但(i32, String)就不是

变量的所有权总是遵循相同的模式：将值赋给另一个变量时移动它。当持有堆中数据值的变量离开作用域时，其值将通过drop被清理掉，除非数据被移动为另一个变量所有。在每一个函数中都获取所有权并接着返回所有权有些啰嗦。如果我们想要函数使用一个值但不获取所有权该怎么办呢？如果我们还要接着使用它的话，每次都传进去再返回来就有点烦人了，除此之外，我们也可能想返回函数体中产生的一些数据。所以Rust提供了**引用**。这是后话。

修正过的move_semantics

```rust
fn main() {
    let vec0 = Vec::new();
    let mut vec1 = fill_vec(vec0.clone());
    //vec1是新owner
    //让move变为clone(深拷贝)
    // Do not change the following line!
    println!("{} has length {} content `{:?}`", "vec0", vec0.len(), vec0);
    vec1.push(88);

    println!("{} has length {} content `{:?}`", "vec1", vec1.len(), vec1);
}

fn fill_vec(vec: Vec<i32>) -> Vec<i32> {
    let mut vec = vec;
    //把一个变量赋值给另一个变量会把所有权(ownership)转移给受让者
    vec.push(22);
    vec.push(44);
    vec.push(66);
    vec
}

```
## move_semantics3

```rust
// move_semantics3.rs
// Make me compile without adding new lines-- just changing existing lines!
// (no lines with multiple semicolons necessary!)
// Execute `rustlings hint move_semantics3` for hints :)


fn main() {
    let vec0 = Vec::new();

    let mut vec1 = fill_vec(vec0);

    println!("{} has length {} content `{:?}`", "vec1", vec1.len(), vec1);

    vec1.push(88);

    println!("{} has length {} content `{:?}`", "vec1", vec1.len(), vec1);
}

//fn fill_vec(vec: Vec<i32>) -> Vec<i32> {
fn fill_vec(mut vec: Vec<i32>) -> Vec<i32> {    
    vec.push(22);
    vec.push(44);
    vec.push(66);

    vec
}
```

这里的问题是，在调用函数后，我们对函数的返回值进行了修改(push(88))，函数参数并未包含可变mut，所以会报错。为了添加变量可变性，就在vec前加上mut即可。如果放在冒号后面，mut修饰的就是类型，仅仅是用在可变引用&mut T和裸指针*mut T上。这个mut无法影响传入变量的可变性。

## move_semantics4

```rust
// move_semantics4.rs
// Refactor this code so that instead of having `vec0` and creating the vector
// in `fn main`, we create it within `fn fill_vec` and transfer the
// freshly created vector from fill_vec to its caller.
// Execute `rustlings hint move_semantics4` for hints!

// I AM NOT DONE

fn main() {
    let vec0 = Vec::new();

    let mut vec1 = fill_vec(vec0);

    println!("{} has length {} content `{:?}`", "vec1", vec1.len(), vec1);

    vec1.push(88);

    println!("{} has length {} content `{:?}`", "vec1", vec1.len(), vec1);
}

// `fill_vec()` no longer takes `vec: Vec<i32>` as argument
fn fill_vec() -> Vec<i32> {
    let mut vec = vec;

    vec.push(22);
    vec.push(44);
    vec.push(66);

    vec
}

```

大部分时候当涉及到泛型时，编译器可以自动推断出泛型参数，但是这里因为fill_vec函数不再传参，所以第一行类型就无法推断出来，编译器会报错。这道题的意思就是构造Vec的工作放在函数内进行。答案如下：

```rust
fn main() {
    let mut vec1 = fill_vec();

    println!("{} has length {} content `{:?}`", "vec1", vec1.len(), vec1);

    vec1.push(88);

    println!("{} has length {} content `{:?}`", "vec1", vec1.len(), vec1);
}

// `fill_vec()` no longer takes `vec: Vec<i32>` as argument
fn fill_vec() -> Vec<i32> {
    let mut vec = Vec::new();
    vec.push(22);
    vec.push(44);
    vec.push(66);

    vec
}

```


第五题后续添加....


# primitive_types

## primitive_types1

```rust
// primitive_types1.rs
// Fill in the rest of the line that has code missing!
// No hints, there's no tricks, just get used to typing these :)


fn main() {
    // Booleans (`bool`)

    let is_morning = true;
    if is_morning {
        println!("Good morning!");
    }

    let is_evening = true;// Finish the rest of this line like the example! Or make it be false!
    if is_evening {
        println!("Good evening!");
    }
}
```

简单的布尔型。

## primitive_types2

```rust
// primitive_types2.rs
// Fill in the rest of the line that has code missing!
// No hints, there's no tricks, just get used to typing these :)
// I AM NOT DONE

fn main() {
    // Characters (`char`)
    let my_first_initial = 'C';
    if my_first_initial.is_alphabetic() {
        println!("Alphabetical!");
    } else if my_first_initial.is_numeric() {
        println!("Numerical!");
    } else {
        println!("Neither alphabetic nor numeric!");
    }
    let your_character = 'N'; // Finish this line like the example! What's your favorite character?
    // Try a letter, try a number, try a special character, try a character
    // from a different language than your own, try an emoji!
    if your_character.is_alphabetic() {
        println!("Alphabetical!");
    } else if your_character.is_numeric() {
        println!("Numerical!");
    } else {
        println!("Neither alphabetic nor numeric!");
    }
}

```

自定义个your_character。

## primitive_types3

```rust
// primitive_types3.rs
// Create an array with at least 100 elements in it where the ??? is.
// Execute `rustlings hint primitive_types3` for hints!

// I AM NOT DONE

fn main() {
    let a = ???

    if a.len() >= 100 {
        println!("Wow, that's a big array!");
    } else {
        println!("Meh, I eat arrays like that for breakfast.");
    }
}
```

这里涉及到的概念是数组的定义。Rust中的数组是固定长度的，一整块分配在栈上的内存。一旦声明，它们的长度不能增长或缩小。平时访问数组是通过索引访问，这个和其他语言差不多。

数组标记为[T; size] T为元素类型，size表示数组的大小。

## primitive_types4

```rust
// primitive_types4.rs
// Get a slice out of Array a where the ??? is so that the test passes.
// Execute `rustlings hint primitive_types4` for hints!!

#[test]
fn slice_out_of_array() {
    let a = [1, 2, 3, 4, 5];

    let nice_slice = ???

    assert_eq!([2, 3, 4], nice_slice)
}

```

这里涉及到一个没有所有权的类型：Slice类型。Slice允许你引用集合中一段连续的元素序列，而不用引用整个集合。切片的类型和数组类似，但其大小在编译时是不确定的。相反，切片是一个双字对象。第一个字是一个指向数据的指针，第二个字是切片的长度。这个"字"的宽度和usize相同，由处理器架构决定，比如在x86-64平台上就是64位。Slice 的类型标记为 &[T]。

Slice有很多种类型，先来说一说**字符串Slice**。

字符串Slice是String中一部分值的**引用**。引用的概念极其重要，后面也会专门讲到。它看起来像这样：

```rust
let s = String::from("hello world");
let hello = &s[0..5];
let world = &s[6..11];
```

可以使用一个由中括号中的[starting_index..ending_index]指定的range创建一个Slice，其中starting_index是Slice的第一个位置，ending_index则是Slice最后一个位置的后一个值。在其内部，Slice的数据结构存储了slice的开始位置和长度，长度对应于ending_index减去starting_index的值。所以对于let world = &s[6..11]的情况，world将是一个包含指向s第7个字节(从1开始)的指针和长度值5的slice。

对于Rust的“..”语法，如果想要从第一个索引(0)开始，可以不写两个点号之前的值。

```rust
let s = String::from("hello");

let slice = &s[0..2];
let slice = &s[..2];
```

上面两个语句是完全相同的。依此类推，如果Slice包含String的最后一个字节，也可以舍弃尾部的数字。这里不再过多赘述。

之前我们说过字符串字面值被储存在二进制文件中，现在我们可以重新理解一下字符串字面值。

```rust
let s = "Hello, world!";
```

这里s的类型是&str：它是一个指向二进制程序特定位置的Slice。这也就是为什么字符串字面值是不可变的；&str是一个不可变引用。

在知道了能够获取字面值和String的Slice后，我们可以把他们作为参数传入函数。如

```rust
fn first_word(s: &str) -> &str {
```

如果有一个字符串Slice，则可以直接传递它。如果有一个String，则可以传递整个String 的Slice。

下面是数组和Slice的相关例子，字符串Slice的类型声明写作&str，不过下面这个例子用的是更通用的Slice类型，即&[i32]。它跟字符串Slice的工作方式一样，通过存储第一个集合元素的引用和一个集合总长度。我们也可以对其他所有集合使用这类Slice

```rust
use std::mem;  //验证栈上内存引入的模块
//此函数借用一个slice
fn analyze_slice(slice: &[i32]) {
    println!("first element of the slice: {}", slice[0]);
    println!("the slice has {} elements", slice.len());
}
fn main() {
    //定长数组(类型标记是多余的)
    let xs: [i32; 5] = [1, 2, 3, 4, 5];
    //所有元素可以初始化成相同的值
    let ys: [i32; 500] = [0; 500];
    //下标从 0 开始
    println!("first element of the array: {}", xs[0]);
    println!("second element of the array: {}", xs[1]);
    //`len` 返回数组的大小
    println!("array size: {}", xs.len());
    //数组是在栈中分配的
    println!("array occupies {} bytes", mem::size_of_val(&xs));
    //数组可以自动被借用成为 slice
    println!("borrow the whole array as a slice");
    analyze_slice(&xs);
    //slice可以指向数组的一部分
    println!("borrow a section of the array as a slice");
    analyze_slice(&ys[1 .. 4]);
    //越界的下标会引发致命错误（panic）
    //println!("{}", xs[5]);
```

输出：

```rust
first element of the array: 1
second element of the array: 2
array size: 5
array occupies 20 bytes
borrow the whole array as a slice
first element of the slice: 1
the slice has 5 elements
borrow a section of the array as a slice
first element of the slice: 0
the slice has 3 elements
```

所以，将？？？那里替换为

```rust
let nice_slice = &a[1..4];
```

## primitive_types5

```rust
// primitive_types5.rs
// Destructure the `cat` tuple so that the println will work.
// Execute `rustlings hint primitive_types5` for hints!

// I AM NOT DONE
fn main() {
    let cat = ("Furry McFurson", 3.5);
    let /* your pattern here */ = cat;

    println!("{} is {} years old.", name, age);
}

```

这里涉及到了元组以及元组的解构。

前面在第一节简单提过元组属于复合类型，它是一个将多个其他类型的值组合进一个复合类型的主要方式。元组长度固定：一旦声明，其长度不会增大或缩小。所以也不会涉及到所有权的问题。我们使用包含在圆括号中的逗号分隔的值列表来创建一个元组。元组中的每一个位置都有一个类型，而且这些不同值的类型也不必是相同的。cat内部包含String字面值和f64两种类型。

为了从元组中获取单个值，我们可以使用模式匹配(pattern matching)来解构(destructure)元组值。除了使用模式匹配解构外，也可以使用点号(.)后跟值的索引来直接访问。

下面是元组的一些输入输出的例子，方便理解。

```rust
//元组可以充当函数的参数和返回值
fn reverse(pair: (i32, bool)) -> (bool, i32) {
    //可以使用let把一个元组的成员绑定到一些变量
    let (integer, boolean) = pair;
    (boolean, integer)
}

#[derive(Debug)]
struct Matrix(f32, f32, f32, f32);
fn main() {
    // 包含各种不同类型的元组
    let long_tuple = (1u8, 2u16, 3u32, 4u64,
                      -1i8, -2i16, -3i32, -4i64,
                      0.1f32, 0.2f64,
                      'a', true);

    //可以通过元组的下标来访问具体的值
    println!("long tuple first value: {}", long_tuple.0);
    println!("long tuple second value: {}", long_tuple.1);
    //元组也可以充当元组的元素
    let tuple_of_tuples = ((1u8, 2u16, 2u32), (4u64, -1i8), -2i16);
    //元组可以打印
    println!("tuple of tuples: {:?}", tuple_of_tuples);
    // 但很长的元组无法打印
    // let too_long_tuple = (1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13);
    // println!("too long tuple: {:?}", too_long_tuple);
    // 试一试 ^ 取消上面两行的注释，阅读编译器给出的错误信息。
    let pair = (1, true);
    println!("pair is {:?}", pair);
    println!("the reversed pair is {:?}", reverse(pair));
    // 创建单元素元组需要一个额外的逗号，这是为了和被括号包含的字面量作区分。
    println!("one element tuple: {:?}", (5u32,));
    println!("just an integer: {:?}", (5u32));
    // 元组可以被解构（deconstruct），从而将值绑定给变量
    let tuple = (1, "hello", 4.5, true);
    let (a, b, c, d) = tuple;
    println!("{:?}, {:?}, {:?}, {:?}", a, b, c, d);
    let matrix = Matrix(1.1, 1.2, 2.1, 2.2);
    println!("{:?}", matrix)
}

```

输出：

```rust
long tuple first value: 1
long tuple second value: 2
tuple of tuples: ((1, 2, 2), (4, -1), -2)
pair is (1, true)
the reversed pair is (true, 1)
one element tuple: (5,)
just an integer: 5
1, "hello", 4.5, true
Matrix(1.1, 1.2, 2.1, 2.2)
```

回到题目，我们添加下列语句即可。

```rust
let (name, age) = cat;
```

即可完成解构。

在例子中，我添加了Debug注解，后续会讲到。

## primitive_types6

```rust

// primitive_types6.rs
// Use a tuple index to access the second element of `numbers`.
// You can put the expression for the second element where ??? is so that the test passes.
// Execute `rustlings hint primitive_types6` for hints!

#[test]
fn indexing_tuple() {
    let numbers = (1, 2, 3);
    // Replace below ??? with the tuple indexing syntax.
    let second = ???;

    assert_eq!(2, second,
        "This is not the 2nd number in the tuple!")
}
```

这里即考察了元组的第二种访问方式：通过索引值访问。

```rust
let second = numbers.1; //Index
```

补充一下Rust的四则运算。

其中整数1、浮点数1.2、字符'a'、字符串"abc"、布尔值true和单元类型()可以用数字、文字或符号之类的字面量(literal)来表示。另外，通过加前缀0x、0o、0b，数字可以用十六进制、八进制或二进制记法表示。为了改善可读性，可以在数值字面量中插入下划线，比如：1_000等同于1000，0.000_001等同于0.000001。

```rust
//整数相加
println!("1 + 2 = {}", 1u32 + 2);
//整数相减
println!("1 - 2 = {}", 1i32 - 2);
//短路求值的布尔逻辑
println!("true AND false is {}", true && false);
println!("true OR false is {}", true || false);
println!("NOT true is {}", !true);
//位运算
println!("0011 AND 0101 is {:04b}", 0b0011u32 & 0b0101);
println!("0011 OR 0101 is {:04b}", 0b0011u32 | 0b0101);
println!("0011 XOR 0101 is {:04b}", 0b0011u32 ^ 0b0101);
println!("1 << 5 is {}", 1u32 << 5);
println!("0x80 >> 2 is 0x{:x}", 0x80u32 >> 2);
//使用下划线改善数字的可读性
println!("One million is written as {}", 1_000_000u32);
```

输出：
```rust
1 + 2 = 3
1 - 2 = -1
true AND false is false
true OR false is true
NOT true is false
0011 AND 0101 is 0001
0011 OR 0101 is 0111
0011 XOR 0101 is 0110
1 << 5 is 32
0x80 >> 2 is 0x20
One million is written as 1000000
```

Rust还提供了多种机制，用于改变或定义原生类型和用户定义类型。Rust不提供原生类型之间的隐式类型转换，但可以使用as关键字进行显式类型转换。整型之间的转换大体遵循C语言的惯例，但是C会产生未定义行为的情形，而在Rust中所有整型转换都是定义良好的。

Rust通过type进行重命名。下面是进行类型转换，类型推断和重命名的样例，有些后续还会涉及。

```rust
// 不显示类型转换产生的溢出警告。
#![allow(overflowing_literals)]

// `NanoSecond` 是 `u64` 的新名字。
type NanoSecond = u64;
type Inch = u64;

fn main() {
    let decimal = 65.4321_f32;

    //错误！不提供隐式转换
    //let integer: u8 = decimal;

    //可以显式转换
    let integer = decimal as u8;
    let character = integer as char;
    println!("Casting: {} -> {} -> {}", decimal, integer, character);

    //当把任何类型转换为无符号类型T时，会不断加上或减去(std::T::MAX + 1)
    //直到值位于新类型T的范围内。

    //1000 已经在 u16 的范围内
    println!("1000 as a u16 is: {}", 1000 as u16);

    //1000 - 256 - 256 - 256 = 232
    // 事实上的处理方式是：从最低有效位（LSB，least significant bits）开始保留
    // 8 位，然后剩余位置，直到最高有效位（MSB，most significant bit）都被抛弃。
    // 译注：MSB 就是二进制的最高位，LSB 就是二进制的最低位，按日常书写习惯就是
    // 最左边一位和最右边一位。
    println!("1000 as a u8 is : {}", 1000 as u8);
    // -1 + 256 = 255
    println!("  -1 as a u8 is : {}", (-1i8) as u8);

    // 对正数，这就和取模一样。
    println!("1000 mod 256 is : {}", 1000 % 256);

    // 当转换到有符号类型时，（位操作的）结果就和 “先转换到对应的无符号类型，
    // 如果 MSB 是 1，则该值为负” 是一样的。

    // 当然如果数值已经在目标类型的范围内，就直接把它放进去。
    println!(" 128 as a i16 is: {}", 128 as i16);
    // 128 转成 u8 还是 128，但转到 i8 相当于给 128 取八位的二进制补码，其值是：
    println!(" 128 as a i8 is : {}", 128 as i8);

    // 重复之前的例子
    // 1000 as u8 -> 232
    println!("1000 as a u8 is : {}", 1000 as u8);
    // 232 的二进制补码是 -24
    println!(" 232 as a i8 is : {}", 232 as i8);


    //对数值字面量，只要把类型作为后缀加上去，就完成了类型说明。
    //比如指定字面量 42 的类型是 i32，只需要写 42i32。
    //无后缀的数值字面量，其类型取决于怎样使用它们。如果没有限制，
    //编译器会对整数使用 i32，对浮点数使用 f64。
    //带后缀的字面量，其类型在初始化时已经知道了。
    let x = 1u8;
    let y = 2u32;
    let z = 3f32;

    //无后缀的字面量，其类型取决于如何使用它们。
    let i = 1;
    let f = 1.0;

    // `size_of_val` 返回一个变量所占的字节数
    //std::mem::size_of_val 是一个函数，这里使用其完整路径（full path）调用。
    //代 码可以分成一些叫做模块（module）的逻辑单元。
    //在本例中，size_of_val 函数是 在 mem 模块中定义的，而 mem 模块又是在 std crate 中定义的。
    println!("size of `x` in bytes: {}", std::mem::size_of_val(&x)); //传引用
    println!("size of `y` in bytes: {}", std::mem::size_of_val(&y));
    println!("size of `z` in bytes: {}", std::mem::size_of_val(&z));
    println!("size of `i` in bytes: {}", std::mem::size_of_val(&i));
    println!("size of `f` in bytes: {}", std::mem::size_of_val(&f));

    //Rust的类型推断引擎是很聪明的，它不只是在初始化时看看右值（r-value）的类型而已，
    //它还会考察变量之后会怎样使用，借此推断类型。这是一个类型推导的进阶例子：

    // 因为有类型说明，编译器知道 `elem` 的类型是 u8。
    let elem = 5u8;
    //创建一个空向量（vector，即不定长的，可以增长的数组）。
    let mut vec = Vec::new();
    //现在编译器还不知道 `vec` 的具体类型，只知道它是某种东西构成的向量（`Vec<_>`）
    //在向量中插入 `elem`。
    vec.push(elem);
    //啊哈！现在编译器知道 `vec` 是 u8 的向量了（`Vec<u8>`）。
    println!("{:?}", vec);

    //可以用 type 语句给已有的类型取个新的名字。
    //类型的名字必须遵循驼峰命名法（像是 CamelCase 这样）
    //否则编译器将给出错误。原生类型是例外，比如： usize、f32，等等。
    // `NanoSecond` = `Inch` = `u64_t` = `u64`.
    let nanoseconds: NanoSecond = 5 as u64_t;
    let inches: Inch = 2 as u64_t;

    // 注意类型别名*并不能*提供额外的类型安全，因为别名*并不是*新的类型。
    println!("{} nanoseconds + {} inches = {} unit?",
            nanoseconds,
            inches,
            nanoseconds + inches);
    //别名的主要用途是避免写出冗长的模板化代码（boilerplate code）。如 IoResult<T> 是 Result<T, IoError> 类型的别名。

}
```

# struct

## struct1

```rust
// structs1.rs
// Address all the TODOs to make the tests pass!


struct ColorClassicStruct {
    // TODO: Something goes here
}

struct ColorTupleStruct(/* TODO: Something goes here */);

#[derive(Debug)]
struct UnitStruct;

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn classic_c_structs() {
        // TODO: Instantiate a classic c struct!
        // let green =

        assert_eq!(green.name, "green");
        assert_eq!(green.hex, "#00FF00");
    }

    #[test]
    fn tuple_structs() {
        // TODO: Instantiate a tuple struct!
        // let green =

        assert_eq!(green.0, "green");
        assert_eq!(green.1, "#00FF00");
    }

    #[test]
    fn unit_structs() {
        // TODO: Instantiate a unit struct!
        // let unit_struct =
        let message = format!("{:?}s are fun!", unit_struct);

        assert_eq!(message, "UnitStructs are fun!");
    }
}

```

Rust自定义数据类型主要是通过struct和enum来创建。

struct是一个自定义数据类型，允许命名和包装多个相关的值，从而形成一个有意义的组合。如果熟悉一门面向对象语言,struct就像对象中的数据属性。

结构体共三种类型：
* 元组结构体
* 经典C风格结构体
* 类单元结构体(不带字段，在泛型中很有用)

结构体和元组类似，每一部分可以是不同类型，但不同于元组，结构体需要命名各部分数据以便能清楚的表明其值的意义。由于有了这些名字，结构体比元组更灵活，不需要依赖顺序来指定或访问实例中的值。

定义结构体，需要使用struct关键字并为整个结构体提供一个名字，用来描述它所组合的数据的意义。接着，在大括号中，定义每一部分数据的名字和类型，即**字段**。例如，下面这条例子展示了一个存储用户账号信息的结构体。

```rust
struct User {
    username: String,
    email: String,
    sign_in_count: u64,
    active: bool,
}
```

一旦定义了结构体后，为了使用它，就需要通过为每个字段指定具体值来创建这个结构体的实例。创建一个实例需要以结构体的名字开头，接着在大括号中使用键值对的形式提供字段。

```rust
let user1 = User {
    email: String::from("someone@example.com"),
    username: String::from("someusername123"),
    active: true,
    sign_in_count: 1,
};
```

如果想更改结构体的值，则整个结构体都必须是可变的，Rust并不允许只将某个字段标记为可变。

```rust
let mut user1 = User {
    email: String::from("someone@example.com"),
    username: String::from("someusername123"),
    active: true,
    sign_in_count: 1,
};

user1.email = String::from("anotheremail@example.com");
```

想从结构体中获取某个特定的值，可以使用点号。如果我们只想要用户的邮箱地址，可以用user1.email。

我们也可以通过函数的方式构造结构体的新实例，然后隐式的返回。

```rust
fn build_user(email: String, username: String) -> User {
    User {
        email: email,
        //email,
        username: username,
        //username,
        active: true,
        sign_in_count: 1,
    }
}
```

Rust提供了一个简写语法，若参数名和字段名完全相同，即可不用重写了。即上面实例的注释部分。只要我们想把他们设置为参数的值，就编写一个即可。

Rust还提供了一个结构体的更新语法，如果我们新创建了一个结构体实例，我们可以使用".."语法指定剩余未显式设置值的字段应有语给定实例对应字段相同的值。

```rust
let user2 = User {
    email: String::from("another@example.com"),
    username: String::from("anotherusername567"),
    ..user1 //其余的值都来自user1变量中实例的字段。
};
```

我们还可以定义元组结构体，元组结构体有着结构体名称提供的含义，但没有具体的字段名，只有字段的类型。(注意加分号)

```rust
struct Color(i32, i32, i32);
struct Point(i32, i32, i32);

let black = Color(0, 0, 0);
let origin = Point(0, 0, 0);
```

但注意这里black和origin值的类型不同，因为它们是不同的元组结构体的实例。我们每定义的一个结构体都有其自己的类型，即使结构体中的字段有着相同的类型。

与单元值类似，我们也可以定义一个没有任何字段的结构体，即类单元结构体，类似于unit类型。类单元结构体常常在想要在某个类型上实现trait但不需要在类型中存储数据的时候发挥作用。这是后话。

回到上题，分别给出了一个结构体和一个元组结构体的接口，下面还定义了一个类单元结构体，我们可以不用管它。

```rust
struct ColorClassicStruct {
    // TODO: Something goes here
}

struct ColorTupleStruct(/* TODO: Something goes here */);

#[derive(Debug)]
struct UnitStruct;
```

通过后面的测试代码，我们可以分析出结构体内部应该具备的实例，此外，我们还需要通过函数的方式创建这些结构体，并且没有参数传递，就说明结构体无需可变。

```rust
struct ColorClassicStruct {
    // TODO: Something goes here
    name: String,
    hex: String,
}

struct ColorTupleStruct(/* TODO: Something goes here */
    String,
    String
);

#[derive(Debug)]
struct UnitStruct;

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn classic_c_structs() {
        // TODO: Instantiate a classic c struct!\
        let green = ColorClassicStruct{
            name: "green".into(),
            hex: "#00FF00".into(),   
        };
        assert_eq!(green.name, "green");
        assert_eq!(green.hex, "#00FF00");
    }

    #[test]
    fn tuple_structs() {
        // TODO: Instantiate a tuple struct!
        let green = ("green", "#00FF00");

        assert_eq!(green.0, "green");
        assert_eq!(green.1, "#00FF00");
    }

    #[test]
    fn unit_structs() {
        // TODO: Instantiate a unit struct!
        let unit_struct = UnitStruct {};
        let message = format!("{:?}s are fun!", unit_struct);

        assert_eq!(message, "UnitStructs are fun!");
    }
}

```

关于下面这里是字符串字面值创建String类型的过程，可能大家会比较迷惑，因为这里是Rust和其他语言有很大不同的地方。大家可能会想，我若是直接令name:"green"是不是也可以？其实这是不对的。因为这其实就是两个类型。后续会陆续提到。Rust使用trait解决类型之间的转换问题，一般的转换会用到From和into两个trait。不过，即便常见的情况也可能会用到特别的trait，尤其是从String转换到别的类型，以及把别的类型转换到String时。

```rust
    fn classic_c_structs() {
        // TODO: Instantiate a classic c struct!\
        let green = ColorClassicStruct{
            name: "green".into(),
            hex: "#00FF00".into(),   
        };
        assert_eq!(green.name, "green");
        assert_eq!(green.hex, "#00FF00");
    }

```

我们接着上面来详细说明一下。

From和Into两个trait是内部相关联的，实际上这是它们实现的一部分。如果我们能够从类型B得到类型A，那么很容易相信我们也能把类型B转换为类型A。From trait允许一种类型定义"怎么根据另一种类型生成自己"，因此它提供了一种类型转换的简单机制。在标准库中有无数From的实现，规定原生类型及其他常见类型的转换功能。Into trait就是把From trait倒过来而已。也就是说，如果你为你的类型实现了From，那么同时你也就免费获得了Into。使用Into trait通常要求指明要转换到的类型，因为编译器大多数时候不能推断它。不过考虑到我们免费获得了Into，这点代价不值一提。上面就用到了into trait。


综上，struct1考察了创建结构体，元组结构体和类单元结构体并初始化的过程。

## struct2

```rust
// structs2.rs
// Address all the TODOs to make the tests pass!

#[derive(Debug)]
struct Order {
    name: String,
    year: u32,
    made_by_phone: bool,
    made_by_mobile: bool,
    made_by_email: bool,
    item_number: u32,
    count: u32,
}

fn create_order_template() -> Order {
    Order {
        name: String::from("Bob"),
        year: 2019,
        made_by_phone: false,
        made_by_mobile: false,
        made_by_email: true,
        item_number: 123,
        count: 0,
    }
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn your_order() {
        let order_template = create_order_template();
        // TODO: Create your own order using the update syntax and template above!
        // let your_order =
        assert_eq!(your_order.name, "Hacker in Rust");
        assert_eq!(your_order.year, order_template.year);
        assert_eq!(your_order.made_by_phone, order_template.made_by_phone);
        assert_eq!(your_order.made_by_mobile, order_template.made_by_mobile);
        assert_eq!(your_order.made_by_email, order_template.made_by_email);
        assert_eq!(your_order.item_number, order_template.item_number);
        assert_eq!(your_order.count, 1);
    }
}

```

这里考察了前面所述的结构体更新语法。题中要求我们构造一个结构体，除了name和count外所有实例都和order_template相等。所以代码如下：

```rust
let your_order = Order {
    name: "Hacker in Rust".into(),
    count: 1,
    ..order_template
        };
```

## struct3

```rust

// structs3.rs
// Structs contain data, but can also have logic. In this exercise we have
// defined the Package struct and we want to test some logic attached to it.
// Make the code compile and the tests pass!
// If you have issues execute `rustlings hint structs3`

#[derive(Debug)]
struct Package {
    sender_country: String,
    recipient_country: String,
    weight_in_grams: i32,
}

impl Package {
    fn new(sender_country: String, recipient_country: String, weight_in_grams: i32) -> Package {
        if weight_in_grams <= 0 {
            // Something goes here...
        } else {
            return Package {
                sender_country,
                recipient_country,
                weight_in_grams,
            };
        }
    }

    fn is_international(&self) -> ??? {
        // Something goes here...
    }

    fn get_fees(&self, cents_per_gram: i32) -> ??? {
        // Something goes here...
    }
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    #[should_panic]
    fn fail_creating_weightless_package() {
        let sender_country = String::from("Spain");
        let recipient_country = String::from("Austria");

        Package::new(sender_country, recipient_country, -2210);
    }

    #[test]
    fn create_international_package() {
        let sender_country = String::from("Spain");
        let recipient_country = String::from("Russia");

        let package = Package::new(sender_country, recipient_country, 1200);

        assert!(package.is_international());
    }

    #[test]
    fn create_local_package() {
        let sender_country = String::from("Canada");
        let recipient_country = sender_country.clone();

        let package = Package::new(sender_country, recipient_country, 1200);

        assert!(!package.is_international());
    }

    #[test]
    fn calculate_transport_fees() {
        let sender_country = String::from("Spain");
        let recipient_country = String::from("Spain");

        let cents_per_gram = ???;

        let package = Package::new(sender_country, recipient_country, 1500);

        assert_eq!(package.get_fees(cents_per_gram), 4500);
    }
}
```

上面采用impl关键字来定义了结构体成员方法，内有一关联函数和静态方法。

这是个计算洲际距离的程序，存在如下的几种情况：
* 创建实例失败：weight_in_grams小于0
* 创建实例成功并计算距离
  
assert!()函数接收的是bool的参数，当true时就会返回panic!。这是Rust独有的错误处理信息。由此我们可以知道package.is_international()返回的是bool类型。

通过assert_eq!判断可知，package.get_fees(cents_per_gram)返回的是i32类型。自此两个函数返回值类型已经确定。

接下来分析程序的逻辑。首先关注Package的方法。new函数首先会对weight_in_grams进行一个检查，若它小于0，根据后面的测试代码可知，我们的程序会panic!。所以在if条件的分支里加上panic!的语句。

其次关注is_international(&self)函数，它对应的情况是sender_country和recipient_country相等。不过因为它返回的是bool型，所以这个函数的作用就是对package内部的实例进行检查，如果sender_country和recipient_country不相等，就会导致panic!。所以就在该函数内添加判断。

最后关注get_fees(&self, cents_per_gram: i32)函数，根据调用的上下文可知，它接收一个i32的参数，返回一个i32的参数，很明显是在内部进行了计算，即和cents_per_gram进行了乘法操作。

所以答案如下：

```rust
#[derive(Debug)]
struct Package {
    sender_country: String,
    recipient_country: String,
    weight_in_grams: i32,
}

impl Package {
    fn new(sender_country: String, recipient_country: String, weight_in_grams: i32) -> Package {
        if weight_in_grams <= 0 {
            panic!("weight_in_grams must be greater than 0!");
            
        } else {
            return Package {
                sender_country,
                recipient_country,
                weight_in_grams,
            };
        }
    }

    fn is_international(&self) -> bool {
        self.sender_country != self.recipient_country
    }

    fn get_fees(&self, cents_per_gram: i32) -> i32 {
        cents_per_gram * self.weight_in_grams
    }
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    #[should_panic]  //测试
    fn fail_creating_weightless_package() {
        let sender_country = String::from("Spain");
        let recipient_country = String::from("Austria");

        Package::new(sender_country, recipient_country, -2210);
    }

    #[test]
    fn create_international_package() {
        let sender_country = String::from("Spain");
        let recipient_country = String::from("Russia");

        let package = Package::new(sender_country, recipient_country, 1200);

        assert!(package.is_international()); //若不是true会panic
    }

    #[test]
    fn create_local_package() {
        let sender_country = String::from("Canada");
        let recipient_country = sender_country.clone();

        let package = Package::new(sender_country, recipient_country, 1200);

        assert!(!package.is_international());
    }

    #[test]
    fn calculate_transport_fees() {
        let sender_country = String::from("Spain");
        let recipient_country = String::from("Spain");

        let cents_per_gram = 3;

        let package = Package::new(sender_country, recipient_country, 1500);

        assert_eq!(package.get_fees(cents_per_gram), 4500);
    }
}
```

panic!会在后续的错误报告专题中讲解，引用和借用在前面已经提过。

这里着重说一下self的问题。为什么在定义类方法里传的是self？这是因为关联函数(associated function)的第一个参数通常为self参数

“所有的trait都定义了一个隐式的类型Self，它指当前实现此接口的类型。” ——Rust官方文档

当self用作函数的第一个参数时，它等价于self: Self。&self参数等价于self: &Self。&mut self参数等价于self: &mut Self。

对照表：
* Self  => self: Self  允许实现者移动和修改对象，对应的闭包特性为FnOnce
* &self => self: &Self 既不允许实现者移动对象也不允许修改，对应的闭包特性为Fn
* &mut self => self: &mut Self 允许实现者修改对象但不允许移动，对应的闭包特性为FnMut
  
若不含self参数的关联函数则称为静态方法

各自使用方法如下：
* ::运算符。::语法用于关联函数和模块创建的命名空间
* .运算符

扯的太远了....后续在高级trait会讲到。

impl关键字后续也会补充。

下面再补充一些结构体的实例：

```rust
#[derive(Debug)]
struct Person<'a> {
    name: &'a str,
    age: u8,
}
//单元结构体
struct Nil;
//元组结构体
struct Pair(i32, f32);

//带有两个字段（field）的结构体
struct Point {
    x: f32,
    y: f32,
}

//结构体可以作为另一个结构体的字段
#[allow(dead_code)] //允许死代码
struct Rectangle {
    p1: Point,
    p2: Point,
}
fn main() {
    //使用简单的写法初始化字段，并创建结构体
    let name = "Peter";
    let age = 27;
    let peter = Person { name, age };
    //以Debug方式打印结构体
    println!("{:?}", peter);
    //实例化结构体 `Point`
    let point: Point = Point { x: 0.3, y: 0.4 };
    //访问 point 的字段
    println!("point coordinates: ({}, {})", point.x, point.y);
    //使用结构体更新语法创建新的 point，这样可以用到之前的 point 的字段
    let new_point = Point { x: 0.1, ..point };
    //`new_point.y` 与 `point.y` 一样，因为这个字段就是从 `point` 中来的
    println!("second point: ({}, {})", new_point.x, new_point.y);
    //使用 `let` 绑定来解构 point
    let Point { x: my_x, y: my_y } = point;
    let _rectangle = Rectangle {
        // 结构体的实例化也是一个表达式
        p1: Point { x: my_y, y: my_x },
        p2: point,
    };
    //实例化一个单元结构体
    let _nil = Nil;
    //实例化一个元组结构体
    let pair = Pair(1, 0.1);
    //访问元组结构体的字段
    println!("pair contains {:?} and {:?}", pair.0, pair.1);
    //解构一个元组结构体
    let Pair(integer, decimal) = pair;
    println!("pair contains {:?} and {:?}", integer, decimal);
}

```

输出：

```rust
Person { name: "Peter", age: 27 }
point coordinates: (0.3, 0.4)
second point: (0.1, 0.4)
pair contains 1 and 0.1
pair contains 1 and 0.1
```



