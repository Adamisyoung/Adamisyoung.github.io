---
layout: post
title:  rustlings-if
date:   2021-07-04 00:15:31 +0800
categories:
- Rust/rustlings
---

> rustlings系列题解

# if1
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

# if2
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
