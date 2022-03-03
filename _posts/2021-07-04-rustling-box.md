---
layout: post
title:  rustlings-智能指针
date:   2021-07-04 19:30:31 +0800
categories:
- Rust/rustlings
---

> rustlings系列题解

# box1
```rust
// box1.rs
//
// At compile time, Rust needs to know how much space a type takes up. This becomes problematic
// for recursive types, where a value can have as part of itself another value of the same type.
// To get around the issue, we can use a `Box` - a smart pointer used to store data on the heap,
// which also allows us to wrap a recursive type.
//
// The recursive type we're implementing in this exercise is the `cons list` - a data structure
// frequently found in functional programming languages. Each item in a cons list contains two
// elements: the value of the current item and the next item. The last item is a value called `Nil`.
//
// Step 1: use a `Box` in the enum definition to make the code compile
// Step 2: create both empty and non-empty cons lists by replacing `unimplemented!()`
//
// Note: the tests should not be changed
//
// Execute `rustlings hint box1` for hints :)

// I AM NOT DONE

#[derive(PartialEq, Debug)]
pub enum List {
    Cons(i32, List),
    Nil,
}

fn main() {
    println!("This is an empty cons list: {:?}", create_empty_list());
    println!(
        "This is a non-empty cons list: {:?}",
        create_non_empty_list()
    );
}

pub fn create_empty_list() -> List {
    unimplemented!()
}

pub fn create_non_empty_list() -> List {
    unimplemented!()
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_create_empty_list() {
        assert_eq!(List::Nil, create_empty_list())
    }

    #[test]
    fn test_create_non_empty_list() {
        assert_ne!(create_empty_list(), create_non_empty_list())
    }
}
```

这道题考察的是智能指针。Rust中最常见的指针是引用，以&符号为标志并借用了他们所指向的值。智能指针是一类数据结构，他们的表现类似指针，但是也拥有额外的元数据和功能。智能指针的概念并不为Rust 所独有；其起源于C++并存在于其他语言中。Rust标准库中不同的智能指针提供了多于引用的额外功能。

在Rust中，普通引用和智能指针的一个额外的区别是引用是一类只借用数据的指针；相反，在大部分情况下，智能指针**拥有**他们指向的数据。实际上Vec<T>和String都算是智能指针，因为它们拥有一些数据并允许你修改它们。它们也带有元数据和额外的功能或保证。

智能指针通常使用结构体实现。智能指针区别于常规结构体的显著特性在于其实现了Deref和Drop trait。Deref trait允许智能指针结构体实例表现的像引用一样，这样就可以编写既用于引用、又用于智能指针的代码。之前我们提及过Drop，Drop trait允许我们自定义当智能指针离开作用域时运行的代码。

上面这道题考察了最常用的智能指针：Box<T>，用于在堆上分配值。

box允许你将一个值放在堆上，并没有性能损失。它一般适用以下场景：

* 当有一个在编译时未知大小的类型，而又想要在需要确切大小的上下文中使用这个类型值的时候
* 当有大量数据并希望在确保数据不被拷贝的情况下转移所有权的时候
* 当希望拥有一个值并只关心它的类型是否实现了特定 trait 而不是其具体类型的时候

该题是属于第一种情况，递归类型。Rust需要在编译时知道类型占用多少空间。一种无法在编译时知道大小的类型是递归类型。其值的一部分可以是相同类型的另一个值。这种值的嵌套理论上可以无限的进行下去，所以Rust不知道递归类型需要多少空间。不过box的大小是已知的，所以通过在循环类型定义中插入box，就可以创建递归类型了。

```rust
pub enum List {
    Cons(i32, List),
    Nil,
}
```

枚举内包含一个cons list。cons list是一个来源于Lisp 编程语言及其方言的数据结构，每一项都包含两个元素：当前项的值和下一项，代表递归的终止条件的规范名称是Nil，它宣布列表的终止。

首先，其中一个函数的功能是创建空的列表，根据测试代码的比较参数，我们如下编写：

```rust
pub fn create_empty_list() -> List {
    let list = List::Nil;
    list
}
```

另外一个函数是让我们创建不空的列表，但是要和空表相等。所以：

```rust
pub fn create_non_empty_list() -> List {
    let list = List::Cons(1, Box::new(List::Nil));
    list
}
```

上面的cons储存了1和另一个List值，值为Nil。编译通过。

# arc1
```rust
// arc1.rs
// In this exercise, we are given a Vec of u32 called "numbers" with values ranging
// from 0 to 99 -- [ 0, 1, 2, ..., 98, 99 ]
// We would like to use this set of numbers within 8 different threads simultaneously.
// Each thread is going to get the sum of every eighth value, with an offset.
// The first thread (offset 0), will sum 0, 8, 16, ...
// The second thread (offset 1), will sum 1, 9, 17, ...
// The third thread (offset 2), will sum 2, 10, 18, ...
// ...
// The eighth thread (offset 7), will sum 7, 15, 23, ...

// Because we are using threads, our values need to be thread-safe.  Therefore,
// we are using Arc.  We need to make a change in each of the two TODOs.


// Make this code compile by filling in a value for `shared_numbers` where the
// first TODO comment is, and create an initial binding for `child_numbers`
// where the second TODO comment is. Try not to create any copies of the `numbers` Vec!
// Execute `rustlings hint arc1` for hints :)

// I AM NOT DONE

#![forbid(unused_imports)] // Do not change this, (or the next) line.
use std::sync::Arc;
use std::thread;

fn main() {
    let numbers: Vec<_> = (0..100u32).collect();
    let shared_numbers = // TODO
    let mut joinhandles = Vec::new();

    for offset in 0..8 {
        let child_numbers = // TODO
        joinhandles.push(thread::spawn(move || {
            let mut i = offset;
            let mut sum = 0;
            while i < child_numbers.len() {
                sum += child_numbers[i];
                i += 8;
            }
            println!("Sum of offset {} is {}", offset, sum);
        }));
    }
    for handle in joinhandles.into_iter() {
        handle.join().unwrap();
    }
}
```

该题目引入引用计数智能指针(Arc)，它能够让程序以线程安全的方式在线程间共享不可变数据。当你试图在线程间共享数据时，需要Arc类型来保证被共享的类型的生命周期，与运行时间最长的线程活得一样久。

```rust
use std::thread;
use std::time::Duration;
fn main() {
  let foo = vec![0]; // creation of foo here
  thread::spawn(|| {
    thread::sleep(Duration::from_millis(20));
    println!("{:?}", &foo); 
  });
} // foo gets dropped here

// wait 20 milliseconds
// try to print foo
```
这段代码无法编译通过。我们会得到一个错误，称foo的引用活得比foo自身更久。这是因为foo在main函数结尾处就被丢弃(drop)了，但这个被丢弃的值会在20毫秒后在生成的线程中被试图访问。这就是Arc的作用所在。

```rust
use std::thread;
use std::sync::Arc;
use std::time::Duration;

fn main() {
  let foo = Arc::new(vec![0]); 
  let bar = Arc::clone(&foo);
  thread::spawn(move || {
    thread::sleep(Duration::from_millis(20));
    println!("{:?}", *bar);
  });
  
  println!("{:?}", foo);
} 

```
当程序调用let foo = Arc::new(vec![0])时，它同时创建了一个vec![0]和一个值为1的原子引用计数，并且把它们都存储在堆上的相同位置(紧挨着)。指向堆上的这份数据的指针存放在foo中。因此，foo是由指向一个对象的指针构成，被指向的对象包含vec![0]和原子计数。

当你调用let bar = Arc::clone(&foo)时，你是在获取foo的一个引用、对foo(指向存放在堆上的数据的指针)解引用、接着找到foo指向的地址、找出里面存放的值(vec![0]和原子计数)、把原子计数加一、最后把指向vec![0]的指针保存在bar中。

当foo或bar离开作用域时，Arc::drop()就被调用了，原子计数减一。如果Arc::drop()发现原子计数等于0，那么它所指向的堆上的数据(vec![0]和原子计数)会被清理并从堆上擦除。所以这种计数是一种能以线程安全的方式修改和增加它的值的类型。在对原子类型允许进行其他操作之前，前面的原子类型操作必须要全部完成，因此被称为原子的(即不可分割的)操作。

Arc只能包含不可变数据。这是因为如果两个线程试图在同一时间修改被包含的值，Arc无法保证避免数据竞争。所以如果希望修改数据，就应该在Arc类型内部封装一个互斥锁保护。因为数据引用存在的期间，数据都是有效的，所以它足够安全。

原理解释清楚了，那么我们可以开始填充了。

首先创建对number的原子技术：

```rust
let shared_numbers = Arc::new(numbers);
```

其次通过引用和解引用获取存放在堆上的数据：
```rust
let child_numbers = Arc::clone(&shared_numbers);
```

至于其他多线程并不需要理解(暂时)

