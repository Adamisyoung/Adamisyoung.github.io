---
layout: post
title:  rustlings-thread
date:   2021-07-04 19:36:31 +0800
categories:
- Rust/rustlings
---

> rustlings系列题解

# thread1
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