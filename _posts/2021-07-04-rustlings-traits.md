---
layout: post
title:  rustlings-traits
date:   2021-07-04 19:26:31 +0800
categories:
- Rust/rustlings
---

> rustlings系列题解

# traits1
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

# traits2
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

