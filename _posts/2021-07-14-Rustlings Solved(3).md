---
title: Rustlings题解(三)
copyright: true
mathjax: true
categories:
- Rust/Rustlings
tags: 
---

> Rustlings练习题的题解，可能不太完整。Rustlings最初由Carol Nichols编写，其中包含一些练习，旨在帮助Rust入门的开发人员习惯于阅读和编写Rust代码。

闲话少说，直接开始。

# option

## option1

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

## option2

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

## option3

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

# traits

## traits1

```rust
// traits1.rs
// Time to implement some traits!
//
// Your task is to implement the trait
// `AppendBar' for the type `String'.
//
// The trait AppendBar has only one function,
// which appends "Bar" to any object
// implementing this trait.

// I AM NOT DONE

trait AppendBar {
    fn append_bar(self) -> Self;
}

impl AppendBar for String {
    //Add your code here
}

fn main() {
    let s = String::from("Foo");
    let s = s.append_bar();
    println!("s: {}", s);
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn is_FooBar() {
        assert_eq!(String::from("Foo").append_bar(), String::from("FooBar"));
    }

    #[test]
    fn is_BarBar() {
        assert_eq!(
            String::from("").append_bar().append_bar(),
            String::from("BarBar")
        );
    }
}
```

在这里我们定义了一个trait，一个类型的行为由其可供调用的方法构成。题目中定义了AppendBar trait，里面包含了由append_bar()方法提供的行为。现在我们为String类型实现这个trait，根据后文，我们可实现具体方法如下：

```rust
fn append_bar(self) -> Self {
    self + "Bar"
}
```

在类型上实现trait类似于实现与trait无关的方法。区别在于impl关键字之后，我们提供需要实现trait的名称，接着是for和需要实现trait的类型的名称。在impl块中，使用trait定义中的方法签名，不过不再后跟分号，而是需要在大括号中编写函数体来为特定类型实现trait方法所拥有的行为。

一旦实现了 trait，我们就可以调用它了。

## traits2

```rust
// traits2.rs
//
// Your task is to implement the trait
// `AppendBar' for a vector of strings.
//
// To implement this trait, consider for
// a moment what it means to 'append "Bar"'
// to a vector of strings.
//
// No boiler plate code this time,
// you can do this!

trait AppendBar {
    fn append_bar(self) -> Self;
}

//TODO: Add your code here

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn is_vec_pop_eq_bar() {
        let mut foo = vec![String::from("Foo")].append_bar();
        assert_eq!(foo.pop().unwrap(), String::from("Bar"));
        assert_eq!(foo.pop().unwrap(), String::from("Foo"));
    }
}
```

根据后文推断出trait是为vector实现的，而内部的元素类型说String。所以编写impl如下：

```rust
impl AppendBar for Vec<String> {
    fn append_bar(self) -> Self {
        let mut a: Vec<String> = self;
        a.push("Bar".to_owned());
        a
    }
}

```

内部要先设置vector可变，而后初始化字符串，再向内装载值。编译通过。

# tests

## tests1

```rust
// tests1.rs
// Tests are important to ensure that your code does what you think it should do.
// Tests can be run on this file with the following command:
// rustlings run tests1

// This test has a problem with it -- make the test compile! Make the test
// pass! Make the test fail! Execute `rustlings hint tests1` for hints :)

// I AM NOT DONE

#[cfg(test)]
mod tests {
    #[test]
    fn you_can_assert() {
        assert!();
    }
}
```

> "Program testing can be a very effective way to show the presence of bugs, but it is hopelessly inadequate for showing their absence.”

程序的正确性意味着代码如我们期望的那样运行。Rust是一个相当注重正确性的编程语言，不过正确性是一个难以证明的复杂主题。Rust的类型系统在此问题上下了很大的功夫，不过它不可能捕获所有种类的错误。为此，Rust也在语言本身包含了编写软件测试的支持。

我们之前见过的各自test，现在终于能实际碰触到了。

Rust中的测试就是一个带有test属性注解的函数。属性是关于Rust代码片段的元数据，我们之前接触的derive就是属性的一个例子。为了将一个函数变成测试函数，需要在fn行之前加上#[test]

我们测试后需要结果时，就需要借助assert!宏了。assert!宏由标准库提供，在希望确保测试中一些条件为true时非常有用。需要向assert!宏提供一个求值为布尔值的参数。如果值是true，assert!什么也不做，同时测试会通过。如果值为false，assert!调用panic!宏，会导致测试失败。

在测试相等的时候就使用使用assert_eq!和 assert_ne!。这两个宏分别比较两个值是相等还是不相等。当断言失败时他们也会打印出这两个值具体是什么，以便于观察测试为什么失败。在一些语言和测试框架中，断言两个值相等的函数的参数叫做expected和actual，而且指定参数的顺序是很关键的。然而在Rust中，它们则叫做left和right。

我们还能注意到测试时也会有use super::*;。它遵循之前提过的可见性规则，将外部模块引入到内部模块的作用域中。

当然，也有一些别扭的情况。比如我们希望代码能按照期望处理错误，即让代码失败。这就可以通过对函数增加另一个属性should_panic来实现这些。这个属性在函数中的代码panic时会通过，而在其中的代码没有panic时失败。

扯远了，题目中就是assert!()缺少参数，加个true就好了。

## tests2

```rust
// tests2.rs
// This test has a problem with it -- make the test compile! Make the test
// pass! Make the test fail! Execute `rustlings hint tests2` for hints :)

// I AM NOT DONE

#[cfg(test)]
mod tests {
    #[test]
    fn you_can_assert_eq() {
        assert_eq!();
    }
}

```

这里也是缺参数。

```rust
assert_eq!(1, 1);
```

## tests3



# quiz3

```rust
// quiz3.rs
// This is a quiz for the following sections:
// - Tests

// This quiz isn't testing our function -- make it do that in such a way that
// the test passes. Then write a second test that tests that we get the result
// we expect to get when we call `times_two` with a negative number.
// No hints, you can do this :)

pub fn times_two(num: i32) -> i32 {
    num * 2
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn returns_twice_of_positive_numbers() {
        assert_eq!(times_two(4), ???);
    }

    #[test]
    fn returns_twice_of_negative_numbers() {
        // TODO replace unimplemented!() with an assert for `times_two(-4)`
        unimplemented!()
    }
}
```

这个就是补充一下参数，多加一个判断条件而已。

```rust
pub fn times_two(num: i32) -> i32 {
    num * 2
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn returns_twice_of_positive_numbers() {
        assert_eq!(times_two(4), 8);
    }

    #[test]
    fn returns_twice_of_negative_numbers() {
        // TODO replace unimplemented!() with an assert for `times_two(-4)`
        //unimplemented!()
        assert_eq!(times_two(-4), -8);
    }
}
```

# box

## box1

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

## arc1

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

# iterators

## iterators1

```rust
// iterators1.rs
//
//  Make me compile by filling in the `???`s
//
// When performing operations on elements within a collection, iterators are essential.
// This module helps you get familiar with the structure of using an iterator and
// how to go through elements within an iterable collection.
//
// Execute `rustlings hint iterators1` for hints :D
fn main () {
    let my_fav_fruits = vec!["banana", "custard apple", "avocado", "peach", "raspberry"];

    let mut my_iterable_fav_fruits = ???;   // TODO: Step 1

    assert_eq!(my_iterable_fav_fruits.next(), Some(&"banana"));
    assert_eq!(my_iterable_fav_fruits.next(), ???);     // TODO: Step 2
    assert_eq!(my_iterable_fav_fruits.next(), Some(&"avocado"));
    assert_eq!(my_iterable_fav_fruits.next(), ???);     // TODO: Step 2.1
    assert_eq!(my_iterable_fav_fruits.next(), Some(&"raspberry"));
    assert_eq!(my_iterable_fav_fruits.next(), ???);     // TODO: Step 3
}

```

Rust内含函数式编程的思想。迭代器模式允许我们对一个项的序列进行处理。在Rust中，迭代器是惰性的，这意味着在调用方法使用迭代器之前它都不会有效果。

下面是创建的一个实例：

```rust
let v1 = vec![1, 2, 3];
let v1_iter = v1.iter();
for val in v1_iter {
    println!("Got: {}", val);
}
```

所以上面那道题即如下所写即可：

```rust
let mut my_iterable_fav_fruits = my_fav_fruits.iter();  
```

后续，程序中调用了迭代器的next方法，这些方法被称为消费适配器。从next调用中得到的值是vector的不可变引用。iter方法生成一个**不可变引用的迭代器**。


## iterators2

```rust
// iterators2.rs
// In this exercise, you'll learn some of the unique advantages that iterators
// can offer. Follow the steps to complete the exercise.
// As always, there are hints if you execute `rustlings hint iterators2`!

// I AM NOT DONE

// Step 1.
// Complete the `capitalize_first` function.
// "hello" -> "Hello"
pub fn capitalize_first(input: &str) -> String {
    let mut c = input.chars();
    match c.next() {
        None => String::new(),
        Some(first) => ???,
    }
}

// Step 2.
// Apply the `capitalize_first` function to a slice of string slices.
// Return a vector of strings.
// ["hello", "world"] -> ["Hello", "World"]
pub fn capitalize_words_vector(words: &[&str]) -> Vec<String> {
    vec![]
}

// Step 3.
// Apply the `capitalize_first` function again to a slice of string slices.
// Return a single string.
// ["hello", " ", "world"] -> "Hello World"
pub fn capitalize_words_string(words: &[&str]) -> String {
    String::new()
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_success() {
        assert_eq!(capitalize_first("hello"), "Hello");
    }

    #[test]
    fn test_empty() {
        assert_eq!(capitalize_first(""), "");
    }

    #[test]
    fn test_iterate_string_vec() {
        let words = vec!["hello", "world"];
        assert_eq!(capitalize_words_vector(&words), ["Hello", "World"]);
    }

    #[test]
    fn test_iterate_into_string() {
        let words = vec!["hello", " ", "world"];
        assert_eq!(capitalize_words_string(&words), "Hello World");
    }
}
```

由测试代码可知，capitalize_words_vector(&words)将vector内的元素字符串首字母进行大写处理，capitalize_words_string(&words)是将vector元素拼接成一个完整的字符串。这就需要遍历后创建一个新的迭代器并收集处理。

则代码如下编写：

```rust
pub fn capitalize_words_vector(words: &[&str]) -> Vec<String> {
    let capitalize_words: Vec<String> = words.iter().map(|x| capitalize_first(x)).collect();
    capitalize_words
} 
```

这里是调用map方法创建一个新迭代器，对原始的vector进行capitalize_first(x)处理，并调用collect方法消费新迭代器并创建一个vector。非常高效，酣畅淋漓。因为map 获取一个闭包，所以可以指定任何希望在遍历的每个元素上执行的操作。

既然引入了闭包的概念，我们也需要详细说一说。

Rus的闭包是可以保存进变量或作为参数传递给其他函数的匿名函数。可以在一个地方创建闭包，然后在不同的上下文中执行闭包运算。

```rust
let capitalize_words: Vec<String> = words.iter().map(|x| capitalize_first(x)).collect();
```

闭包的定义是从一对竖线开始，在竖线中指定闭包的参数。如果是多个参数，就需要用逗号分离。闭包被开发的初衷是我们需要在一个位置定义代码，储存代码，并在之后的位置实际调用它。

下面是闭包的另一个例子：

```rust
let expensive_closure = |num| {
    println!("calculating slowly...");
    thread::sleep(Duration::from_secs(2));
    num
};
```

参数之后是存放闭包体的大括号,如果闭包体只有一行则大括号是可以省略的。大括号之后闭包的结尾，需要用于let语句的分号。闭包体(num)最后一行的返回值作为调用闭包时的返回值。

需要格外注意的是**这个let语句意味着expensive_closure包含一个匿名函数的定义，不是调用匿名函数的返回值**。

闭包很神奇的地方是不要求像fn函数那样在参数和返回值上注明类型。函数中需要类型注解是因为他们是暴露给用户的显式接口的一部分。严格的定义这些接口对于保证所有人都认同函数使用和返回值的类型来说是很重要的。但是闭包并不用于这样暴露在外的接口：他们储存在变量中并被使用，不用命名他们或暴露给库的用户调用。在有限的上下文中，编译器能可靠的推断参数和返回值的类型。

带有泛型和Fn trait的闭包等后面遇到再提吧...

## iterators3

```rust
  
// iterators3.rs
// This is a bigger exercise than most of the others! You can do it!
// Here is your mission, should you choose to accept it:
// 1. Complete the divide function to get the first four tests to pass.
// 2. Get the remaining tests to pass by completing the result_with_list and
//    list_of_results functions.
// Execute `rustlings hint iterators3` to get some hints!

// I AM NOT DONE

#[derive(Debug, PartialEq, Eq)]
pub enum DivisionError {
    NotDivisible(NotDivisibleError),
    DivideByZero,
}

#[derive(Debug, PartialEq, Eq)]
pub struct NotDivisibleError {
    dividend: i32,
    divisor: i32,
}

// Calculate `a` divided by `b` if `a` is evenly divisible by `b`.
// Otherwise, return a suitable error.
pub fn divide(a: i32, b: i32) -> Result<i32, DivisionError> {}

// Complete the function and return a value of the correct type so the test passes.
// Desired output: Ok([1, 11, 1426, 3])
fn result_with_list() -> () {
    let numbers = vec![27, 297, 38502, 81];
    let division_results = numbers.into_iter().map(|n| divide(n, 27));
}

// Complete the function and return a value of the correct type so the test passes.
// Desired output: [Ok(1), Ok(11), Ok(1426), Ok(3)]
fn list_of_results() -> () {
    let numbers = vec![27, 297, 38502, 81];
    let division_results = numbers.into_iter().map(|n| divide(n, 27));
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_success() {
        assert_eq!(divide(81, 9), Ok(9));
    }

    #[test]
    fn test_not_divisible() {
        assert_eq!(
            divide(81, 6),
            Err(DivisionError::NotDivisible(NotDivisibleError {
                dividend: 81,
                divisor: 6
            }))
        );
    }

    #[test]
    fn test_divide_by_0() {
        assert_eq!(divide(81, 0), Err(DivisionError::DivideByZero));
    }

    #[test]
    fn test_divide_0_by_something() {
        assert_eq!(divide(0, 81), Ok(0));
    }

    #[test]
    fn test_result_with_list() {
        assert_eq!(format!("{:?}", result_with_list()), "Ok([1, 11, 1426, 3])");
    }

    #[test]
    fn test_list_of_results() {
        assert_eq!(
            format!("{:?}", list_of_results()),
            "[Ok(1), Ok(11), Ok(1426), Ok(3)]"
        );
    }
}
```

代码逻辑很简单，测试代码首先检验的是能不能整除，其次是除0的情况等等。在一些异常情况也引入Result。

所以首先修正divide()

```rust
pub fn divide(a: i32, b: i32) -> Result<i32, DivisionError> {
    if b == 0 {return Err(DivisionError::DivideByZero);}
    if a % b == 0 {
    Ok(a / b)
    }
    else {
        Err(DivisionError::NotDivisible(NotDivisibleError{dividend:a, divisor:b}))
    }
}
```

根据两个错误类型的枚举，我们将Err信息对应上。最后返回Ok(a / b)。

divide功能完好后，第二个函数是需要验证vector是否能整除，返回的是个Result，即能整除就是Ok，不能整除就是DivisionError。

```rust
fn result_with_list() -> Result<Vec<i32>, DivisionError> {
    let numbers = vec![27, 297, 38502, 81];
    let division_results = numbers.into_iter().map(|n| divide(n, 27));
    let x: Result<Vec<i32>, DivisionError>= division_results.collect();
    x

```

调用map方法创建一个新迭代器，对参数进行divide(n, 27)处理，并调用collect方法消费新迭代器并创建一个Result。

第三个函数是对vector内部的元素验证是否整除，和上面的函数唯一的区别是返回类型不同。返回的是vector，内部是Result类型。

```rust
fn list_of_results() ->  Vec<Result<i32, DivisionError>>{
    let numbers = vec![27, 297, 38502, 81];
    let division_results = numbers.into_iter().map(|n| divide(n, 27));
    let x: Vec<Result<i32, DivisionError>> = division_results.collect();
    x
}
```

## iterators4

```rust
// iterators4.rs

// I AM NOT DONE

pub fn factorial(num: u64) -> u64 {
    // Complete this function to return the factorial of num
    // Do not use:
    // - return
    // Try not to use:
    // - imperative style loops (for, while)
    // - additional variables
    // For an extra challenge, don't use:
    // - recursion
    // Execute `rustlings hint iterators4` for hints.
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn factorial_of_1() {
        assert_eq!(1, factorial(1));
    }
    #[test]
    fn factorial_of_2() {
        assert_eq!(2, factorial(2));
    }

    #[test]
    fn factorial_of_4() {
        assert_eq!(24, factorial(4));
    }
}
```

这个是对标准库内方法的使用。代码的逻辑是让你求阶乘。

实现方法很简单

* 闭包实现：
    ```rust
    (1..=num).fold(1, |acc, v| acc * v)
    ```
    内含两个参数，进行累积乘法。fold()是一个非常强大的方法。它返回一个特殊的“累加器”类型闭包的结果给迭代器的所有元素，得到一个单一的值。
* 标准库方法：
    ```rust
    (1..=num).product()
    ```
    非常方便的一种方法。

## iterators5

```rust
// iterators5.rs

// Let's define a simple model to track Rustlings exercise progress. Progress
// will be modelled using a hash map. The name of the exercise is the key and
// the progress is the value. Two counting functions were created to count the
// number of exercises with a given progress. These counting functions use
// imperative style for loops. Recreate this counting functionality using
// iterators. Only the two iterator methods (count_iterator and
// count_collection_iterator) need to be modified.
// Execute `rustlings hint iterators5` for hints.
//
// Make the code compile and the tests pass.

// I AM NOT DONE

use std::collections::HashMap;

#[derive(Clone, Copy, PartialEq, Eq)]
enum Progress {
    None,
    Some,
    Complete,
}

fn count_for(map: &HashMap<String, Progress>, value: Progress) -> usize {
    let mut count = 0;
    for val in map.values() {
        if val == &value {
            count += 1;
        }
    }
    count
}

fn count_iterator(map: &HashMap<String, Progress>, value: Progress) -> usize {
    // map is a hashmap with String keys and Progress values.
    // map = { "variables1": Complete, "from_str": None, ... }
}

fn count_collection_for(collection: &[HashMap<String, Progress>], value: Progress) -> usize {
    let mut count = 0;
    for map in collection {
        for val in map.values() {
            if val == &value {
                count += 1;
            }
        }
    }
    count
}

fn count_collection_iterator(collection: &[HashMap<String, Progress>], value: Progress) -> usize {
    // collection is a slice of hashmaps.
    // collection = [{ "variables1": Complete, "from_str": None, ... },
    //     { "variables2": Complete, ... }, ... ]
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn count_complete() {
        let map = get_map();
        assert_eq!(3, count_iterator(&map, Progress::Complete));
    }

    #[test]
    fn count_equals_for() {
        let map = get_map();
        assert_eq!(
            count_for(&map, Progress::Complete),
            count_iterator(&map, Progress::Complete)
        );
    }

    #[test]
    fn count_collection_complete() {
        let collection = get_vec_map();
        assert_eq!(
            6,
            count_collection_iterator(&collection, Progress::Complete)
        );
    }

    #[test]
    fn count_collection_equals_for() {
        let collection = get_vec_map();
        assert_eq!(
            count_collection_for(&collection, Progress::Complete),
            count_collection_iterator(&collection, Progress::Complete)
        );
    }

    fn get_map() -> HashMap<String, Progress> {
        use Progress::*;

        let mut map = HashMap::new();
        map.insert(String::from("variables1"), Complete);
        map.insert(String::from("functions1"), Complete);
        map.insert(String::from("hashmap1"), Complete);
        map.insert(String::from("arc1"), Some);
        map.insert(String::from("as_ref_mut"), None);
        map.insert(String::from("from_str"), None);

        map
    }

    fn get_vec_map() -> Vec<HashMap<String, Progress>> {
        use Progress::*;

        let map = get_map();

        let mut other = HashMap::new();
        other.insert(String::from("variables2"), Complete);
        other.insert(String::from("functions2"), Complete);
        other.insert(String::from("if1"), Complete);
        other.insert(String::from("from_into"), None);
        other.insert(String::from("try_from_into"), None);

        vec![map, other]
    }
}
```

这是对哈希map键值相等的计数。程序已经通过循环实现过，现在要求我们使用迭代器再实现一遍。

这里的参数是哈希map，通过引用的方式来调用。所以闭包的参数为|&(_, v)|。我们无需关心name，只需关心对应的值即可。而判断是否和传入的参数相等的语句为

```rust
*v == value;
```

所以闭包为

```rust
map.iter().filter(|&(_, v)| *v == value).count()
```

调用map重新建立一个迭代器，以哈希map为参数，判断键值是否和函数的参数相等，并采用count计数。这一小段逻辑被封装，作为匿名函数的定义。

下面count_collection_iterator()函数是异曲同工，但是传入的参数不一样，多了一层壳。这次我们需要一个组合器来通过设置筛选条件进行过滤，即filter()方法。

最后闭包函数为

```rust
ollection.iter().map(|x| x.iter().filter(|&(_, v)| *v == value).count()).sum()
```

调用map，以x为参数，是数组的索引值，再对x创造一个迭代器，访问哈希map，设置判断条件，并通过filter()设置过滤条件，最后用sum进行计数。

完成。

目前并没有提到涉及到泛型的迭代器，这个后续提到的时候再详细说，因为过于复杂。


# thread

## thread1

```rust
// threads1.rs
// Make this compile! Execute `rustlings hint threads1` for hints :)
// The idea is the thread spawned on line 22 is completing jobs while the main thread is
// monitoring progress until 10 jobs are completed. Because of the difference between the
// spawned threads' sleep time, and the waiting threads sleep time, when you see 6 lines
// of "waiting..." and the program ends without timing out when running,
// you've got it :)


use std::sync::Arc;
use std::thread;
use std::time::Duration;

struct JobStatus {
    jobs_completed: u32,
}

fn main() {
    let status = Arc::new(JobStatus { jobs_completed: 0 });
    let status_shared = status.clone();
    thread::spawn(move || {
        for _ in 0..10 {
            thread::sleep(Duration::from_millis(250));
            status_shared.jobs_completed += 1;
        }
    });
    while status.jobs_completed < 10 {
        println!("waiting... ");
        thread::sleep(Duration::from_millis(500));
    }
}
```

在大部分现代操作系统中，已执行程序的代码在一个进程(process)中运行，操作系统则负责管理多个进程。在程序内部，也可以拥有多个同时运行的独立部分。运行这些独立部分的功能被称为线程(threads)。

将程序中的计算拆分进多个线程可以改善性能，因为程序可以同时进行多个任务，不过这也会增加复杂性。因为线程是同时运行的，所以无法预先保证不同线程中的代码的执行顺序。这会导致诸如此类的问题：

* 竞争状态：多个线程以不一致的顺序访问数据或资源
* 死锁：两个线程相互等待对方停止使用其所拥有的资源，这会阻止它们继续运行

在Rust中，为了创建一个新线程，需要调用thread::spawn函数并传递一个闭包，并在其中包含希望在新线程运行的代码。

如上面的题目，代码中开启了原子指针计数，开始计数后开启线程，加入了move闭包，作用就是强制闭包获取其使用的值的所有权，而不是任由 Rust 推断它应该借用值。之后thread::sleep调用强制线程停止执行一小段时间，这会允许其他不同的线程运行。这些线程可能会轮流运行，不过并不保证如此。这依赖操作系统如何调度线程。

目前这个多线程是不安全的。编译会无法通过。因为多个线程共用一个status，并且还在线程间共享。我们不能通过共享内存来通讯。所以要添加互斥器(mutex)，一次只允许一个线程访问数据。线程首先需要通过获取互斥器的锁(lock)来表明其希望访问数据。锁是一个作为互斥器一部分的数据结构，它记录谁有数据的排他访问权。因此，我们描述互斥器为通过锁系统保护其数据。

所以我们在use引入路径后，使用关联函数新建一个Mutex<T>，对共享的status用关联函数new新建一个，并通过lock方法获取锁，以访问互斥器中的数据。这个调用会阻塞当前线程，直到我们拥有锁为止。

```rust
use std::sync::Arc;
use std::sync::Mutex;
use std::thread;
use std::time::Duration;

struct JobStatus {
    jobs_completed: u32,
}

fn main() {
    let status = Arc::new(Mutex::new(JobStatus { jobs_completed: 0 }));
    let status_shared = status.clone();
    thread::spawn(move || {
        for _ in 0..10 {
            thread::sleep(Duration::from_millis(250));
            status_shared.lock().unwrap().jobs_completed += 1;
        }
    });
    while status.lock().unwrap().jobs_completed < 10 {
        println!("waiting... ");
        thread::sleep(Duration::from_millis(500));
    }
}
```

如果另一个线程拥有锁，并且那个线程panic了，则lock调用会失败。在这种情况下，没人能够再获取锁，所以这里选择unwrap并在遇到这种情况时使线程panic！。

关于线程更高级的用法，会在后面rcore的部分进行说明。

# macros

## macros1

```rust
// macros1.rs
// Make me compile! Execute `rustlings hint macros1` for hints :)


macro_rules! my_macro {
    () => {
        println!("Check out my macro!");
    };
}

fn main() {
    my_macro();
}

```

这个题目的问题是调用宏的时候没加！，现在这样是相当于调用名为my_macro()的函数。只要加个!就能解决。

但是我们也详细说说macro_rules!，这个将来会被淘汰的宏类型。

我们已经用过很多种宏了。但是还没完全探索什么是宏以及它是如何工作的。

宏(Macro)指的是Rust中一系列的功能：声明(Declarative)宏，使用 macro_rules!，和三种过程(Procedural)宏：

* 自定义#[derive]宏: 在结构体和枚举上指定通过derive属性添加的代码
* 类属性(Attribute)：宏定义可用于任意项的自定义属性
* 类函数宏：看起来像函数不过作用于作为参数传递的token

从根本上来说，宏是一种为写其他代码而写代码的方式，即所谓的元编程。元编程对于减少大量编写和维护的代码是非常有用的，它也扮演了函数的角色。但宏有一些函数所没有的附加能力。一个函数标签必须声明函数参数个数和类型。相比之下，宏只接受一个可变参数：用一个参数调用println!("hello")或用两个参数调用println!("hello {}", name)。而且，宏可以在编译器翻译代码前展开，例如，宏可以在一个给定类型上实现trait。而函数则不行，因为函数是在运行时被调用，同时trait需要在编译时实现。

实现一个宏而不是函数的消极面是宏定义要比函数定义更复杂，因为你正在编写生成Rust代码的Rust代码。由于这样的间接性，宏定义通常要比函数定义更难阅读、理解以及维护。

宏和函数的最后一个重要的区别是：在调用宏之前 必须定义并将其引入作用域，而函数则可以在任何地方定义和调用。

Rust最常用的宏形式是声明宏(macro_rules!)。声明宏允许我们编写一些类似Rust match表达式的代码。match表达式是控制结构，其接收一个表达式，与表达式的结果进行模式匹配，然后根据模式匹配执行相关代码。宏也将一个值和包含相关代码的模式进行比较；此种情况下，该值是传递给宏的Rust源代码字面值，模式用于和传递给宏的源代码进行比较，同时每个模式的相关代码则用于替换传递给宏的代码。所有这一切都发生于编译时。

我们以vec!宏为例。

```rust
let v: Vec<u32> = vec![1, 2, 3];
```

下面是vec!稍微简化版的定义

```rust
#[macro_export]
macro_rules! vec {
    ( $( $x:expr ),* ) => {
        {
            let mut temp_vec = Vec::new();
            $(
                temp_vec.push($x);
            )*
            temp_vec
        }
    };
}
```

标准库中实际定义的 vec! 包括预分配适当量的内存的代码。这部分为代码优化，为了让示例简化，此处并没有包含在内。

无论何时导入定义了宏的包，#[macro_export]注解说明宏应该是可用的。如果没有该注解，这个宏不能被引入作用域。接着使用macro_rules!和宏名称开始宏定义，且所定义的宏并**不带**感叹号。名字后跟随着的大括号表示宏定义体，在该例中宏名称是vec。

vec!宏的结构和match表达式的结构类似。此处有一个单边模式($($x:expr),* ) ，后跟 => 以及和模式相关的代码块。如果模式匹配，该相关代码块将被执行。假设这是这个宏中唯一的模式，则只有这一种有效匹配，其他任何匹配都是错误的。更复杂的宏会有多个单边模式。

首先，一对括号包含了全部模式。接下来是后跟一对括号$()，其通过替代代码捕获了符合括号内模式的值。括号内则是 $x:expr，其匹配 Rust的任意表达式或给定 $x名字的表达式。

$()之后的逗号说明一个逗号分隔符可以有选择的出现代码之后，这段代码与在 $()中所捕获的代码相匹配。紧随逗号之后的 * 说明该模式匹配零个或多个 * 之前的任何模式。

当以vec![1, 2, 3]; 调用宏时，$x模式与三个表达式 1、2 和 3 进行了三次匹配。

在 $()* 部分中所生成的temp_vec.push()为在匹配到模式中的 $() 每一部分而生成。 $x 由每个与之相匹配的表达式所替换。当以vec![1, 2, 3]; 调用该宏时，替换该宏调用所生成的代码会是下面这样：

```rust
let mut temp_vec = Vec::new();
temp_vec.push(1);
temp_vec.push(2);
temp_vec.push(3);
temp_vec
```

## macros2

```rust
fn main() {
    my_macro!();
}

macro_rules! my_macro {
    () => {
        println!("Check out my macro!");
    };
}
```

这道题的问题所在是作用域。之前已经提过了。

```rust
macro_rules! my_macro {
    () => {
        println!("Check out my macro!");
    };
}

fn main() {
    my_macro!();
}
```

编译通过。

## macros3

```rust
  
// macros3.rs
// Make me compile, without taking the macro out of the module!
// Execute `rustlings hint macros3` for hints :)
mod macros {
    macro_rules! my_macro {
        () => {
            println!("Check out my macro!");
        };
    }
}

fn main() {
    my_macro!();
}

```

还是作用域的问题。这次是封装在mod里。所以我们要在mod前添加上属性#[macro_use]即可。

## macros4

```rust
// macros4.rs
// Make me compile! Execute `rustlings hint macros4` for hints :)

// I AM NOT DONE

macro_rules! my_macro {
    () => {
        println!("Check out my macro!");
    }
    ($val:expr) => {
        println!("Look at this other macro: {}", $val);
    }
}

fn main() {
    my_macro!();
    my_macro!(7777);
}
```

不同的匹配模式后要加分号。

# quiz4

```rust
// quiz4.rs
// This quiz covers the sections:
// - Modules
// - Macros

// Write a macro that passes the quiz! No hints this time, you can do it!

// I AM NOT DONE

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_my_macro_world() {
        assert_eq!(my_macro!("world!"), "Hello world!");
    }

    #[test]
    fn test_my_macro_goodbye() {
        assert_eq!(my_macro!("goodbye!"), "Hello goodbye!");
    }
}

```

这道题就是让我们自己写一个可实现的宏。根据测试代码可知，宏名为my_macro，作用是将输入的字符串拼接到"Hello "后。

所以代码如下：

```rust
macro_rules! my_macro {
    ($val:expr) => {
        "Hello ".to_string() + $val
    };
}

```

编译通过。

更多的宏的类型我们会在实战涉及到。
