---
layout: post
title:  rustlings-move_semantics
date:   2021-07-04 00:16:31 +0800
categories:
- Rust/rustlings
---

> rustlings系列题解

# move_semantics1
```rust
// move_semantics1.rs
// Make me compile! Execute `rustlings hint move_semantics1` for hints :)

fn main() {
    let vec0 = Vec::new();

    //let v = vec![1, 2, 3];
    let mut v = vec![1, 2, 3];
    println!("{} has length {} content `{:?}`", "vec1", vec1.len(), vec1);

    vec1.push(88);

    println!("{} has length {} content `{:?}`", "vec1", vec1.len(), vec1);
}

fn fill_vec(vec: Vec<i32>) -> Vec<i32> {
    let mut vec = vec;

    vec.push(22);
    vec.push(44);
    vec.push(66);
    vec
}
```
Rust标准库中包含一系列被称为集合(collections)的非常有用的数据结构。大部分其他数据类型都代表一个特定的值，不过集合可以包含多个值。不同于内建的数组和元组类型，这些集合指向的数据是储存在堆上的，这意味着数据的数量不必在编译时就已知，并且还可以随着程序的运行增长或缩小。每种集合都有着不同功能和成本，而根据当前情况选择合适的集合，这是一项始终成长的技能。

有三个被广泛使用的集合：
* vector：允许我们一个挨着一个地储存一系列数量可变的值；
* string：字符的集合；
* 哈希map：允许我们将值与一个特定的键值(key)相关联。这是一个叫做map的更通用的数据结构的特定实现

创建一个Vector：
```rust
let v: Vec<i32> = Vec::new();
```
加了类型注解却未加入任何值，现在Rust并不知道我们储存什么元素。这就是泛型的实现。其实我们很少需要类型注解。所以为了方便，Rust提供了vec!宏，这个宏会根据我们提供的值来创建一个新的Vec。
```rust
let v = vec![1, 2, 3];
```
上题中，创建了一个未知类型的Vec，但是不报错。因为在下一行，该Vec就作为fill_vec函数的实参传入，而该函数形参类型是i32，返回的是Vec<i32>。所以编译器自动推断出类型。上面代码的问题就在改变Vec上。缺少一个mut。改变Vec也是需要mut的。更新Vec就是push()。


# move_semantics2
```rust
// move_semantics2.rs
// Make me compile without changing line 13!
// Execute `rustlings hint move_semantics2` for hints :)

fn main() {
    let vec0 = Vec::new();

    let mut vec1 = fill_vec(vec0);

    // Do not change the following line!
    println!("{} has length {} content `{:?}`", "vec0", vec0.len(), vec0);

    vec1.push(88);

    println!("{} has length {} content `{:?}`", "vec1", vec1.len(), vec1);
}

fn fill_vec(vec: Vec<i32>) -> Vec<i32> {
    let mut vec = vec;

    vec.push(22);
    vec.push(44);
    vec.push(66);

    vec
}

```
这里涉及到借用权变更的问题。一旦程序获取了一个有效的引用，借用检查器将会执行所有权和借用规则来确保vector内容的这个引用和任何其他引用保持有效。

所有权系统是Rust最为与众不同的特性，它让Rust无需垃圾回收(garbage collector)即可保障内存安全。因此，理解Rust中所有权如何工作是十分重要的。

所有运行的程序都必须管理其使用计算机内存的方式。一些语言中具有垃圾回收机制，在程序运行时不断地寻找不再使用的内存；在另一些语言中，程序员必须亲自分配和释放内存。Rust则选择了第三种方式：通过所有权系统管理内存，编译器在编译时会根据一系列的规则进行检查。在运行时，所有权系统的任何功能都不会减慢程序。

栈和堆不需要多说，前者中所有数据都必须占用已知且固定的大小。在编译时大小未知或大小可能变化的数据，要改为存储在堆上。当向堆放入数据时，你要请求一定大小的空间。操作系统在堆的某处找到一块足够大的空位，把它标记为已使用，并返回一个表示该位置地址的指针。将数据推入栈中并不被认为是分配。因为指针的大小是已知并且固定的，你可以将指针存储在栈上，不过当需要实际数据时，必须访问指针。访问堆上的数据比访问栈上的数据慢，因为必须通过指针来访问。跟踪哪部分代码正在使用堆上的哪些数据，最大限度的减少堆上的重复数据的数量，以及清理堆上不再使用的数据确保不会耗尽空间，这些问题正是所有权系统要处理的。

从根本上来说，所有权的存在就是为了管理堆数据。

所有权三条规则：
* Rust中的每一个值都有一个被称为其所有者(owner)的变量。
* 值有且只有一个所有者。
* 当所有者(变量)离开作用域，这个值将被丢弃。
作用域在前面已经提过了。
```rust
{                      //s在这里无效,它尚未声明
    let s = "hello";   //从此处起，s是有效的

    //使用 s
}                      //此作用域已结束，s不再有效
```
当s进入作用域，它是有效的，一直持续到离开作用域为止。到这里为止，和其他编程语言都没什么区别。接下来我们借用一个能存储在堆上的数据结构来摸索一下。

我们已经见过字符串字面值，字符串值被硬编码进程序里，这是很方便的。不过他们并不适合使用文本的每一种场景。原因之一就是他们是不可变的。另一个原因是并不是所有字符串的值都能在编写代码时就知道：例如，要是想获取用户输入并存储，为此，Rust有第二个字符串类型，即String。这个类型被分配到堆上，所以能够存储在编译时未知大小的文本。可以使用from函数基于字符串字面值来创建String，如下：
```rust
let mut s = String::from("hello");
s.push_str(", world!"); //push_str()在字符串后追加字面值
println!("{}", s); //将打印 `hello, world!`
```
上面的例子中，我们可以发现，String是可变的。这和硬编码的字面值形成鲜明对比。就字符串字面值来说，我们在编译时就知道其内容，所以文本被直接硬编码进最终的可执行文件中。这使得字符串字面值快速且高效。不过这些特性都只得益于字符串字面值的不可变性。不幸的是，我们不能为了每一个在编译时大小未知的文本而将一块内存放入二进制文件中，并且它的大小还可能随着程序运行而改变。

而对于String类型，为了支持一个可变，可增长的文本片段，需要在堆上分配一块在编译时未知大小的内存来存放内容。这意味着：
* 必须在运行时向操作系统请求内存
* 需要一个当我们处理完String时将内存返回给操作系统的方法
  
第一部分即当调用String::from时，它的实现请求其所需的内存。这在编程语言中是非常通用的。然而，第二部分实现起来就各有区别了。在有垃圾回收(GC)的语言中，GC记录并清楚不再使用的内存，而我们并不需要关心它。没有GC的话，识别出不再使用的内存并调用代码显式释放就是我们的责任了，跟请求内存的时候一样。从历史的角度上说正确处理内存回收曾经是一个困难的编程问题。如果忘记回收了会浪费内存。如果过早回收了，将会出现无效变量。如果重复回收，这也是个bug。我们需要精确的为一个allocate配对一个free。

Rust采取了一个不同的策略：内存在拥有它的变量离开作用域后就被自动释放。上面的例子中，当变量离开作用域，Rust为我们调用一个特殊的函数。这个函数叫做drop，在drop里String的作者可以放置释放内存的代码。Rust在结尾的 } 处自动调用了drop。这种机制在C++中，生命周期结束释放资源的模式称为RAII。这个模式对编写 Rust 代码的方式有着深远的影响。

变量和数据有多种交互方式。
```rust
let x = 5;
let y = x;
```
这里是将5绑定到x；接着生成一个值x的拷贝并绑定到y。因为整数是有已知固定大小的简单值，所以这两个5被放入了栈中。而下面的这个版本
```rust
let s1 = String::from("hello");
let s2 = s1;
```
这看起来与上面的代码非常类似，但事实上并不完全是这样。String是由三部分组成：一个指向存放字符串内容内存的指针，一个长度，和一个容量。这一组数据是存储在栈上。右侧则是堆上存放内容的内存部分。长度表示String的内容当前使用了多少字节的内存。容量是String从操作系统总共获取了多少字节的内存。长度与容量的区别是很重要的，不过在当前上下文中并不重要，所以现在可以忽略容量。当我们将s1赋值给s2，String的数据被复制了，这意味着我们从栈上拷贝了它的指针、长度和容量。我们并没有复制指针指向的堆上数据。则这就意味着**两个变量同时指向堆上同一处内存**。之前我们提到过当变量离开作用域后，Rust自动调用drop函数并清理变量的堆内存。则当s2和s1离开作用域，他们都会尝试释放相同的内存(double free)。

所以，为了确保内存安全，与其尝试拷贝被分配的内存，Rust则认为s1不再有效，因此Rust不需要在s1离开作用域后清理任何东西。
```rust
let s1 = String::from("hello");
let s2 = s1;

println!("{}, world!", s1);
```
上面这段代码会报错，因为s1已经是无效的引用了。这种行为被称之为**Move**。浅拷贝的一种。这里隐含了一个设计选择：Rust永远也不会自动创建数据的**深拷贝**，任何自动的复制都可以被认为运行时性能影响最小。

让我们回到上面这道题。

vec0传入函数后，浅拷贝到了函数内部的Vec中，则vec0即失效。当我们再调用vec0.len()的时候就会报错。此时vec1是新的owner，这中间发生了所有权的转移。

所以解决方法就是由浅拷贝改为**深拷贝**如果我们确实需要深度复制堆上的数据，而不仅仅是栈上的数据，可以使用一个叫做 clone 的通用函数。这中间也有一些不太寻常的事情。

让我们回到上面的例子中
```rust
let x = 5;
let y = x;
println!("x = {}, y = {}", x, y);
```
这段代码似乎没有调用clone，但x和y都有效。原因是像整型这样的在编译时已知大小的类型被整个存储在栈上，所以拷贝其实际的值是快速的。这意味着没有理由在创建变量y后使x无效。换句话说，这里没有深浅拷贝的区别，所以这里调用clone并不会与通常的浅拷贝有什么不同。Rust有一个叫做Copy trait的特殊注解，可以用在类似整型这样的存储在栈上的类型上：如果一个类型拥有Copy trait，则一个旧的变量在将其赋值给其他变量后仍然可用。但如果想在变量离开作用域的时候使用还是会出错的。任何简单标量值的组合可以是Copy的，即不需要分配内存或某种形式资源的类型是Copy的。具体类别如下：
* 所有整数类型，比如u32
* 布尔类型，bool，它的值是true和false
* 所有浮点数类型，比如f64
* 字符类型，char
* 元组，当且仅当其包含的类型也都是Copy的时候。比如(i32, i32)是Copy的，但(i32, String)就不是

变量的所有权总是遵循相同的模式：将值赋给另一个变量时移动它。当持有堆中数据值的变量离开作用域时，其值将通过drop被清理掉，除非数据被移动为另一个变量所有。在每一个函数中都获取所有权并接着返回所有权有些啰嗦。如果我们想要函数使用一个值但不获取所有权该怎么办呢？如果我们还要接着使用它的话，每次都传进去再返回来就有点烦人了，除此之外，我们也可能想返回函数体中产生的一些数据。所以Rust提供了**引用**。这是后话。

修正过的move_semantics
```rust
fn main() {
    let vec0 = Vec::new();
    let mut vec1 = fill_vec(vec0.clone());
    //vec1是新owner
    //让move变为clone(深拷贝)
    // Do not change the following line!
    println!("{} has length {} content `{:?}`", "vec0", vec0.len(), vec0);
    vec1.push(88);

    println!("{} has length {} content `{:?}`", "vec1", vec1.len(), vec1);
}

fn fill_vec(vec: Vec<i32>) -> Vec<i32> {
    let mut vec = vec;
    //把一个变量赋值给另一个变量会把所有权(ownership)转移给受让者
    vec.push(22);
    vec.push(44);
    vec.push(66);
    vec
}

```
# move_semantics3
```rust
// move_semantics3.rs
// Make me compile without adding new lines-- just changing existing lines!
// (no lines with multiple semicolons necessary!)
// Execute `rustlings hint move_semantics3` for hints :)


fn main() {
    let vec0 = Vec::new();

    let mut vec1 = fill_vec(vec0);

    println!("{} has length {} content `{:?}`", "vec1", vec1.len(), vec1);

    vec1.push(88);

    println!("{} has length {} content `{:?}`", "vec1", vec1.len(), vec1);
}

//fn fill_vec(vec: Vec<i32>) -> Vec<i32> {
fn fill_vec(mut vec: Vec<i32>) -> Vec<i32> {    
    vec.push(22);
    vec.push(44);
    vec.push(66);

    vec
}
```
这里的问题是，在调用函数后，我们对函数的返回值进行了修改(push(88))，函数参数并未包含可变mut，所以会报错。为了添加变量可变性，就在vec前加上mut即可。如果放在冒号后面，mut修饰的就是类型，仅仅是用在可变引用&mut T和裸指针*mut T上。这个mut无法影响传入变量的可变性。

# move_semantics4
```rust
// move_semantics4.rs
// Refactor this code so that instead of having `vec0` and creating the vector
// in `fn main`, we create it within `fn fill_vec` and transfer the
// freshly created vector from fill_vec to its caller.
// Execute `rustlings hint move_semantics4` for hints!

// I AM NOT DONE

fn main() {
    let vec0 = Vec::new();

    let mut vec1 = fill_vec(vec0);

    println!("{} has length {} content `{:?}`", "vec1", vec1.len(), vec1);

    vec1.push(88);

    println!("{} has length {} content `{:?}`", "vec1", vec1.len(), vec1);
}

// `fill_vec()` no longer takes `vec: Vec<i32>` as argument
fn fill_vec() -> Vec<i32> {
    let mut vec = vec;

    vec.push(22);
    vec.push(44);
    vec.push(66);

    vec
}

```
大部分时候当涉及到泛型时，编译器可以自动推断出泛型参数，但是这里因为fill_vec函数不再传参，所以第一行类型就无法推断出来，编译器会报错。这道题的意思就是构造Vec的工作放在函数内进行。答案如下：
```rust
fn main() {
    let mut vec1 = fill_vec();

    println!("{} has length {} content `{:?}`", "vec1", vec1.len(), vec1);

    vec1.push(88);

    println!("{} has length {} content `{:?}`", "vec1", vec1.len(), vec1);
}

// `fill_vec()` no longer takes `vec: Vec<i32>` as argument
fn fill_vec() -> Vec<i32> {
    let mut vec = Vec::new();
    vec.push(22);
    vec.push(44);
    vec.push(66);

    vec
}

```


第五题后续添加....
