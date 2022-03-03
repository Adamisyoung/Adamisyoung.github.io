---
layout: post
title:  "rustlings-迭代器和闭包"
date:   2021-07-04 19:32:31 +0800
categories:
- Rust/rustlings
---

> rustlings系列题解

# iterators1
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


# iterators2
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

# iterators3
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

# iterators4
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

# iterators5
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
