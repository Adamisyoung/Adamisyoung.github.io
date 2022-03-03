---
layout: post
title:  rustlings-string
date:   2021-07-04 19:17:31 +0800
categories:
- Rust/rustlings
---

> rustlings系列题解

# string1
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

# string2
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
这里后续添加。
