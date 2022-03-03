---
layout: post
title:  rustlings-option
date:   2021-07-04 19:22:31 +0800
categories:
- Rust/rustlings
---

> rustlings系列题解

# option1
```rust
// option1.rs
// Make me compile! Execute `rustlings hint option1` for hints

// you can modify anything EXCEPT for this function's sig
fn print_number(maybe_number: Option<u16>) {
    println!("printing: {}", maybe_number.unwrap());
}

fn main() {
    print_number(13);
    print_number(99);

    let mut numbers: [Option<u16>; 5];
    for iter in 0..5 {
        let number_to_add: u16 = {
            ((iter * 1235) + 2) / (4 * 16)
        };

        numbers[iter as usize] = number_to_add;
    }
}

```

前面我们叙述过Option枚举，这里是需要确定print_number()传入的参数是否存在，如果是None，则直接通过unwrap()panic!。所以就在调用print_number的位置的实参加上Some即可。

```rust
print_number(Some(13));
print_number(Some(99));
```

# option2

```rust
// option2.rs
// Make me compile! Execute `rustlings hint option2` for hints

fn main() {
    let optional_word = Some(String::from("rustlings"));
    // TODO: Make this an if let statement whose value is "Some" type
    word = optional_word {
        println!("The word is: {}", word);
    } else {
        println!("The optional word doesn't contain anything");
    }

    let mut optional_integers_vec: Vec<Option<i8>> = Vec::new();
    for x in 1..10 {
        optional_integers_vec.push(Some(x));
    }

    // TODO: make this a while let statement - remember that vector.pop also adds another layer of Option<T>
    // You can stack `Option<T>`'s into while let and if let
    integer = optional_integers_vec.pop() {
        println!("current value: {}", integer);
    }
}
```

optional_word作为Some()，作为右值赋值给word。所以word也是Some。这个匹配可用if-let完成。会匹配的非常优雅。

如果if let是左值，Some类型为右值绑定，则结构读作：若let将变量解构成Some(i)，则执行后面的语句块。如果要指明失败情形，就使用else。这相当于是一个解构的过程。我们可以提供一些额外的失败条件(和Option无关)。

同样，可以用if let匹配任何枚举值，见下面的实例：

```rust
let a = Foo::Bar;
let b = Foo::Baz;
let c = Foo::Qux(100);
    
//变量 a 匹配到了 Foo::Bar
if let Foo::Bar = a {
    println!("a is foobar");
}
    
//变量 b 没有匹配到 Foo::Bar，因此什么也不会打印。
if let Foo::Bar = b {
    println!("b is foobar");
}
    
// 量 c 匹配到了 Foo::Qux，它带有一个值，就和上面例子中的 Some() 类似。
if let Foo::Qux(value) = c {
    println!("c is {}", value);
}
```
另一个好处是if let 允许匹配枚举非参数化的变量，即枚举未注明 #[derive(PartialEq)]，我们也没有为其实现PartialEq。在这种情况下，通常if Foo::Bar==a会出错，因为此类枚举的实例不具有可比性。但是，if-let是可行的。


题目中我们是匹配Some的情况，代码如下：

```rust
if let Some(word) = optional_word {
        println!("The word is: {}", word);
    } else {
        println!("The optional word doesn't contain anything");
    }

```

接下来是Vector内部元素是Option枚举，所以integar需要套上两层Some。这里用到while-let。 和if-let类似，while-let也可以把别扭的match改写得好看一些。下面提供一个实例：
```rust
fn main() {
    // 将 `optional` 设为 `Option<i32>` 类型
    let mut optional = Some(0);
    //这读作：当 let将optional解构成Some(i)时，就
    //执行语句块。否则就 `break`。
    while let Some(i) = optional {
        if i > 9 {
            println!("Greater than 9, quit!");
            optional = None;
        } else {
            println!("`i` is `{:?}`. Try again.", i);
            optional = Some(i + 1);
        }
        // ^ 使用的缩进更少，并且不用显式地处理失败情况。
    }
    //if-let有可选的else/else if分句，
    //而while let没有。
}
```
接下来就很简单了：

```rust
while let Some(Some(integer)) = optional_integers_vec.pop() {
    println!("current value: {}", integer);
}
```

运行成功。

# option3
```rust
// option3.rs
// Make me compile! Execute `rustlings hint option3` for hints

// I AM NOT DONE

struct Point {
    x: i32,
    y: i32,
}

fn main() {
    let y: Option<Point> = Some(Point { x: 100, y: 200 });

    match y {
        Some(p) => println!("Co-ordinates are {},{} ", p.x, p.y),
        _ => println!("no match"),
    }
    y; // Fix without deleting this line.
}
```
这里对y的类型是Option，枚举内套用的泛型是经典结构体类型。
