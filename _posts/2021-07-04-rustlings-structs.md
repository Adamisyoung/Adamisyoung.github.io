---
layout: post
title:  rustlings-structs
date:   2021-07-04 13:16:31 +0800
categories:
- Rust/rustlings
---

> rustlings系列题解

# struct1
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

# struct2
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

# struct3
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



