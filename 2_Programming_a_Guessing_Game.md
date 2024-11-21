# 第二章 编写一个猜数字游戏

让我们通过动手实现一个小程序来深入了解Rust！这个章节通过如何在一个真正的程序中使用他们来向你介绍Rust中的几个常见的概念。你将会学到`let`，`match` ，方法，关联函数外部crates等等。在接下来的章节中，我们将会详细的探究这些想法。在本章，你将仅是练习基础知识。

我们将会实现一个经典的编程初学者问题：一个猜数字的游戏。这是它的工作原理：

* 程序生成一个1到100之间的随机数字
* 然后提示玩家输入自己猜测的数字
* 当用户猜测的数字输入之后，程序将表明用户的输入是否更大或者更小
* 如果用户猜测的值是正确的，程序将会打印一句祝贺语然后退出

## 建立一个新的项目

要建立一个新的项目，进入在第一章节中你创建的项目文件夹，然后使用Cargo生成一个新的项目，比如：

```shell
cargo new guessing_game
cd guessing_game
```

第一个命令，`cargo new` ，传入项目的名字（guessing_game）作为第一个参数，第二个命令改变（工作目录）到新的项目文件夹中。
可以看到生成的`Cargo.toml`文件

```toml
[package]
name = "guessing_game"
version = "0.1.0"
edition = "2021"

# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

[dependencies]
```

正如你在第一章节中看到的，`cargo new` 为你生成了一个 “Hello World” 程序，查看src/main.rs文件：

```rust
fn main() {
    println!("Hello, world!");
}
```

现在，让我们使用`cargo run`命令编译这个 “Hello World” 程序，然后同时运行它：

```txt
cargo run
   Compiling guessing_game v0.1.0 (/Users/varus/workdir/github.com/varushsu/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 1.29s
     Running `target/debug/guessing_game`
Hello, world!
```

当你需要快速迭代项目时，`run` 命令将会派上用场，正如这个游戏项目一样，在下次迭代之前快速的测试当前的迭代。
重新打开`src/main.rs` 文件，你将在这个文件写所有的代码。

## 处理猜数

程序的第一部分将会寻求用户的输入，处理用户的输入，检查用户输入的格式。首先，我们将允许用户输入猜测的值。将下面的代码写入`src/main.rs` 中：

```rust
use std::io;

fn main() {
    println!("Guess the number!");
    println!("Please input your guess.");
    
    let mut guess = String::new();
    
    io::stdin()
        .read_line(&mut guess)
        .expect("Failed to read line");
        
    println!("You guessed: {}", guess);
}
```

这段代码包含了许多信息，我们一行一行地梳理一下。为了获取用户的输入和打印结果作为输出，我们需要将`io` 输入/输出库纳入作用域中。`io` 库来自于标准库，被称为`std` :

```rust
use std::io;
```

默认情况下，Rust 的标准库中定义了一组项目，并将它们带入了每个程序的作用域内。该组被称之为`prelude` 。并且你可以在标准库文档中参阅到每件内容。
如果你想要使用一个不在`prelude` 中的类型，你必须显示地使用`use` 声明该类型到作用域中。`std::io` 提供给你包含接受用户输入能力等许多有用的特征。

正如你在第一章节看到的，main 函数是程序的入口点。

```rust
fn main {
```

`fn` 语法定义了一个新的函数，括号`()` 表明没有入参；花括号`{` 表示函数体的开始。
正如你在第一章节中学到的`println!`是一个打印字符串到控制台上的宏
