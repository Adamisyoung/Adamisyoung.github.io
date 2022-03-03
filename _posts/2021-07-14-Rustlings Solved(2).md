---
title: Rustlings题解(二)
copyright: true
mathjax: true
categories:
- Rust/Rustlings
tags: 
---

> Rustlings练习题的题解，可能不太完整。Rustlings最初由Carol Nichols编写，其中包含一些练习，旨在帮助Rust入门的开发人员习惯于阅读和编写Rust代码。

闲话少说，直接开始。

# enum

## enum1

```rust

// enums1.rs
// Make me compile! Execute `rustlings hint enums1` for hints!


#[derive(Debug)]
enum Message {
    // TODO: define a few types of messages as used below
}

fn main() {
    println!("{:?}", Message::Quit);
    println!("{:?}", Message::Echo);
    println!("{:?}", Message::Move);
    println!("{:?}", Message::ChangeColor);
}
```

考察的是枚举的构造。

枚举允许你通过列举可能的成员(variants)来定义一个类型，Rust的枚举和函数式编程语言中的代数数据类型最为相似。

假如我们要处理IP地址，我们程序可能会遇到的可能的地址类型都可以枚举出来。

```rust
enum IpAddrKind {
    V4,
    V6,
}
```

v4和v6就是该枚举的成员，而IpAddrKind就是一个自定义的数据类型。

这个类型后续就可以放置在结构体中存储。当然我们也可以用一种最简洁的方式，仅仅使用枚举并将数据直接放进每个枚举成员。枚举内的每个成员可以往外关联。比如：

```rust
enum IpAddr {
    V4(String),
    V6(String),
}
let home = IpAddr::V4(String::from("127.0.0.1"));

let loopback = IpAddr::V6(String::from("::1"));
```

像上面这样，我们直接将数据附加到枚举的每个成员上。

回到题目，我们构造合适的枚举即可通过。

```rust
#[derive(Debug)]
enum Message {
    // TODO: define a few types of messages as used below
    Quit,
    Echo,
    Move,
    ChangeColor
}
```

## enum2

```rust
// enums2.rs
// Make me compile! Execute `rustlings hint enums2` for hints!
#[derive(Debug)]
enum Message {
    // TODO: define the different variants used below
}

impl Message {
    fn call(&self) {
        println!("{:?}", &self);
    }
}

fn main() {
    let messages = [
        Message::Move { x: 10, y: 30 },  //典型结构体
        Message::Echo(String::from("hello world")),  //元组结构体
        Message::ChangeColor(200, 255, 255),   //元组结构体
        Message::Quit,   //单元结构体
    ];

    for message in &messages {
        message.call();
    }
}
```

这道题使用了impl关键字定义了枚举Message的方法。方法体使用了self来获取调用方法的值。这里简单举个例子：

```rust
impl Message {
    fn call(&self) {
        //在这里定义方法体
    }
}
let m = Message::Write(String::from("hello"));
m.call();
```

这个例子中，创建了一个值为Message::Write(String::from("hello"))的变量m，而且这就是当m.call()运行时call方法中的self的值。前面我们也说过了方法的调用方式。

从后面我们可以知道，题目的意思是想让我们在枚举中内嵌更多不同类型的变量。

```rust
#[derive(Debug)]
enum Message {
    // TODO: define the different variants used below
    Move {x: i32, y: i32},
    Echo(String),
    ChangeColor(u8, u8, u8),
    Quit
}
```

构造完成。定义一个这样的有关联值的枚举的方式和定义多个不同类型的结构体的方式很相像，但是区别是枚举不使用struct关键字以及其所有成员都被组合在一起位于Message类型下。而struct构造的类型都不相同。

此题通过。

## enum3

```rust
// enums3.rs
// Address all the TODOs to make the tests pass!

enum Message {
    // TODO: implement the message variant types based on their usage below
}

struct Point {
    x: u8,
    y: u8,
}

struct State {
    color: (u8, u8, u8),
    position: Point,
    quit: bool,
}

impl State {
    fn change_color(&mut self, color: (u8, u8, u8)) {
        self.color = color;
    }

    fn quit(&mut self) {
        self.quit = true;
    }

    fn echo(&self, s: String) {
        println!("{}", s);
    }

    fn move_position(&mut self, p: Point) {
        self.position = p;
    }

    fn process(&mut self, message: Message) {
        // TODO: create a match expression to process the different message variants
        
    }
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_match_message_call() {
        let mut state = State {
            quit: false,
            position: Point { x: 0, y: 0 },
            color: (0, 0, 0),
        };
        state.process(Message::ChangeColor((255, 0, 255)));
        state.process(Message::Echo(String::from("hello world")));
        state.process(Message::Move(Point { x: 10, y: 15 }));
        state.process(Message::Quit);

        assert_eq!(state.color, (255, 0, 255));
        assert_eq!(state.position.x, 10);
        assert_eq!(state.position.y, 15);
        assert_eq!(state.quit, true);
    }
}

```

根据下面我们可以知道枚举的成员。首先先构造枚举。值得注意的是Move内的类型是Point类型。

```rust
enum Message {
    // TODO: implement the message variant types based on their usage below
    Move {Point},
    Echo(String),
    ChangeColor(u8, u8, u8),
    Quit
}
```

Process方法和Message不同的模式都能对应，所以这里使用match控制流。match非常强大，它允许我们将一个值与一系列的模式相比较，并根据相匹配的模式执行相应代码。模式可由字面值、变量、通配符和许多其他内容构成。

下面即是Process内match控制流的写法。

```rust
match message {
        Message::ChangeColor(r, g, b) => {
        self.change_color((r, g, b));
    }
        Message::Echo(s) => self.echo(s),
        Message::Quit => self.quit(),
        Message::Move(pt) => {
            self.move_position(pt);
        }
}
```

我们列出match关键字后跟一个表达式，它和if有非常大的区别，对于if，表达式必须返回一个布尔值，而match它可以是任何类型的。

接下来是match的分支。一个分支有两个部分：一个模式和一些代码。第一个分支的模式是Message::ChangeColor(r, g, b)，而之后的箭头运算符就是将模式和将要运行的代码分开。

总结一下，当match表达式执行时，它将结果值按顺序与每一个分支的模式相比较。如果模式匹配了这个值，这个模式相关联的代码将被执行。如果模式并不匹配这个值，将继续执行下一个分支。每个分支相关联的代码是一个表达式，而表达式的结果值将作为整个match表达式的返回值。

最后为防止一些麻烦，Rust也提供了一个"_"通配符，_模式会匹配所有的值。通过将其放置于其他分支之后，将会匹配所有之前没有指定的可能的值。

match在只关心一个情况的场景中可能就有点啰嗦了。为此Rust提供了if let让控制流简洁。这是后话。

关于枚举，还有几类风格。如C风格的枚举，拥有隐式辨别值的enum：

```rust
enum Number {
    Zero,
    One,
    Two,
}
```

拥有显式辨别值的enum：

```rust
enum Color {
    Red = 0xff0000,
    Green = 0x00ff00,
    Blue = 0x0000ff,
}
```

下面是枚举的用法的样例。

```rust
// 该属性用于隐藏对未使用代码的警告。
#![allow(dead_code)]
//创建一个枚举来对web事件分类。变量名和类型共同指定了 `enum`
//取值的种类：`PageLoad` 不等于 `PageUnload`，`KeyPress(char)` 不等于
//`Paste(String)`。各个取值不同，互相独立。
enum WebEvent {
    PageLoad,
    PageUnload,
    KeyPress(char),
    Paste(String),
    Click { x: i64, y: i64 }
}
enum Status {
    Rich,
    Poor,
}

enum Work {
    Civilian,
    Soldier,
}
enum Number {
    Zero,
    One,
    Two,
}

enum Color {
    Red = 0xff0000,
    Green = 0x00ff00,
    Blue = 0x0000ff,
}

//此函数将一个 `WebEvent` enum 作为参数，无返回值。
fn inspect(event: WebEvent) {
    match event {
        WebEvent::PageLoad => println!("page loaded"),
        WebEvent::PageUnload => println!("page unloaded"),
        // 从 `enum` 里解构出 `c`。
        WebEvent::KeyPress(c) => println!("pressed '{}'.", c),
        WebEvent::Paste(s) => println!("pasted \"{}\".", s),
        // 把 `Click` 解构给 `x` and `y`。
        WebEvent::Click { x, y } => {
            println!("clicked at x={}, y={}.", x, y);
        },
    }
}

fn main() {
    let pressed = WebEvent::KeyPress('x');
    // `to_owned()` 从一个字符串切片中创建一个具有所有权的 `String`。
    let pasted  = WebEvent::Paste("my text".to_owned());
    let click   = WebEvent::Click { x: 20, y: 80 };
    let load    = WebEvent::PageLoad;
    let unload  = WebEvent::PageUnload;

    inspect(pressed);
    inspect(pasted);
    inspect(click);
    inspect(load);
    inspect(unload);

    //显式地use各个名称使他们直接可用，而不需要指定它们来自 `Status`。
    use Status::{Poor, Rich};
    //自动地 use`Work` 内部的各个名称。
    use Work::*;
    //`Poor` 等价于 `Status::Poor`。
    let status = Poor;
    //`Civilian` 等价于 `Work::Civilian`。
    let work = Civilian;

    match status {
        // 注意这里没有用完整路径，因为上面显式地使用了 `use`。
        Rich => println!("The rich have lots of money!"),
        Poor => println!("The poor have no money..."),
    }
    match work {
        // 再次注意到没有用完整路径。
        Civilian => println!("Civilians work!"),
        Soldier  => println!("Soldiers fight!"),
    }
    //`enum` 可以转成整型。
    println!("zero is {}", Number::Zero as i32);
    println!("one is {}", Number::One as i32);

    println!("roses are #{:06x}", Color::Red as i32);
    println!("violets are #{:06x}", Color::Blue as i32);
}

```

我们逐渐接触到mod的思想。

# modules

Rust的模块系统：
* 包：Cargo 的一个功能，它允许你构建、测试和分享 crate。
* Crates：一个模块的树形结构，它形成了库或二进制项目。
* 模块和use：允许你控制作用域和路径的私有性。
* 路径：一个命名例如结构体、函数或模块等项的方式
  
## module1

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

## module2

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

# collections

## vec1

```rust
// vec1.rs
// Your task is to create a `Vec` which holds the exact same elements
// as in the array `a`.
// Make me compile and pass the test!
// Execute the command `rustlings hint vec1` if you need hints.


fn array_and_vec() -> ([i32; 4], Vec<i32>) {
    let a = [10, 20, 30, 40]; // a plain array
    let v = // TODO: declare your vector here with the macro for vectors

    (a, v)
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_array_and_vec_similarity() {
        let (a, v) = array_and_vec();
        assert_eq!(a, v[..]);
    }
}
```

Vector的相关属性在前面说的差不多了。这里就是考察一下vec!宏构造。

```rust
let v = vec![10, 20, 30, 40];// TODO: declare your vector here with the macro for vectors
```

## vec2

```rust
// vec2.rs
// A Vec of even numbers is given. Your task is to complete the loop
// so that each number in the Vec is multiplied by 2.
//
// Make me pass the test!
//
// Execute the command `rustlings hint vec2` if you need
// hints.

fn vec_loop(mut v: Vec<i32>) -> Vec<i32> {
    for i in v.iter_mut() {
        // TODO: Fill this up so that each element in the Vec `v` is
        // multiplied by 2.
    }

    // At this point, `v` should be equal to [4, 8, 12, 16, 20].
    v
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_vec_loop() {
        let v: Vec<i32> = (1..).filter(|x| x % 2 == 0).take(5).collect();
        let ans = vec_loop(v.clone());

        assert_eq!(ans, v.iter().map(|x| x * 2).collect::<Vec<i32>>());
    }
}
```

这里考察我们遍历vector的元素。方式一般是通过索引一次一次的访问，但是我们如果想要改变元素的值，可以遍历可变vector的每一个元素的可变引用以便能改变它们。

```rust
fn vec_loop(mut v: Vec<i32>) -> Vec<i32> {
    for i in v.iter_mut() {  //可变引用
        // TODO: Fill this up so that each element in the Vec `v` is
        // multiplied by 2.
        *i = *i * 2;  //可以遍历可变 vector 的每一个元素的可变引用以便能改变他们
    }

    // At this point, `v` should be equal to [4, 8, 12, 16, 20].
    v
}
```

## hashmap1

```rust
// hashmap1.rs
// A basket of fruits in the form of a hash map needs to be defined.
// The key represents the name of the fruit and the value represents
// how many of that particular fruit is in the basket. You have to put
// at least three different types of fruits (e.g apple, banana, mango)
// in the basket and the total count of all the fruits should be at
// least five.
//
// Make me compile and pass the tests!
//
// Execute the command `rustlings hint hashmap1` if you need
// hints.


use std::collections::HashMap;

fn fruit_basket() -> HashMap<String, u32> {
    let mut basket = // TODO: declare your hash map here.

    // Two bananas are already given for you :)
    basket.insert(String::from("banana"), 2);

    // TODO: Put more fruits in your basket here.

    basket
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn at_least_three_types_of_fruits() {
        let basket = fruit_basket();
        assert!(basket.len() >= 3);
    }

    #[test]
    fn at_least_five_fruits() {
        let basket = fruit_basket();
        assert!(basket.values().sum::<u32>() >= 5);
    }
}
```

常用集合还有一个，就是哈希map。HashMap<K, V>类型储存了一个键类型K对应一个值类型V的映射。它通过一个哈希函数(hashing function)来实现映射。哈希map可以用于需要任何类型作为键来寻找数据的情况，而不是像vector那样通过索引

题目考察的是新建一个哈希map，我们可以使用new创建，并用insert添加元素。不过首先要use标准库中集合部分的HashMap。

```rust
let mut basket = HashMap::new();// TODO: declare your hash map here.
```

这样题目就能编译成功了。像vector一样，哈希map也将它们的数据储存在堆上，此外，哈希map是同质的：所有的键必须是相同类型，值也必须都是相同类型。

值得注意的是，哈希map insert的时候会有所有权的交接。像i32这样的实现了Copy trait的类型，其值可以拷贝进哈希map。但对于像String这样拥有所有权的值，其值将被移动而哈希map会成为这些值的所有者。

如果将值的引用插入哈希map，这些值本身将不会被移动进哈希map。但是这些引用指向的值必须至少在哈希map有效时也是有效的。

## hashmap2

```rust
// hashmap2.rs

// A basket of fruits in the form of a hash map is given. The key
// represents the name of the fruit and the value represents how many
// of that particular fruit is in the basket. You have to put *MORE
// THAN 11* fruits in the basket. Three types of fruits - Apple (4),
// Mango (2) and Lychee (5) are already given in the basket. You are
// not allowed to insert any more of these fruits!
//
// Make me pass the tests!
//
// Execute the command `rustlings hint hashmap2` if you need
// hints.

use std::collections::HashMap;

#[derive(Hash, PartialEq, Eq)]
enum Fruit {
    Apple,
    Banana,
    Mango,
    Lychee,
    Pineapple,
}

fn fruit_basket(basket: &mut HashMap<Fruit, u32>) {
    let fruit_kinds = vec![
        Fruit::Apple,
        Fruit::Banana,
        Fruit::Mango,
        Fruit::Lychee,
        Fruit::Pineapple,
    ];

    for fruit in fruit_kinds {
        // TODO: Put new fruits if not already present. Note that you
        // are not allowed to put any type of fruit that's already
        // present!
    }
}

#[cfg(test)]
mod tests {
    use super::*;

    fn get_fruit_basket() -> HashMap<Fruit, u32> {
        let mut basket = HashMap::<Fruit, u32>::new();
        basket.insert(Fruit::Apple, 4);
        basket.insert(Fruit::Mango, 2);
        basket.insert(Fruit::Lychee, 5);

        basket
    }

    #[test]
    fn test_given_fruits_are_not_modified() {
        let mut basket = get_fruit_basket();
        fruit_basket(&mut basket);
        assert_eq!(*basket.get(&Fruit::Apple).unwrap(), 4);
        assert_eq!(*basket.get(&Fruit::Mango).unwrap(), 2);
        assert_eq!(*basket.get(&Fruit::Lychee).unwrap(), 5);
    }

    #[test]
    fn at_least_five_types_of_fruits() {
        let mut basket = get_fruit_basket();
        fruit_basket(&mut basket);
        let count_fruit_kinds = basket.len();
        assert!(count_fruit_kinds >= 5);
    }

    #[test]
    fn greater_than_eleven_fruits() {
        let mut basket = get_fruit_basket();
        fruit_basket(&mut basket);
        let count = basket.values().sum::<u32>();
        assert!(count > 11);
    }
}


```

这个是搭配match方法向hashmap里存储。

```rust
for fruit in fruit_kinds {
        // TODO: Put new fruits if not already present. Note that you
        // are not allowed to put any type of fruit that's already
        // present!
    match fruit { //一个枚举和一个以枚举成员作为模式的 match 表达式
            Fruit::Apple => basket.insert(Fruit::Apple, 4),
            Fruit::Banana => basket.insert(Fruit::Banana, 4),
            Fruit::Mango => basket.insert(Fruit::Mango, 2),
            Fruit::Lychee => basket.insert(Fruit::Lychee, 5),
            Fruit::Pineapple => basket.insert(Fruit::Pineapple,5),
        };
    }
```

哈希map其他的性质后续会提到。

# strings

## strings1

```rust
// strings1.rs
// Make me compile without changing the function signature!
// Execute `rustlings hint strings1` for hints ;)


fn main() {
    let answer = current_favorite_color();
    println!("My current favorite color is {}", answer);
}

fn current_favorite_color() -> String {
    "blue"
}
```

之前的例子里我们提到过这个问题。Rust的核心语言中只有一种字符串类型：str。字符串Slice通常以被借用的形式出现，即&str。很多Vec可用的操作在 String中同样可用。当我们新建了一个叫做s的空的字符串，接着我们可以向其中装载数据。题目中有println!，所以可以使用to_string方法，它能用于任何实现了Display trait的类型。

```rust
fn current_favorite_color() -> String {
    "blue".to_string()
}
```

这里的问题是字符串字面值没有装在到String中，它们类型并不相同。

## strings2

```rust
  
// strings2.rs
// Make me compile without changing the function signature!
// Execute `rustlings hint strings2` for hints :)


fn main() {
    let word = String::from("green"); // Try not changing this line :)
    if is_a_color_word(&word) { //已经修改
        println!("That is a color word I know!");
    } else {
        println!("That is not a color word I know.");
    }
}

fn is_a_color_word(attempt: &str) -> bool {
    attempt == "green" || attempt == "blue" || attempt == "red"
}

```

该题目创建的方式是使用String::from函数从字符串字面值创建String。在效果上等于to_string。问题是出在，函数参数是字符串的引用。所以我们要在word前添加&。

字符串并不简单。。。很难。。。

# quiz2

```rust
  
// quiz2.rs
// This is a quiz for the following sections:
// - Strings

// Ok, here are a bunch of values-- some are `String`s, some are `&str`s. Your
// task is to call one of these two functions on each value depending on what
// you think each value is. That is, add either `string_slice` or `string`
// before the parentheses on each line. If you're right, it will compile!


fn string_slice(arg: &str) {
    println!("{}", arg);
}
fn string(arg: String) {
    println!("{}", arg);
}

fn main() {
    ???("blue");
    ???("red".to_string());
    ???(String::from("hi"));
    ???("rust is fun!".to_owned());
    ???("nice weather".into());
    ???(format!("Interpolation {}", "Station"));
    ???(&String::from("abc")[0..1]);
    ???("  hello there ".trim());
    ???("Happy Monday!".to_string().replace("Mon", "Tues"));
    ???("mY sHiFt KeY iS sTiCkY".to_lowercase());
}

```


```rust
fn main() {
    string_slice("blue");
    string("red".to_string());
    string(String::from("hi"));
    string("rust is fun!".to_owned());
    string("nice weather".into());
    string(format!("Interpolation {}", "Station"));
    string_slice(&String::from("abc")[0..1]);
    string_slice("  hello there ".trim());
    string("Happy Monday!".to_string().replace("Mon", "Tues"));
    string("mY sHiFt KeY iS sTiCkY".to_lowercase());
}

```

后续会添加。


# error_handling

## errors1

```rust
// errors1.rs
// This function refuses to generate text to be printed on a nametag if
// you pass it an empty string. It'd be nicer if it explained what the problem
// was, instead of just sometimes returning `None`. The 2nd test currently
// does not compile or pass, but it illustrates the behavior we would like
// this function to have.
// Execute `rustlings hint errors1` for hints!

// I AM NOT DONE

pub fn generate_nametag_text(name: String) -> Option<String> {
    if name.len() > 0 {
        Some(format!("Hi! My name is {}", name))
    } else {
        // Empty names aren't allowed.
        None
    }
}

#[cfg(test)]
mod tests {
    use super::*;

    // This test passes initially if you comment out the 2nd test.
    // You'll need to update what this test expects when you change
    // the function under test!
    #[test]
    fn generates_nametag_text_for_a_nonempty_name() {
        assert_eq!(
            generate_nametag_text("Beyoncé".into()),
            Some("Hi! My name is Beyoncé".into())
        );
    }

    #[test]
    fn explains_why_generating_nametag_text_fails() {
        assert_eq!(
            generate_nametag_text("".into()),
            Err("`name` was empty; it must be nonempty.".into())
        );
    }
}

```

Rust对可靠性的执着也延伸到了错误处理。在很多情况下，Rust要求你承认出错的可能性，并在编译代码之前就采取行动。这些要求使得程序更为健壮。Rust将错误组合为两个主要类别：可恢复错误和不可恢复错误。大部分语言并不区分这两类错误，并采用类似异常这样方式统一处理他们。Rust并没有异常，但是，有可恢复错误 Result<T, E>，和不可恢复(遇到错误时停止程序执行)错误panic!。

panic!前面已经接触过，但大多数错误并没有严重到需要程序完全停止执行。

我们先来说说题目中的那个option是怎么回事。

Option是标准库定义的一个枚举，它编码了一个常见的情况：一个值要么有值要么没值。Rust并没有空值，不过它确实拥有一个可以编码存在或不存在概念的枚举。这个枚举就是Option<T>。

```rust
enum Option<T> {
    Some(T),
    None,
}
```

泛型参数使得它可以包含任意类型的数据。我们现在知道它和错误处理没什么关系。对于可恢复错误，标准库也有一个枚举，Result。

```rust
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

T表示成功时返回的Ok成员中的数据类型，E代表失败时返回的Err成员中的错误类型。所以我们修改程序。

```rust
pub fn generate_nametag_text(name: String) -> Result<String, String> {
    if name.len() > 0 {
        Ok(format!("Hi! My name is {}", name))
    } else {
        // Empty names aren't allowed.
        //None
        Err("`name` was empty; it must be nonempty.".to_string())
    }
}

#[cfg(test)]
mod tests {
    use super::*;

    // This test passes initially if you comment out the 2nd test.
    // You'll need to update what this test expects when you change
    // the function under test!
    #[test]
    fn generates_nametag_text_for_a_nonempty_name() {
        assert_eq!(
            generate_nametag_text("Beyoncé".into()),
            Ok("Hi! My name is Beyoncé".into())
        );
    }

    #[test]
    fn explains_why_generating_nametag_text_fails() {
        assert_eq!(
            generate_nametag_text("".into()),
            Err("`name` was empty; it must be nonempty.".into())
        );
    }
}
```

这样就能通过了，

## errors2

```rust
// errors2.rs
// Say we're writing a game where you can buy items with tokens. All items cost
// 5 tokens, and whenever you purchase items there is a processing fee of 1
// token. A player of the game will type in how many items they want to buy,
// and the `total_cost` function will calculate the total number of tokens.
// Since the player typed in the quantity, though, we get it as a string-- and
// they might have typed anything, not just numbers!

// Right now, this function isn't handling the error case at all (and isn't
// handling the success case properly either). What we want to do is:
// if we call the `parse` function on a string that is not a number, that
// function will return a `ParseIntError`, and in that case, we want to
// immediately return that error from our function and not try to multiply
// and add.

// There are at least two ways to implement this that are both correct-- but
// one is a lot shorter! Execute `rustlings hint errors2` for hints to both ways.

use std::num::ParseIntError;

pub fn total_cost(item_quantity: &str) -> Result<i32, ParseIntError> {
    let processing_fee = 1;
    let cost_per_item = 5;
    let qty = item_quantity.parse::<i32>();

    Ok(qty * cost_per_item + processing_fee)
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn item_quantity_is_a_valid_number() {
        assert_eq!(total_cost("34"), Ok(171));
    }

    #[test]
    fn item_quantity_is_an_invalid_number() {
        assert_eq!(
            total_cost("beep boop").unwrap_err().to_string(),
            "invalid digit found in string"
        );
    }
}
```

当编写一个其实现会调用一些可能会失败的操作的函数时，除了在这个函数中处理错误外，还可以选择让调用者知道这个错误并决定该如何处理。这被称为传播错误。传播错误的简写是一个问好"?"。

```rust
let qty = item_quantity.parse::<i32>()?;
```

这里，调用结尾的?将会把Ok的值传给qty，如果出现错误，?会提前返回整个函数并将Err传播给调用者。我们甚至可以通过链式方法调用?来进一步缩减代码。样例如下

```rust
File::open("hello.txt")?.read_to_string(&mut s)?;
```

## errors3

```rust
// errors3.rs
// This is a program that is trying to use a completed version of the
// `total_cost` function from the previous exercise. It's not working though!
// Why not? What should we do to fix it?
// Execute `rustlings hint errors3` for hints!

use std::num::ParseIntError;

fn main() {
    let mut tokens = 100;
    let pretend_user_input = "8";

    let cost = total_cost(pretend_user_input)?;

    if cost > tokens {
        println!("You can't afford that many!");
    } else {
        tokens -= cost;
        println!("You now have {} tokens.", tokens);
    }
}

pub fn total_cost(item_quantity: &str) -> Result<i32, ParseIntError> {
    let processing_fee = 1;
    let cost_per_item = 5;
    let qty = item_quantity.parse::<i32>()?;

    Ok(qty * cost_per_item + processing_fee)
}
```

这道题将带问号的语句的位置移动到了main函数里。?运算符可被用于返回值类型为Result的函数，main原本的返回值是()，所以会报错。

有两种方法修复这个问题。一种技巧是将main函数返回值类型修改为Result<T, E>，另一种技巧是通过合适的方法使用match或Result的方法之一来处理 Result<T, E>。

main函数是特殊的，其返回什么类型是有限制的。main函数的一个有效的返回值是()，同时出于方便，另一个有效的返回值是Result<T, E>。

```rust
fn main() -> Result<(), ParseIntError> {
```

编译通过。

## errorsn

```rust

```


## result1

```rust

```

# generics

## generics1

```rust
// This shopping list program isn't compiling!
// Use your knowledge of generics to fix it.

// Execute `rustlings hint generics1` for hints!

fn main() {
    let mut shopping_list: Vec<?> = Vec::new();
    shopping_list.push("milk");
}

```

每一个编程语言都有高效处理重复概念的工具。在Rust中其工具之一就是泛型。泛型是具体类型或其他属性的抽象替代。Vec是泛型容器，我们在<>里面填上适合的类型即可。这里填入&str。

## generics2

```rust
// This powerful wrapper provides the ability to store a positive integer value.
// Rewrite it using generics so that it supports wrapping ANY type.

// Execute `rustlings hint generics2` for hints!


struct Wrapper {
    value: u32,
}

impl Wrapper {
    pub fn new(value: u32) -> Self {
        Wrapper { value }
    }
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn store_u32_in_wrapper() {
        assert_eq!(Wrapper::new(42).value, 42);
    }

    #[test]
    fn store_str_in_wrapper() {
        assert_eq!(Wrapper::new("Foo").value, "Foo");
    }
}
```

我们可以看到，测试代码里有两种数据类型。所以这要求我们将代码中涉及的部分都改为泛型实现。为了将要定义的函数的签名中的类型参数化，我们需要像给函数的值参数起名那样为该类型参数起一个名字。选择T的原因是它足够简单~

首先修改结构体。归根结底，Wrapper是围绕结构体建立的方法。

```rust
struct Wrapper<T> {
    value: T,
}
```

多类型的泛型不再讨论，都是一样的。

结构体修改后，紧随其后的是方法定义的泛型。

```rust
impl<T> Wrapper<T> {
    pub fn new(value: T) -> Self {
        Wrapper { value }
    }
    
}
```

在Wrapper<T>结构体上实现方法Wrapper，返回T类型的字段value。注意必须在 impl后面声明T，这样就可以在Wrapper<T>上实现的方法中使用它了。在impl 之后声明泛型T ，这样Rust就知道尖括号中的类型是泛型而不是具体类型。

实现泛型后，使用泛型类型参数的代码相比使用具体类型并没有任何速度上的损失。这是因为Rust通过在编译时进行泛型代码的单态化来保证效率。单态化是一个通过填充编译时使用的具体类型，将通用代码转换为特定代码的过程。我们可以使用泛型来编写不重复的代码，而Rust将会为每一个实例编译其特定类型的代码。这意味着在使用泛型时没有运行时开销；当代码运行，它的执行效率就跟好像手写每个具体定义的重复代码一样。这个单态化过程正是Rust泛型在运行时极其高效的原因。

## generics3

```rust
// An imaginary magical school has a new report card generation system written in Rust!
// Currently the system only supports creating report cards where the student's grade
// is represented numerically (e.g. 1.0 -> 5.5).
// However, the school also issues alphabetical grades (A+ -> F-) and needs
// to be able to print both types of report card!

// Make the necessary code changes in the struct ReportCard and the impl block
// to support alphabetical report cards. Change the Grade in the second test to "A+"
// to show that your changes allow alphabetical grades.

// Execute 'rustlings hint generics3' for hints!


pub struct ReportCard {
    pub grade: f32,
    pub student_name: String,
    pub student_age: u8,
}

impl ReportCard {
    pub fn print(&self) -> String {
        format!("{} ({}) - achieved a grade of {}",
            &self.student_name, &self.student_age, &self.grade)
    }
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn generate_numeric_report_card() {
        let report_card = ReportCard {
            grade: 2.1,
            student_name: "Tom Wriggle".to_string(),
            student_age: 12,
        };
        assert_eq!(
            report_card.print(),
            "Tom Wriggle (12) - achieved a grade of 2.1"
        );
    }

    #[test]
    fn generate_alphabetic_report_card() {
        // TODO: Make sure to change the grade here after you finish the exercise.
        let report_card = ReportCard {
            grade: A+,
            student_name: "Gary Plotter".to_string(),
            student_age: 11,
        };
        assert_eq!(
            report_card.print(),
            "Gary Plotter (11) - achieved a grade of A+"
        );
    }
}
```

这道题考察的是trait中定义共享。一个类型的行为由其可供调用的方法构成。如果可以对不同类型调用相同的方法的话，这些类型就可以共享相同的行为了。但是这样有限制，只有当trait或者要实现trait的类型位于crate的本地作用域时，才能为该类型实现trait。这个限制是被称为相干性的程序属性的一部分，或者更具体的说是孤儿规则。

trait也可以接受不同类型的参数。我们指定impl关键字和trait名称，而不是具体的类型。该参数支持任何实现了指定trait的类型，即Trait Bound语法。

示例如下：

```rust
pub fn notify<T: Summary>(item: T) {
    println!("Breaking news! {}", item.summarize());
}
```
trait bound与泛型参数声明在一起，位于尖括号中的冒号后面。题目中需要打印出来，就需要Dispaly Trait。我们之前说过，Display并没有针对泛型的实现。所以我们这次要打造一个专为ReportCard实现Dispaly trait的方法。

首先先将结构体改为泛型。因就一个grade需要泛型处理，所以不需要多加额外的泛型参数。

```rust
pub struct ReportCard <T>{
    pub grade: T,
    pub student_name: String,
    pub student_age: u8,
}
```

其次修改ReportCard方法，令trait共享。

```rust
impl<T:std::fmt::Display> ReportCard<T> {
    pub fn print(&self) -> String {
        format!("{} ({}) - achieved a grade of {}",
            &self.student_name, &self.student_age, &self.grade)
    }
}
```

通过使用带有trait bound的泛型参数的impl块，可以有条件地只为那些实现了特定trait的类型实现方法。

题目编译通过。
