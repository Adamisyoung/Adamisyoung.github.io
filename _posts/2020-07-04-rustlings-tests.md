---
layout: post
title:  rustlings-test
date:   2021-07-04 19:28:31 +0800
categories:
- Rust/rustlings
---

> rustlings系列题解。

# test1
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
"Program testing can be a very effective way to show the presence of bugs, but it is hopelessly inadequate for showing their absence.”

程序的正确性意味着代码如我们期望的那样运行。Rust是一个相当注重正确性的编程语言，不过正确性是一个难以证明的复杂主题。Rust的类型系统在此问题上下了很大的功夫，不过它不可能捕获所有种类的错误。为此，Rust也在语言本身包含了编写软件测试的支持。

我们之前见过的各自test，现在终于能实际碰触到了。

Rust中的测试就是一个带有test属性注解的函数。属性是关于Rust代码片段的元数据，我们之前接触的derive就是属性的一个例子。为了将一个函数变成测试函数，需要在fn行之前加上#[test]

我们测试后需要结果时，就需要借助assert!宏了。assert!宏由标准库提供，在希望确保测试中一些条件为true时非常有用。需要向assert!宏提供一个求值为布尔值的参数。如果值是true，assert!什么也不做，同时测试会通过。如果值为false，assert!调用panic!宏，会导致测试失败。

在测试相等的时候就使用使用assert_eq!和 assert_ne!。这两个宏分别比较两个值是相等还是不相等。当断言失败时他们也会打印出这两个值具体是什么，以便于观察测试为什么失败。在一些语言和测试框架中，断言两个值相等的函数的参数叫做expected和actual，而且指定参数的顺序是很关键的。然而在Rust中，它们则叫做left和right。

我们还能注意到测试时也会有use super::*;。它遵循之前提过的可见性规则，将外部模块引入到内部模块的作用域中。

当然，也有一些别扭的情况。比如我们希望代码能按照期望处理错误，即让代码失败。这就可以通过对函数增加另一个属性should_panic来实现这些。这个属性在函数中的代码panic时会通过，而在其中的代码没有panic时失败。

扯远了，题目中就是assert!()缺少参数，加个true就好了。

# test2
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

# test3



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
