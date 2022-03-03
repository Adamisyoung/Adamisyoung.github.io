---
layout: post
title:  rustlings-error_handling
date:   2021-07-04 19:18:31 +0800
categories:
- Rust/rustlings
---

> rustlings系列题解

# errors1
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

# errors2
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

# errors3
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

# errorsn
```rust

```


# result1
```rust

```