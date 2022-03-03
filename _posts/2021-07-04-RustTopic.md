---
title: Rust相关专题
copyright: true
mathjax: true
categories:
- Rust/Topic
tags: 
---

> 待补充。

此为Rust专题，是Rust的独有用法的合集。

# enum构造链表

```rust
use List::*;

enum List {
    //Cons：元组结构体，包含链表的一个元素和一个指向下一节点的指针
    Cons(u32, Box<List>),
    //Nil：末结点，表明链表结束
    Nil,
}

//可以为 enum 定义方法
impl List {
    //创建一个空的 List 实例
    fn new() -> List {
        //`Nil` 为 `List` 类型（译注：因 `Nil` 的完整名称是 `List::Nil`）
        Nil
    }

    //处理一个 List，在其头部插入新元素，并返回该 List
    fn prepend(self, elem: u32) -> List {
        //`Cons` 同样为 List 类型
        Cons(elem, Box::new(self))
    }

    //返回 List 的长度
    fn len(&self) -> u32 {
        //必须对 `self`进行匹配（match），因为这个方法的行为取决于 `self` 的
        //取值种类。
        //`self` 为 `&List` 类型，`*self` 为 `List` 类型，匹配一个具体的 `T`
        //类型要好过匹配引用 `&T`。
        match *self {
            //不能得到tail的所有权，因为`self`是借用的；
            //因此使用一个对tail的引用
            Cons(_, ref tail) => 1 + tail.len(),
            //(递归的)基准情形(base case)：一个长度为 0 的空列表
            Nil => 0
        }
    }

    //返回列表的字符串表示(该字符串是堆分配的)
    fn stringify(&self) -> String {
        match *self {
            Cons(head, ref tail) => {
                //`format!`和 `print!`类似，但返回的是一个堆分配的字符串，
                //而不是打印结果到控制台上
                format!("{}, {}", head, tail.stringify())
            },
            Nil => {
                format!("Nil")
            },
        }
    }
}

fn main() {
    //创建一个空链表
    let mut list = List::new();
    //追加一些元素
    list = list.prepend(1);
    list = list.prepend(2);
    list = list.prepend(3);

    //显示链表的最后状态
    println!("linked list has length: {}", list.len());
    println!("{}", list.stringify());
}
```

# 格式化打印(Display和Debug)

具体说说#[derive(Debug)]。

所有的类型，若想用std::fmt的格式化打印，都要求实现至少一个可打印的traits。自动的实现只为一些类型提供，比如std库中的类型。所有其他类型都必须手动实现。fmt::Debug这个trait使这项工作变得相当简单。所有类型都能推导(derive)fmt::Debug的实现。但是fmt::Display需要手动实现。

所有std库类型都天生可以使用{:?}来打印，不过，使用derive的一个问题是不能控制输出的形式。而且也会牺牲一些美感。我们可以通过{:#?}来美化打印。

```rust
//`derive`属性会自动创建所需的实现，使这个`struct`能使用 `fmt::Debug`打印。
#[derive(Debug)]
struct Structure(i32);//创建一个包含单个i32的结构体structure。命名为Structure
#[derive(Debug)]
struct Person<'a> {
    name: &'a str,
    age: u8
}
fn main() {
    //使用 {:?}打印 和使用 {}类似，但是丧失美感
    println!("{:?} months in a year.", Structure(3));
    let name = "Peter";
    let age = 27;
    let peter = Person { name, age }; //初始化结构体
    //美化打印
    println!("{:#?}", peter);
}
```

输出：

```rust
Person {
    name: "Peter",
    age: 27,
}
```

fmt::Debug通常看起来不太简洁，因此自定义输出的外观经常是更可取的。这需要通过手动实现fmt::Display来做到。fmt::Display采用{}标记。

不过面临一个问题，如何显示一些摇摆不定的类型？例如假设标准库对所有的Vec<T>都实现了同一种输出样式

* Vec<path>：/:/etc:/home/username:/bin(使用 : 分割)
* Vec<number>：1,2,3(使用 , 分割)
  
这样会分不清楚。所以对于任何泛型容器，Dispaly都没有实现。在泛型容器的情况要fmt::Debug。但任何非泛型容器，Display都可以实现。

下面是Debug输出和Dispaly输出的比较。

```rust
use std::fmt;  //使用use导入fmt模块使得fmt::Display可用
//fmt::Debug可自动实现。但是fmt::Display需要手动实现
#[derive(Debug)] 
struct MinMax(i64, i64); //创建带有两个数字的结构体。推导出Debug，以便与Display的输出进行比较
impl fmt::Display for MinMax {
//这个trait要求fmt使用与下面的函数完全一致的函数签名
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
    //仅将self的第一个元素写入到给定的输出流f。返回fmt:Result，此
    //结果表明操作成功或失败。注意write!的用法和println!很相似。
    //使用self.number来表示各个数据
        write!(f, "({}, {})", self.0, self.1)
    }
    //非泛型容器类型
}
#[derive(Debug)]
struct Point2D {
    x: f64,
    y: f64,
}
impl fmt::Display for Point2D {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        //自定义格式，使得仅显示x和y的值。
        write!(f, "x: {}, y: {}", self.x, self.y)
    }
}
fn main()
{
    let minmax = MinMax(0, 14);

    println!("Compare structures:");
    println!("Display: {}", minmax);
    println!("Debug: {:?}", minmax);

    let big_range =   MinMax(-300, 300);
    let small_range = MinMax(-3, 3);

    println!("The big range is {big} and the small is {small}",
             small = small_range,
             big = big_range);
    let point = Point2D { x: 3.3, y: 7.2 };
    println!("Compare points:");
    println!("Display: {}", point);
    println!("Debug: {:?}", point);
}
```

输出如下：

```rust
Compare structures:
Display: (0, 14)
Debug: MinMax(0, 14)
The big range is (-300, 300) and the small is (-3, 3)
Compare points:
Display: x: 3.3, y: 7.2
Debug: Point2D { x: 3.3, y: 7.2 }
```

std::fmt有很多类似Debug和Dispaly的trait，都有各自的实现方式。这里不一一赘述。

若对一个结构体实现fmt::Display，其中的元素需要一个接一个地处理，这可能会很麻烦。处理时用到的宏write!每次都要生成一个fmt::Result。为正确的处理每个Result，需要加上?操作符。

```rust
struct List(Vec<i32>);
impl fmt::Display for List {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        //使用元组的下标获取值，并创建一个vec的引用
        let vec = &self.0;
        write!(f, "[")?;
        //使用v对vec进行迭代，并用count记录迭代次数
        for (count, v) in vec.iter().enumerate() {
            //对每个元素（第一个元素除外）加上逗号
            //使用 ? 或 try! 来返回错误
        //    println!("{}", count);
            if count != 0 {write!(f, ", ")?;}
            write!(f, "{}: ", count);
            write!(f, "{}", v)?;  
        }
        //加上配对中括号，并返回一个fmt::Result值
        write!(f, "]")
    }
}

fn main() {
    let v = List(vec![1, 2, 3]);
    println!("{}", v);
}

```

输出如下：

```rust
[0: 1, 1: 2, 2: 3]
```

现在我们可以自定义输出了。是不是非常开心:)

我们还可以对字符串进行格式化处理。format!即将格式化文本写到字符串String。格式化的方式是通过格式化字符串来指定的

例如：
* format!("{}", foo) -> "3735928559"
* format!("0x{:X}", foo) -> "0xDEADBEEF"
* format!("0o{:o}", foo) -> "0o33653337357"
  
根据使用的参数类型是**X**、**o** 还是**未指定**，同样的变量(foo)能够格式化 成不同的形式。

该格式化功能是通过trait实现的。每种参数类型都对应一种trait。最常见的格式化trait就是Display，它可以处理参数类型为未指定的情况。

```rust
use std::fmt::{self, Formatter, Display};
struct City {
    name: &'static str,
    //纬度
    lat: f32,
    //经度
    lon: f32,
}
impl Display for City { 
    fn fmt(&self, f: &mut Formatter) -> fmt::Result {
        let lat_c = if self.lat >= 0.0 { 'N' } else { 'S' };
        let lon_c = if self.lon >= 0.0 { 'E' } else { 'W' };
        write!(f, "{}: {:.3}°{} {:.3}°{}",
               self.name, self.lat.abs(), lat_c, self.lon.abs(), lon_c)
    }
}
#[derive(Debug)]
struct Color {
    red: u8,
    green: u8,
    blue: u8,
}
impl Display for Color { 
    fn fmt(&self, f: &mut Formatter) -> fmt::Result {
        write!(f, "0x{:X}",self.red)
    }
}

fn main() {
    for city in [
        City { name: "Dublin", lat: 53.347778, lon: -6.259722 },
        City { name: "Oslo", lat: 59.95, lon: 10.75 },
        City { name: "Vancouver", lat: 49.25, lon: -123.1 },
    ].iter() {
        println!("{}", *city);
    }
    for color in [
        Color { red: 128, green: 255, blue: 90 },
        Color { red: 0, green: 3, blue: 254 },
        Color { red: 0, green: 0, blue: 0 },
    ].iter() {
        println!("{:?}", *color)
    }
}

```

初始化一个结构体，采用impl关键字来实例结构体的方法，为Color定义Display。

在上面的代码中，用到了一些新的方法，如Result返回，iter()迭代器，Formatter首指针等等。后续我们会一一涉及。

输出如下

```rust
Dublin: 53.348°N 6.260°W
Oslo: 59.950°N 10.750°E
Vancouver: 49.250°N 123.100°W
Color { red: 128, green: 255, blue: 90 }
Color { red: 0, green: 3, blue: 254 }
Color { red: 0, green: 0, blue: 0 }
```

