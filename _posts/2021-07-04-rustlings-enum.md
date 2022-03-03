---
layout: post
title:  rustlings-enum
date:   2021-07-04 14:16:31 +0800
categories:
- Rust/rustlings
---

> rustlings系列题解

# enum1
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

# enum2
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

# enum3

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
