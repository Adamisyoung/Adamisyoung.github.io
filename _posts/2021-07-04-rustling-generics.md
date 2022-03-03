---
layout: post
title:  rustlings-generics
date:   2021-07-04 19:20:31 +0800
categories:
- Rust/rustlings
---

> rustlings系列题解

# generics1
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

# generics2
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

# generics3
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
