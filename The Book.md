# The Book

## CAP 2

### 猜数字游戏

```
[package]
name = "my_test"
version = "0.1.0"
edition = "2021"

[dependencies]
rand = "0.8.5"
```

```rust
use rand::Rng;

fn main() {
    println!("Guess the number!");

    let secret_number: u32 = rand::thread_rng().gen_range(1..100);
    //println!("The secret number is: {secret_number}");

    loop {
        println!("Please input your guess.");

        let mut guess: String = String::new();

        std::io::stdin()
            .read_line(&mut guess)
            .expect("Failed to read line");

        //let guess = guess.trim().parse::<u32>().expect("Please type a number!");
        let guess = match guess.trim().parse::<u32>(){
            Ok(num) => num,
            Err(_) => continue,
        };

        println!("You guessed: {}", guess);

        match guess.cmp(&secret_number) {
            std::cmp::Ordering::Less => println!("Too small!"),
            std::cmp::Ordering::Greater => println!("Too big!"),
            std::cmp::Ordering::Equal => {
                println!("You win!");
                break;
            },
        }
    }
}
```

## CAP 3

### 3.1 变量, 常量

```rust
fn main() {
  	// let 用于声明变量, 变量默认不可变
  	let x: i32 = 5;
  
    // mut 使得变量可变
    let mut x: i32 = 5;
		x += 1;
}
```

```rust
// 必须注明 ‘类型’
// 常量可以在 ‘任何作用域’ 中声明
// 常量只能被设置为 ‘常量表达式’ ，不能是运行时计算出的值
// 常量名全部大写, 以下划线连接单词
const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3;
```

### 3.2 数据类型

#### 标量

整型 浮点型 布尔类型 字符类型

##### 整型

| 长度    | 有符号  | 无符号  |
| ------- | ------- | ------- |
| 8-bit   | `i8`    | `u8`    |
| 16-bit  | `i16`   | `u16`   |
| 32-bit  | `i32`   | `u32`   |
| 64-bit  | `i64`   | `u64`   |
| 128-bit | `i128`  | `u128`  |
| arch    | `isize` | `usize` |

| 数字字面值                    | 例子          |
| ----------------------------- | ------------- |
| Decimal (十进制)              | `98_222`      |
| Hex (十六进制)                | `0xff`        |
| Octal (八进制)                | `0o77`        |
| Binary (二进制)               | `0b1111_0000` |
| Byte (单字节字符)(仅限于`u8`) | `b'A'`        |

所有模式下都可以使用 `wrapping_*` 方法进行 wrapping，如 `wrapping_add`

checked_*: 出现溢出，则返回 `None`值

overflowing_*: 返回 值和布尔值，表示是否出现溢出

saturating_*: 在值的最小值或最大值处进行饱和处理

##### 浮点型

```rust
let numf: f32 = 3.14_f32;
let numf: f64 = 3.14_f64;
```

##### 数值运算

```rust
// + - * / %
// 整数除法向零取整
```

##### 布尔类型

```rust
let tmp: bool = true;
let tmp: bool = false;
```

##### 字符类型

```rust
let c: char = 'a'; // 'char' 占 4 Byte
```

#### 复合类型

元组 数组

##### 元组

```rust
// 元组: 固定长度, 容纳多种类型
fn main() {
    let tup: (u32, i32, f32) = (2, -2, 3.14);
    
    // 1.使用模式匹配解构元组中的元素
    let (x, y, z) = tup;
    println!("'y' is {}", y);

    // 2.使用索引访问元组中的元素
    println!("'y' is {}", tup.1);
  
  	// 不带任何值的元组称为 unit: ()
  	// 类型和值都写作: ()
  	// 表示空值或空返回类型
  	let t: () = ();
    println!("{:?}", t);
}
```

##### 数组

```rust
// 数组: 固定长度, 元素应该为相同类型
fn main() {
    // 数组元素会分配在 ‘栈’
    let a: [i32; 4] = [2, 4, 6, 8];
  	let a: [i32; 4] = [0; 4]; // 分配4个元素, 元素值为0
  
    let idx: usize = 0;
    println!("{}", a[idx]); // 使用 usize 类型作为下标访问数组
}
```

### 3.3 函数

#### 语句, 表达式

函数体由一系列的语句和一个可选的结尾表达式构成

​	语句（Statements）是执行一些操作但不返回值的指令
​	表达式（Expressions）计算并产生一个值

表达式会计算出一个值

​	数学运算 5 + 6 是表达式, 计算出值 11

​	表达式可以是语句的一部分：语句 `let y = 6;` 中的 `6` 是一个表达式，计算出值是 `6`

​	函数调用是一个表达式

​	宏调用是一个表达式。

​	块作用域也是一个表达式，

```rust
let num: i32 = {
    let x: i32 = 0;
    x+1
};
```

```rust
fn foo(x: i32) -> i32 {
    let y: i32 = 1;
    x + y
}
fn main() {
    let num: i32 = foo(0);
}
```

### 3.5 控制流

#### 1.if 表达式

```rust
// 条件必须是 bool 类型
let num: i32 = 6;
if num < 5 {
  	println!("condition was true");
} else {
  	println!("condition was false");
}
```

```rust
// if 是表达式
let condition: bool = false;
let num: i32 = if condition {
    1
} else {
    0
};
```

#### 2. loop 循环

```rust
loop {
    println!("loop");
    // continue;
    // break;
}
```

```rust
// return val
let mut counter = 0;
let val: i32 = loop {
    counter += 1;
    if counter == 10 {
        break counter * 2;
    }
};
```

```rust
// use tag of loop
let mut counter: i32 = 0;
'loop_out: loop {
    let mut tmp: i32 = 0;
    'loop_in: loop {
        tmp += 1;
        if tmp == 10 {
            println!("exit loop_in");
            break;
        }
        if counter == 10 {
            println!("exit loop_out");
            break 'loop_out;
        }
    }
    counter += 1;
}
```

#### 3.while 循环

```rust
let mut counter: i32 = 10;
while counter > 0 {
    counter -= 1;
}
println!("{counter}");
```

#### 4.for 循环

```rust
let nums: [i32; 6] = [0, 2, 4, 6, 8, 10];
for num in nums.iter() {
    println!("{num}");
}
for i in 0..10 {
    println!("{i}");
}
for i in (0..10).rev() {
    println!("{i}");
}
```

## CAP 4

### 1.所有权

Rust 中的每一个值都有且只有一个 **所有者**（*owner*）

当所有者（变量）离开作用域，这个值将被丢弃

```rust
fn main() {
    let x = 5; // copy 类型
    makes_copy(x);

    let s0 = String::from("hello");
    takes_ownership(s0); // move
    
  	let s1 = gives_ownership(); // gives_ownership 将返回值移动到 s1

    let s2 = String::from("hello");     // s2 进入作用域

    let s3 = takes_and_gives_back(s2);  // s2 被移动到 takes_and_gives_back 中，
                                                         // 它也将返回值移给 s3
} // s3 移出作用域并被丢弃 
  // s2 移出作用域, 但已被移走, 什么也不会发生
  // s1 离开作用域并被丢弃
  // s0 移出作用域, 但已被移走, 什么也不会发生

fn makes_copy(t: i32) {
    println!("{t}");
}

fn takes_ownership(some_string: String) { // some_string 进入作用域
    println!("{}", some_string);
}

fn gives_ownership() -> String {             // gives_ownership 会将
                                             // 返回值移动给调用处

    let some_string = String::from("yours"); // some_string 进入作用域。

    some_string                              // 返回 some_string 
                                             // 并移出给调用的函数
}

// takes_and_gives_back 将传入字符串并返回该值
fn takes_and_gives_back(a_string: String) -> String { // a_string 进入作用域

    a_string  // 返回 a_string 并移出给调用的函数
}
```

### 2.引用

```rust
fn main() {
    let mut s1 = String::from("hello");

    change(&mut s1);
    let len = calculate_length(&s1);

    println!("The length of '{s1}' is {len}.");
}

fn calculate_length(s: &String) -> usize { // 引用
    s.len()
}

fn change(s: &mut String) { // 可变引用
    s.push_str(", world!");
}
```

```rust
// 在任意给定时间，要么 只能有一个可变引用，要么 只能有多个不可变引用
// 引用必须总是有效的
// 引用的作用域: [声明, 最后一次使用]
```

### 3.Slice

```rust
fn main() {
    let s = String::from("hello world");
    let r = first_word(&s[0..6]);
    //s.clear();
    println!("{r}");

    let s = "hello world";
    let r = first_word(&s[0..6]);
    println!("{r}");
}

fn first_word(s: &str) -> &str {
    for (idx, c) in s.chars().enumerate() {
        if c == ' ' {
            return &s[0..idx]
        }
    }
    &s[..]
}
```

```rust
fn main() {
    let a: [i32; 6] = [0, 2, 4, 6, 8, 10];
    let a_slice: &[i32] = &a[1..5];

    for num in a_slice.iter() {
        println!("{num}");
    }
}
```

## CAP 5

### 1.结构体的定义和实例化

```rust
fn main() {
    let mut user1: User = User {
        active: true,
        username: String::from("someusername123"),
        email: String::from("someone@example.com"),
        sign_in_count: 1,
    };
    user1.email = String::from("anotheremail@example.com");
  
  	// 使用结构体的字段新建结构体
  	let user2 = User {
        email: String::from("another@example.com"),
        ..user1
    };
  	// use1此时已经不再可用, 因为 user1.username 已经被 move
  
  	let blue: Color = Color( 0, 0, 255 );

    let subject: AlwaysEqual = AlwaysEqual;
}

// struct
struct User {
    active: bool,
    username: String,
    email: String,
    sign_in_count: u64,
  	// 结构体存储被其他对象拥有的数据的引用，需要使用 '生命周期'
}

// 元组结构体
struct Color( u32, u32, u32 );

// unit-like struct
struct AlwaysEqual;
```

### 2.结构体实践

```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}
impl Rectangle {
    fn square(size: u32) -> Self {
        Self {
            width: size,
            height: size,
        }
    }

    // Self: Rectangle
    // &self == self: &Self
    fn area(&self) -> u32 {
        self.width * self.height
    }

    fn can_hold(&self, other: &Rectangle) -> bool {
        self.width > other.width && self.height > other.height
    }
}
fn main() {
    //let scale: u32 = 2;
    let rect1: Rectangle = Rectangle {
        width: 30,
        //width: dbg!(30 * scale),
        height: 50,
    };

    println!("{:?}", rect1);
    println!("{:#?}", rect1);
    dbg!(&rect1);

    println!("The area of the rectangle is {} square pixels",
             area_struct(&rect1));
  
		let rect2 = Rectangle {
        width: 10,
        height: 40,
    };
    let rect3 = Rectangle {
        width: 60,
        height: 45,
    };
        
    println!("Can rect1 hold rect2? {}", rect1.can_hold(&rect2));
    println!("Can rect1 hold rect3? {}", rect1.can_hold(&rect3));

    let square1: Rectangle = Rectangle::square(20_u32);
    println!("square1's area is {}", square1.area());
}
```

## CAP 6

### 6.1 枚举的定义

```rust
enum Message {
  	// 都是结构体, 会发生移动
    Quit, // 单元结构体
    Move { x: i32, y: i32 }, // 结构体
    Write(String,), // 元组结构体
    ChangeColor(i32, i32, i32,),
}
impl Message {
    fn call(&self) {

    }
}
fn foo(msg: Message) {

}
```

```rust
enum Option<T> {
    None,
    Some(T),
}

let some_num: Option<i32> = Some(6);
let some_char: Option<char> = Some('a');
let absent_num: Option<i32> = None;
```

### 6.2 match

```rust
#[derive(Debug)]
enum UsState {
    Alabama,
    Alaska,
}
enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter(UsState,),
}

// match enum
fn value_in_cents(coin: Coin) -> u8 {
    // 每个分支相关联的代码是一个表达式
    // 而表达式的结果值将作为整个 match 表达式的返回值
    match coin {
        Coin::Penny => {
            println!("Lucky penny!");
            1
        },
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter(state) => { // 绑定值的模式
            println!("State quarter from {state:?}!");
            25
        },
    }
}

// match Option<T>
fn plus_one(x: Option<i32>) -> Option<i32> {
    match x {
        Some(i) => Some(i+1),
        None => None,
    }
}

// 通配符
let dice_roll: u8 = 9;
match dice_roll {
    3 => add_fancy_hat(),
    7 => remove_fancy_hat(),
    //other => move_player(other), // 通配模式
    //_ => reroll(), // 占位符
    _ => (),
}
```

### 6.3 if let

```rust
let config_max = Some(3u8);
match config_max {
    Some(max) => println!("The maximum is configured to be {max}"),
    _ => (),
}
// `if let` 语法让我们以一种不那么冗长的方式结合 `if` 和 `let`，来处理只匹配一个模式的值而忽略其他模式的情况
// 可以加上 else
// if 分支也是表达式
let num = Some(6_u8);
if let Some(val) = num {
    println!("{val}");
    8
} else {
    println!("It's None");
    0
};
```

## CAP 7

### 7.1 Package, Crate

```shell
$ cargo new my-project
     Created binary (application) `my-project` package
$ ls my-project
Cargo.toml
src
$ ls my-project/src
main.rs

# Crate: binary crate, library crate
# 		binary crate's root is main.rs
# 		library crate's root is lib.rs
```

### 7.2

```rust
// pub
// 父模块不可以使用子模块的项
// 子模块可以使用父模块的项
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {

        }
    }
}
pub fn eat_at_restaurant1() {
    crate::front_of_house::hosting::add_to_waitlist(); // 绝对路径
    front_of_house::hosting::add_to_waitlist(); // 相对路径
}

// super
mod back_of_house {
    fn fix_incorrect_order() {
        cook_order();
        //crate::deliver_order();
        super::deliver_order();
    }
    fn cook_order() {

    }
}
fn deliver_order() {

}

// pub struct 的字段仍是私有的, 需要加 pub 决定字段是否公有
mod back_of_house {
    pub struct Breakfast {
        pub toast: String,
        seasonal_fruit: String,
    }
    impl Breakfast {
        pub fn summer(toast: &str) -> Breakfast {
            Breakfast {
                toast: String::from(toast),
                seasonal_fruit: String::from("peaches"),
            }
        }
    }
}
pub fn eat_at_restaurant() {
    let mut meal = crate::back_of_house::Breakfast::summer("Rye");
    meal.toast = String::from("Wheat");
    println!("I'd like {} toast please", meal.toast);
}

// pub enum 的成员都是公有的
mod back_of_house {
    pub enum Appetizer {
        Soup,
        Salad,
    }
}
pub fn eat_at_restaurant() {
    let order1 = crate::back_of_house::Appetizer::Soup;
    let order2 = crate::back_of_house::Appetizer::Salad;
}
```

### 7.3 use 关键字引入路径

```rust
// use 关键字引入路径
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {

        }
    }
}
use crate::front_of_house::hosting;
pub fn eat_at_restaurant() {
    
    hosting::add_to_waitlist();
}

// use 将函数的父模块引入作用域, 可以清晰地表明函数不是在本地定义的
use crate::front_of_house::hosting;

// use 引入结构体、枚举和其他项时, 习惯是指定它们的完整路径
use std::collections::HashMap;

// 使用父模块区分同名类型
use std::fmt;
use std::io;
fn function1() -> fmt::Result {
    // --snip--
}

fn function2() -> io::Result<()> {
    // --snip--
}

// 使用 as 关键字区分同名类型
use std::fmt::Result;
use std::io::Result as IoResult;

// pub use
mod restaurant {
    mod front_of_house {
        pub mod hosting {
            pub fn add_to_waitlist() {
                println!("add to waitlist");
            }
        }
    }
    pub use front_of_house::hosting;
}
fn main() {
    use crate::restaurant;
    restaurant::hosting::add_to_waitlist();
}

// 嵌套路径
// example1:
use std::cmp::Ordering;
use std::io;

use std::{cmp::Ordering, io};

// example2:
use std::io;
use std::io::Write;

use std::io::{self, Write};

// golb运算符将所有的共有定义引入作用域
use std::collections::*;
```

### 7.4 拆分文件

```rust
/*
* ./src
* 		main.rs
* 		front_of_house.rs
* 		front_of_house/hosting.rs
*/

// main.rs
mod front_of_house;
use front_of_house::hosting;
fn main() {
    hosting::add_to_waitlist();
}

// front_of_house.rs
pub mod hosting;

// hosting.rs
pub fn add_to_waitlist() {
    println!("add to waitlist");
}
```

## CAP 8

### 8.1 Vec

```rust
// new
let v: Vec<i32> = Vec::<i32>::new();
let mut v: Vec<i32> = vec![1, 2, 3,];

// push
v.push(4);

// get
let first: &i32 = &v[0];
let first: Option<&i32> = v.get(0);
match first {
    Some(val) => println!("The first element is {}", val),
    None => println!("There is no first element."),
}

// error
let first = &v[0];
v.push(6);
println!("{first}"); // error, 不可变引用和可变引用同时存在

// 遍历
for val in &mut v {
    *val *= 10;
}
for val in &v {
    println!("{val}");
}

// enum作为元素可以存储不同类型的值
enum SpreadsheetCell {
    IntElem(i32,),
    FloatElem(f64,),
    StringElem(String,),
}
let row: Vec<SpreadsheetCell> = vec![
        SpreadsheetCell::IntElem(6),
        SpreadsheetCell::FloatElem(3.14),
        SpreadsheetCell::StringElem(String::from("hello"))];

// 当 vector 被丢弃时，所有其内容也会被丢弃，这意味着这里它包含的元素将被清理
// 借用检查器确保了任何 vector 中内容的引用仅在 vector 本身有效时才可用
```

### 8.2 String

```rust
// 初始化
let s = String::new();
let s = String::from("hello");

// 更新

// push_str(&str)
let mut s1: String = String::from("foo");
let s2 = "bar";
s1.push_str(s2);
println!("s1: {s1}, s2: {s2}");

// push(char)
let mut s1: String = String::from("lo");
let c: char = 'l';
s1.push(c);
println!("{s1}");

// +
// fn add(self, s: &str) -> String {...}
let s1: String = String::from("hello, ");
let s2: String = String::from("world.");
let s3 = s1 + &s2[..]; 
println!("s3: {s3}");

// format!
let s1 = String::from("tic");
let s2 = String::from("tac");
let s3 = String::from("toe");

let s = format!("{s1}-{s2}-{s3}");
println!("{s}");

// 内部表现
// String 是一个 Vec<u8> 的封装
// utf-8编码, 字符的编码所需字节不同, rust不支持下标访问

// 获取 &str
let s = String::from("hello, world.");
let s_slice: &str = &s[0..5]; // [l , r)
println!("{s_slice}");

// 遍历字符串
let s = String::from("hello, world.");

for c in s.chars() {
    println!("{}", c);
}
println!("{}", s);

for c in s.bytes() {
    println!("{}", c);
}
println!("{}", s);

// replace
let s1 = String::from("hello, world.");
let s2 = s1.replace("world", "rust");

println!("s1: {s1}");
println!("s2: {s2}");

// contains
let s1 = String::from("hello, world.");
let b: bool = s1.contains("world");

println!("'{s1}' contains world ? : {b}");
```

### 8.3 HashMap

```rust
// 初始化
let mut scores: HashMap<String, u8> = HashMap::<String, u8>::new();

// 插入键值对
scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Yellow"), 50);

// 访问元素
let team_name: String = String::from("Blue");
// get(&key) -> Option(&value)
// copied(): Option(&value) - > Option(value)
let score: u8 = scores.get(&team_name).copied().unwrap_or(0);
println!("{team_name}: {score}");

// 遍历元素
for (key, value) in &scores {
    println!("{key}: {value}");
}

// 所有权
let team_name: String = String::from("Blue");
let score: u8 = 10;

scores.insert(team_name, score);
// println!("{}", team_name); // error, move
println!("{}", score);

// 更新
// 1.覆盖
scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Blue"), 25);

// 2.只插入新键
scores.insert(String::from("Blue"), 10);

scores.entry(String::from("Yellow")).or_insert(50);
scores.entry(String::from("Blue")).or_insert(25);

// 3.根据旧值更新
let text = "hello world wonderful world";
let mut map: HashMap<String, u8> = HashMap::<String, u8>::new();

for word in text.split_whitespace() {
    let count = map.entry(String::from(word)).or_insert(0);
    *count += 1;
}

println!("{}", text);
println!("{:?}", map)
```

## CAP 9

### 9.1 panic!

```rust
// 造成 panic 的方式
// 执行会造成代码 panic 的操作（比如访问超过数组结尾的内容）
// 显式调用 panic! 宏
panic!("crash and burn!");

let v = vec![1, 2, 3,];
v[99]; // panic
// note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
// $ RUST_BACKTRACE=1 cargo run // 查看信息
```

### 9.2 Result

#### 处理错误

```rust
// upwrap(), expect(&str)
let greeting_file: std::fs::File = std::fs::File::open("hello.txt").unwrap(); // 失败会 panic
let greeting_file: std::fs::File = std::fs::File::open("hello.txt")
    .expect("hello.txt should be included in this project");

// 处理错误
let greeting_file_result: Result<std::fs::File, std::io::Error> = std::fs::File::open("hello.txt");
// 使用 match
let greeting_file = match greeting_file_result {
    Ok(file) => file,
    Err(error) => match error.kind() {
        std::io::ErrorKind::NotFound => match std::fs::File::create("hello.txt") {
            Ok(fc) => fc,
            Err(e) => panic!("Problem creating the file: {:?}", e),
        }
        other_error => panic!("Problem opening the file: {:?}", other_error),
    }
};

// 使用 闭包
let greeting_file = std::fs::File::open("hello.txt").unwrap_or_else(|error| {
    if error.kind() == std::io::ErrorKind::NotFound {
        std::fs::File::create("hello.txt").unwrap_or_else(|error| {
            panic!("Problem creating the file: {:?}", error);
        })
    } else {
        panic!("Problem opening the file: {:?}", error);
    }
});
```

#### 传播错误

```rust
// 传播错误
use std::io::Read;
fn read_username_from_file() -> Result<String, std::io::Error> {
    let username_file_result: Result<std::fs::File, std::io::Error> = std::fs::File::open("hello.txt");

    let mut username_file: std::fs::File = match username_file_result {
        Ok(file) => file,
        Err(e) => return Err(e),
    };

    let mut username = String::new();
    match username_file.read_to_string(&mut username) {
        Ok(_) => Ok(username),
        Err(e) => Err(e),
    }
}

// 使用 ? 运算符
use std::io::Read;
fn read_username_from_file() -> Result<String, std::io::Error> {
    // 如果 Result 的值是 Ok，这个表达式将会返回 Ok 中的值而程序将继续执行。
    // 如果值是 Err，Err 将作为整个函数的返回值
    let mut username_file: std::fs::File = std::fs::File::open("hello.txt")?;
    let mut username = String::new();

    // ? 运算符所使用的错误值被传递给了 from 函数, (impl From<io::Error> for OurError)
    // 它定义于标准库的 From trait 中, 其用来将错误从一种类型转换为另一种类型
    // read_to_string 返回的错误类型不是 std::io::Error, 但是能够被转换为 std::io::Error
    username_file.read_to_string(&mut username)?;
    Ok(username)
}

// 简写
use std::io::Read;
fn read_username_from_file() -> Result<String, std::io::Error> {
  	let mut username = String::new();
		std::fs::File::open("hello.txt")?.read_to_string(&mut username)?;
    Ok(username)
}

// std::fs::read_to_string
fn read_username_from_file() -> Result<String, std::io::Error> {
  	// 它会打开文件、新建一个 String、读取文件的内容，
  	// 并将内容放入 String，接着返回它
  	std::fs::read_to_string("hello.txt")
}
```

```rust
fn last_char_of_first_line(text: &str) -> Option<char> {
    text.lines().next()?.chars().last()
}
```

```rust
use std::error::Error;
use std::fs::File;

fn main() -> Result<(), Box<dyn Error>> {
    let greeting_file = File::open("hello.txt")?;

    Ok(())
}
```

## CAP 10

### 10.1 泛型

#### 函数中定义泛型

```rust
fn largest<T>(list: &[T]) -> &T {
    let mut largest = &list[0];

    for item in &list {
        if item > largest {
            largest = item;
        }
    }

    largest
}
```

#### 结构体中定义泛型

```rust
struct Point<T> {
    x: T,
    y: T,
}
let t = Point::<i32> {x:2_i32, y:6_i32};
```

#### 枚举中定义泛型

```rust
enum Option<T> {
  	Some(T),
  	None,
}
enum Result<T, E> {
  	Ok(T),
  	Err(T),
}
```

#### 方法中定义泛型

```rust
impl<T> Point<T> {
    fn x(&self) -> &T {
        &self.x
    }
}

// 为指定类型添加方法
impl Point<f64> {
    fn distance_from_origin(&self) -> f64 {
        (self.x.powi(2) + self.y.powi(2)).sqrt()
    }
}

// 不同泛型
struct Point<X1, Y1> {
    x: X1,
    y: Y1,
}
impl<X1, Y1> Point<X1, Y1> {
    fn mixup<X2, Y2>(self, other: Point<X2, Y2>) -> Point<X1, Y2> {
        Point {
            x: self.x,
            y: other.y,
        }
    }
}
fn main() {
    let p1 = Point::<i32, f32> { x: 2_i32, y: 3.14_f32 };
    let p2 = Point::<char, String> { x: 'a', y: String::from("hello") };
    let p3 = p1.mixup(p2);

    println!("p3.x = {}, p3.y = {}", p3.x, p3.y);
}
```

### 10.2 Trait

#### Trait

```rust
// 一个类型的行为由其可供调用的方法构成
// 如果可以对不同类型调用相同的方法的话，这些类型就可以共享相同的行为
// trait 定义是一种将方法签名组合起来的方法，目的是定义一个实现某些目的所必需的行为的集合

// 只有在 trait 或类型至少有一个属于当前 crate 时，才能对类型实现该 trait
trait Summary {
    fn summarize(&self) -> String;
}
struct Tweet {
    username: String,
    content: String,
}
impl Summary for Tweet {
    fn summarize(&self) -> String {
        format!("{}: {}", self.username, self.content)
    }
}
fn main() {
    let tweet: Tweet = Tweet {
        username: String::from("hxj"),
        content: String::from("hello, rust"),
    };
    println!("1 new tweet: {}", tweet.summarize());
}
```

```rust
// 使用默认实现
trait Summary {
    fn summarize(&self) -> String {
        // 默认实现
        String::from("(Read more...)")
    }
}
struct NewsArticle {
    author: String,
    content: String,
}
impl Summary for NewsArticle {
    // 使用默认实现
}
fn main() {
    let article = NewsArticle {
        author: String::from("Iceburgh"),
        content: String::from(
            "The Pittsburgh Penguins once again are the best \
             hockey team in the NHL.",
        ),
    };
    println!("New article available! {}", article.summarize());
}
```

```rust
// 默认实现结合非默认实现
trait Summary {
    fn summarize_author(&self) -> String;

    fn summarize(&self) -> String {
        format!("(Read more from {}...)", self.summarize_author())
    }
}
impl Summary for Tweet {
    fn summarize_author(&self) -> String {
        format!("@{}", self.username)
    }
}
```

#### Trait 作为参数

```rust
fn notify(item: &impl Summary) {
    println!("Breaking news! {}", item.summarize());
}
fn notify<T: Summary>(item: &T) {
    println!("Breaking news! {}", item.summarize());
}

fn notify(item1: &impl Summary, item2: &impl Summary) {
  	// // item1 和 item2 允许是不同类型的情况（只要它们都实现了 Summary）
    println!("Breaking news! {}", item1.summarize());
    println!("Breaking news! {}", item2.summarize());
}
fn notify<T: Summary>(item1: &T, item2: &T) {
  	// item1 和 item2 值的具体类型必须一致
    println!("Breaking news! {}", item1.summarize());
    println!("Breaking news! {}", item2.summarize());
}
```

#### 通过 + 指定多个 Trait

```rust
fn notify<T: Summary + Display>(item: &T) {
    println!("Breaking news! {}", item.summarize());
}

fn notify(item: &(impl Summary + Display)) {
    println!("Breaking news! {}", item.summarize());
}

// where 指定 Trait
fn notify<T, U>(item1: &T, item2: &U) -> i32
where
    T: Summary + Display,
    U: Clone + Display,
{
    0
}
```

#### 返回实现了 Trait 的类型

```rust
fn return_summarizable() -> impl Summary {
    Tweet {
        username: String::from("hxj"),
        content: String::from("hello, rust"),
    }
}
fn main() {
    let tweet = return_summarizable();
    tweet.summarize();
}
```

#### 对实现了特定 Trait 的类型实现 Trait

```rust
// 标准库为任何实现了 Display trait 的类型实现了 ToString trait
impl<T: Display> ToString for T {
  	//
}
```

### 10.3 生命周期

```rust
// 生命周期是另一类我们已经使用过的泛型。
// 不同于确保类型有期望的行为，生命周期确保引用如预期一直有效
```

#### 函数中的生命周期

```rust
// 例如如果函数有一个生命周期 'a 的 i32 的引用的参数 first。
// 还有另一个同样是生命周期 'a 的 i32 的引用的参数 second。
// 这两个生命周期注解意味着引用 first 和 second 必须与这泛型生命周期存在得一样久

// 泛型生命周期 'a 的具体生命周期等同于 x 和 y 的生命周期中较小的那一个
// 因为我们用相同的生命周期参数 'a 标注了返回的引用值，所以返回的引用值就能保证在 x 和 y 中较短的那个生命周期结束之前保持有效
fn longest<'a>(s1: &'a str, s2: &'a str) -> &'a str {
    if s1.len() > s2.len() {
        s1
    } else {
        s2
    }
}

fn main() {
    let string1 = String::from("long string is long");
    {
        let string2 = String::from("xyz");
        let result = longest(string1.as_str(), string2.as_str());
        println!("The longest string is {result}");
    }
}

// 生命周期语法是用于将函数的多个参数与其返回值的生命周期进行关联的
// 一旦它们形成了某种关联，Rust 就有了足够的信息来允许内存安全的操作并阻止会产生悬垂指针亦或是违反内存安全的行为
```

#### 结构体中的生命周期

```rust
struct Tmp<'a> {
    part: &'a str,
}
fn main() {
    let s1 = String::from("hello");
    let mut tmp = Tmp { part: "hello" };
    {
        let s2 = String::from("hello, world.");
        tmp.part = &s2[..];
        println!("{}", tmp.part); // 引用的生命周期取决于使用的范围
    }
}
```

#### 生命周期规则

```rust
//函数或方法的参数的生命周期被称为 输入生命周期（input lifetimes）
// 而返回值的生命周期被称为 输出生命周期（output lifetimes）

// 编译器采用三条规则来判断引用何时不需要明确的注解。
// 		第一条规则适用于输入生命周期，后两条规则适用于输出生命周期
// 		如果编译器检查完这三条规则后仍然存在没有计算出生命周期的引用，编译器将会停止并生成错误。这些规则适用于 fn 定义，以及 impl 块。

// 第一条规则是编译器为每一个引用参数都分配一个生命周期参数。换句话说就是，函数有一个引用参数的就有一个生命周期参数：fn foo<'a>(x: &'a i32)，有两个引用参数的函数就有两个不同的生命周期参数，fn foo<'a, 'b>(x: &'a i32, y: &'b i32)，依此类推。

// 第二条规则是如果只有一个输入生命周期参数，那么它被赋予所有输出生命周期参数：fn foo<'a>(x: &'a i32) -> &'a i32。

// 第三条规则是如果方法有多个输入生命周期参数并且其中一个参数是 &self 或 &mut self，说明是个对象的方法 (method)(译者注：这里涉及 rust 的面向对象参见 17 章)，那么所有输出生命周期参数被赋予 self 的生命周期。第三条规则使得方法更容易读写，因为只需更少的符号。

struct Tmp<'a> {
    part: &'a str,
}

impl<'a> Tmp<'a> {
    fn func(&self, s1: &'a str) -> &'a str {
        s1
    }
}

fn main() {
    let s1 = String::from("hello");
    let t = Tmp { part: &s1[..] };
    println!("{}", t.func(&s1[..]));
}
```

#### 静态声明周期

```rust
// 'static: 生命周期是整个程序期间
// 字符串字面值都拥有 'static 生命周期
let s: &'static str = "I have a static lifetime.";
```

#### 函数

```rust
fn longest<'a, 'b, T>(x: &'a str, y: &'a str, t: &'b T) -> &'a str 
where
    T: std::fmt::Display
{
    println!("{}", t);
    if x.len() > y.len() {
        x
    } else {
        y
    }
}

fn main() {
    let s1 = String::from("hello");
    let s2 = "hello, world.";
    let t = String::from("This is the Rust!");
    let s3 = longest(&s1[..], s2, &t);
    println!("longest str is '{}'", s3);
}
```

## CAP 11

### 11.1 如何编写测试

```rust
$ cargo new adder --lib

pub fn add(left: u64, right: u64) -> u64 {
    left + right
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn it_works() {
        let result = add(2, 2);
        assert_eq!(result, 4);
      
      	// #[derive(PartialEq, Debug)]
      	// 被比较的值必须实现了 PartialEq 和 Debug trait
      	assert_eq!(lhs, rhs);
      	assert_ne!(lhs, rhs);
      
      	let ans: bool = ...
      	assert!(ans);
      	assert!(ans, "ans is {}", ans); // 自定义错误信息
        panic!("Make this test fail");
    }
}

$ cargo test
```

```rust
pub struct Guess {
    value: i32,
}
impl Guess {
    pub fn new(value: i32) -> Guess {
        if value < 1 {
            panic!(
                "Guess value must be greater than or equal to 1, got {value}."
            );
        } else if value > 100 {
            panic!(
                "Guess value must be less than or equal to 100, got {value}."
            );
        }

        Guess { value }
    }
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    #[should_panic] // 发生 panic, test 通过
    fn greater_than_100() {
        Guess::new(200);
    }
  
  	#[test]
  	//  // 发生 panic, 并且 panic 的报错信息包含 expected, test 通过
    #[should_panic(expected = "must be less than or equal to 100")]
    fn greater_than_100_better() {
        Guess::new(200);
    }
}
```

```rust
pub fn add(left: usize, right: usize) -> usize {
    left + right
}

#[cfg(test)]
mod tests {
    use super::*;

    // ANCHOR: here
    #[test]
    fn it_works() -> Result<(), String> {
        let result = add(2, 2);

        if result == 4 {
            Ok(())
        } else {
            Err(String::from("two plus two does not equal four"))
        }
    }
    // ANCHOR_END: here
}
```

### 11.2 控制测试如何运行

#### 并行或连续的运行测试

```shell
$ cargo test -- --test-threads=1 # 指定线程数量
$ cargo test -- --show-output # 查看通过的测试中打印的值
$ cargo test test_name # 运行指定测试, test_name可以是模块名的子串也可以是函数名字的子串

#[test]
#[ignore] // 忽略该测试
fn expensive_test() {
    // 需要运行一个小时的代码
}

$ cargo test # 忽略 '#[ignore]' 测试
$ cargo test -- --ignored # 运行 '#[ignore]' 测试
$ cargo test -- --include-ignored # 运行所有测试
```

### 11.3 测试的组织结构

#### 单元测试



#### 集成测试

```rust
/*
adder
├── Cargo.lock
├── Cargo.toml
├── src
│   └── lib.rs
└── tests
    └── integration_test.rs
*/
// 为了编写集成测试，需要在项目根目录创建一个 tests 目录，与 src 同级
// Cargo 知道如何去寻找这个目录中的集成测试文件
// 接着可以随意在这个目录中创建任意多的测试文件，Cargo 会将每一个文件当作单独的 crate 来编译
```

```rust
// lib.rs
pub fn add_two(a: i32) -> i32 {
    a + 2
}

#[cfg(test)]
mod my_tests {
    use super::*;

    #[test]
    fn add_two_and_two() {
        assert_eq!(4, add_two(2));
    }
}
```

```rust
// integration_test.rs
use adder::add_two;

#[test]
fn it_adds_two() {
    assert_eq!(4, add_two(2));
}

// $ cargo test --test integration_test // 单独运行
```

## CAP 13

### 13.1 闭包
闭包:
    1.可以捕获环境的匿名函数
    2.Rust 的 闭包（closures）是可以保存在变量中或作为参数传递给其他函数的匿名函数
    3.闭包允许捕获其被定义时所在作用域中的值

闭包可以通过三种方式捕获环境中的值:
    不可变借用, 可变借用, 获取所有权

#### 不可变借用
```rust
fn main() {
    let list = vec![1, 2, 3];
    println!("{:?}", list);

    let only_borrows = | | { println!("{:?}", list) };

    println!("{:?}", list);
    only_borrows();
    println!("{:?}", list);
}
```

#### 可变借用
```rust
fn main() {
    let mut list = vec![1, 2, 3];
    println!("{:?}", list);

    let mut borrows_mutably = | | { list.push(4); };

    //println!("{:?}", list); // list's &mut 被 borrows_mutably 捕获
    borrows_mutably();
    println!("{:?}", list);
}
```

#### 获取所有权
```rust
fn main() {
    let list = vec![1, 2, 3];
    println!("{:?}", list);

    std::thread::spawn(move| | { println!("From thread: {list:?}") })
        .join()
        .unwrap();
    //println!("{:?}", list);
}
```

1.FnOnce 适用于只能被调用一次的闭包. 所有闭包至少都实现了这个 trait，因为所有闭包都能被调用. 一个会将捕获的值从闭包体中移出的闭包只会实现 FnOnce trait, 而不会实现其他 Fn 相关的 trait, 因为它只能被调用一次
```rust
impl<T> Option<T> {
    pub fn unwrap_or_else<F>(self, f: F) -> T
    where
        F: FnOnce() -> T
    {
        match self {
            Some(x) => x,
            None => f(),
        }
    }
}
```

2.FnMut 适用于不会将捕获的值移出闭包体, 但可能会修改捕获值的闭包. 这类闭包可以被调用多次
```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

fn main() {
    let mut list = [
        Rectangle { width: 10, height: 1 },
        Rectangle { width: 3, height: 5 },
        Rectangle { width: 7, height: 12 },
    ];

    list.sort_by_key(|r| r.width);
    println!("{list:#?}");
}
```

3.Fn 适用于既不将捕获的值移出闭包体, 也不修改捕获值的闭包, 同时也包括不从环境中捕获任何值的闭包. 这类闭包可以被多次调用而不会改变其环境. 这在会多次并发调用闭包的场景中十分重要

### 13.2 迭代器
```rust
fn main() {
    let v1 = vec![1, 2, 3];

    let v1_iter = v1.iter();

    for val in v1_iter {
        println!("Got: {val}");
    }
}

fn main() {
    let v1 = vec![1, 2, 3];
    // 迭代器是 惰性的（lazy）, 这意味着在调用消费迭代器的方法之前不会执行任何操作
    let v1_iter = v1.iter();
    assert_eq!(v1_iter.next(), Some(&1));
    assert_eq!(v1_iter.next(), Some(&2));
    assert_eq!(v1_iter.next(), Some(&3));
    assert_eq!(v1_iter.next(), None);

    // 注意我们需要将 v1_iter 声明为可变的：在迭代器上调用 next 方法会改变迭代器内部的状态，该状态用于跟踪迭代器在序列中的位置。换句话说，代码 消费（consume）了，或者说用尽了迭代器。每一次 next 调用都会从迭代器中消费一个项。使用 for 循环时无需使 v1_iter 可变因为 for 循环会获取 v1_iter 的所有权并在后台使 v1_iter 可变
}
```

```rust
// iter(): 生成不可变引用的迭代器
// iter_mut(): 生成可变引用的迭代器
// into_iter(): 获取 v1 所有权的迭代器
```

```rust
fn main() {
    let v1 = vec![1, 2, 3];
    
    let v1_iter = v1.iter();

    let sum: i32 = v1_iter.sum(); // v1_iter is moved

    println!("{:?}'s sum = {}", v1, sum);
}
```

```rust
fn main() {
    let v1 = vec![1, 2, 3];
    
    let v1_iter = v1.iter();

    let v2: Vec<i32> = v1_iter.map(|val| { val+1 }).collect(); // v1_iter is moved
    println!("v1: {v1:?}");
    println!("v2: {v2:?}");
}
```

```rust
#[derive(PartialEq, Debug)]
struct Thing {
    size: u8,
    style: String,
}

fn thing_in_size(things: Vec<Thing>, size: u8) -> Vec<Thing> {
    let vec_iter = things.into_iter();
    vec_iter.filter(| thing | { thing.size == size }).collect()
}

fn main() {
    let things = vec![Thing{size: 2, style: "hello".to_string()},
    Thing{size: 2, style: "hello".to_string()},
    Thing{size: 3, style: "hello".to_string()},
    Thing{size: 4, style: "hello".to_string()},];

    let things = thing_in_size(things, 2);
    println!("{things:?}");
}
```

## CAP 15 智能指针
Box<T>

Rc<T>(Rc::clone(), strong_count(), Rc::downgrade()), Weak<T>(upgrade() -> Option<Rc<T>>)
RefCell<T>(borrow(), borrow_mut())

Arc<T>
Mutex<T>(lock())
### 15.1 Box<T>
```rust
let num = Box::<i32>::new(10);
println!("num = {}", num);
```

```rust
enum List {
    Cons(i32, Box<List>),
    Nil,
}
fn main() {
    let list = List::Cons(1, 
        Box::<List>::new(List::Cons(2, 
            Box::<List>::new(List::Cons(3, 
                Box::<List>::new(List::Nil))))));
    // Box<T> 类型是一个智能指针，因为它实现了 Deref trait，它允许 Box<T> 值被当作引用对待。
    // 当 Box<T> 值离开作用域时，由于 Box<T> 类型 Drop trait 的实现，box 所指向的堆数据也会被清除。
}
```

### 15.2 Deref Trait
#### 追踪指针的值
```rust
let x: i32 = 10;
let y: &i32 = &x;

assert_eq!(x, 10);
assert_eq!(*y, 10);
```

#### 像引用一样使用 Box<T>
```rust
fn main() {
    let x = 10;
    let y = Box::new(x);

    assert_eq!(x, 10);
    assert_eq!(*y, 10);
}
```

#### 自定义智能指针, 实现 Deref Trait, 将自定义类型像引用一样处理
```rust
struct MyBox<T>(T,);
impl<T> MyBox<T> {
    fn new(x: T) -> MyBox<T> {
        MyBox(x,)
    }
}
impl<T> std::ops::Deref for MyBox<T> {
    type Target = T; // 定义了用于此 trait 的关联类型
    fn deref(&self) -> &Self::Target {
        &self.0
    }
}
fn main() {
    let x = 10;
    let y = MyBox::new(x);

    assert_eq!(x, 10);
    assert_eq!(*y, 10); // *y == *(y.deref())
}
```

#### 函数和方法的隐式 Deref 强制转换
```rust
// ... 如上

fn MyPrint(s: &str) {
    println!("{}", s);
}

fn main() {
    let s = MyBox::new(String::from("hello"));
    MyPrint(&s);
    MyPrint( &(*s)[..] );
    // MyBox impl std::ops::Deref, => &MyBox<String> -> &String
    // String impl std::ops::Deref => &String -> &str
}
```

#### Deref 强制转换如何与可变性交互
```rust
// 类似于如何使用 Deref trait 重载不可变引用的 * 运算符
// Rust 提供了 DerefMut trait 用于重载可变引用的 * 运算符

// Rust 在发现类型和 trait 实现满足三种情况时会进行 Deref 强制转换：

// 1.当 T: Deref<Target=U>    时从 &T     到 &U
// 2.当 T: DerefMut<Target=U> 时从 &mut T 到 &mut U
// 3.当 T: Deref<Target=U>    时从 &mut T 到 &U
```

### 15.3 Drop Trait
```rust
// 在 Rust 中，可以指定每当值离开作用域时被执行的代码，编译器会自动插入这些代码
// 于是我们就不需要在程序中到处编写在实例结束时清理这些变量的代码, 而且还不会泄漏资源
// 指定在值离开作用域时应该执行的代码的方式是实现 Drop trait
struct CustomSmartPointer {
    data: String,
}
impl Drop for CustomSmartPointer { // 使用 Drop Trait 运行清理代码
    fn drop(&mut self) {
        println!("Dropping CustomSmartPointer with data `{}`!", self.data);
    }
}

fn main() {
    let c = CustomSmartPointer {
        data: String::from("some data"),
    };
    println!("CustomSmartPointer created.");
    // 通过 std::mem::drop 提早丢弃值
    std::mem::drop(c); // 会调用 fn drop(&mut self)
    println!("CustomSmartPointer dropped before the end of main.");
}
```

### 15.4 Rc<T> 引用计数智能指针
```rust
// Rc<T> 只能用于单线程场景
use std::rc::Rc;
enum List {
    Cons(i32, Rc<List>),
    Nil,
}
fn main() {
    use List::{Cons, Nil};
    
    let a: Rc<List> = Rc::new(Cons(1, Rc::new(Cons(2, Rc::new(Nil)))));
    println!("count after creating a = {}", Rc::strong_count(&a));

    let b: List = Cons(3, Rc::clone(&a));
    println!("count after creating b = {}", Rc::strong_count(&a));

    {
        let c: List = Cons(4, Rc::clone(&a));
        println!("count after creating c = {}", Rc::strong_count(&a));
    }

    println!("count after c goes out of scope = {}", Rc::strong_count(&a));
}
```

### 15.5 RefCell<T>
RefCell<T> 只能用于单线程场景

如下为选择 Box<T>，Rc<T> 或 RefCell<T> 的理由：
    1.Rc<T> 允许相同数据有多个所有者；Box<T> 和 RefCell<T> 有单一所有者。
    2.Box<T> 允许在编译时执行不可变或可变借用检查；
        Rc<T>仅允许在编译时执行不可变借用检查；
        RefCell<T> 允许在运行时执行不可变或可变借用检查。
    3.因为 RefCell<T> 允许在运行时执行可变借用检查，所以我们可以在即便 RefCell<T> 自身是不可变的情况下修改其内部的值。
在不可变值内部改变值就是**内部可变性**模式。让我们看看何时内部可变性是有用的，并讨论这是如何成为可能的。

#### 内部可变性, 不可变值的可变借用
编译器中的借用检查器允许*内部可变性*并相应地在*运行*时检查借用规则。如果违反了这些规则，会出现 panic 而不是编译错误
```rust
pub trait Messenger {
    fn send(&self, msg: &str);
}

pub struct LimitTracker<'a, T: Messenger> {
    messenger: &'a T,
    value: usize,
    max: usize,
}

impl<'a, T> LimitTracker<'a, T>
where
    T: Messenger, // impl's T
{
    pub fn new(messenger: &'a T, max: usize) -> LimitTracker<'a, T> {
        LimitTracker {
            messenger,
            value: 0,
            max,
        }
    }

    pub fn set_value(&mut self, value: usize) {
        self.value = value;

        let percentage_of_max = self.value as f64 / self.max as f64;

        if percentage_of_max >= 1.0 {
            self.messenger
                .send("Error: You are over your quota!");
        } else if percentage_of_max >= 0.9 {
            self.messenger
                .send("Urgent warning: You've used up over 90% of your quota!");
        } else if percentage_of_max >= 0.75 {
            self.messenger
                .send("Warning: You've used up over 75% of your quota!");
        }
    }
}

#[cfg(test)]
mod tests {
    use super::*;
    use std::cell::RefCell;
    struct MockMessenger {
        sent_messages: RefCell<Vec<String>>, // RefCell
    }

    impl MockMessenger {
        fn new() -> MockMessenger {
            MockMessenger {
                sent_messages: RefCell::new(<Vec::<String>>::new()), // RefCell
            }
        }
    }

    impl Messenger for MockMessenger {
        fn send(&self, message: &str) {
            // 因为此处不能将 &self -> &mut self, 不能改变接口的声明
            // 所以使用 RefCell
            // .borrow_mut()
            self.sent_messages.borrow_mut().push(String::from(message));
        }
    }

    #[test]
    fn it_sends_an_over_75_percent_warning_message() {
        let mock_messenger = MockMessenger::new();
        let mut limit_tracker = LimitTracker::new(&mock_messenger, 100);

        limit_tracker.set_value(80);
        // .borrow()
        assert_eq!(mock_messenger.sent_messages.borrow().len(), 1);
    }
}
```

#### RefCell 在运行时记录借用
当创建不可变和可变引用时，我们分别使用 & 和 &mut 语法。

对于 RefCell<T> 来说，则是 borrow 和 borrow_mut 方法，这属于 RefCell<T> 安全 API 的一部分。

borrow 方法返回 Ref<T> 类型的智能指针，borrow_mut 方法返回 RefMut<T> 类型的智能指针。这两个类型都实现了 Deref，所以可以当作常规引用对待

RefCell<T> 记录当前有多少个活动的 Ref<T> 和 RefMut<T> 智能指针。
每次调用 borrow，RefCell<T> 将活动的不可变借用计数加一。当 Ref<T> 值离开作用域时，不可变借用计数减一。
就像编译时借用规则一样，RefCell<T> 在任何时候只允许有多个不可变借用或一个可变借用。
```rust
impl Messenger for MockMessenger {
    fn send(&self, message: &str) {
        let mut one_borrow = self.sent_messages.borrow_mut();
        let mut two_borrow = self.sent_messages.borrow_mut();

        // 运行时报错, 有多个可变引用
        one_borrow.push(String::from("hello"));
        two_borrow.push(String::from("hello"));
    }
}
```

#### Rc<T> and RefCell<T>
```rust
use std::rc::Rc;
use std::cell::RefCell;

#[derive(Debug)]
enum List {
    Cons(Rc<RefCell<i32>>, Rc<List>),
    Nil,
}

use crate::List::{Cons, Nil};

fn main() {
    let value = Rc::new(RefCell::new(5));

    let a = Rc::new(Cons(Rc::clone(&value), Rc::new(Nil)));

    let b = Cons(Rc::new(RefCell::new(3)), Rc::clone(&a));
    let c = Cons(Rc::new(RefCell::new(4)), Rc::clone(&a));
    
    *(*value).borrow_mut() += 10;
    
    println!("a after = {:?}", a);
    println!("a after = {:?}", b);
    println!("a after = {:?}", c);
}
```

### 15.6 循环引用与内存泄漏
#### 制造循环引用
```rust
use std::rc::Rc;
use std::cell::RefCell;

#[derive(Debug)]
enum List {
    Cons(i32, RefCell<Rc<List>>),
    Nil,
}

use crate::List::{Cons, Nil};

impl List {
    fn tail(&self) -> Option<&RefCell<Rc<List>>> {
        match self {
            Cons(_, item) => Some(item),
            Nil => None,
        }
    }
}

fn main() {
    let a = Rc::new(Cons(5, RefCell::new(Rc::new(Nil))));
    println!("a initial rc count = {}", Rc::strong_count(&a));
    println!("a next item = {:?}", (*a).tail());

    println!("");

    let b = Rc::new(Cons(10, RefCell::new(Rc::clone(&a))));
    println!("a rc count after b creation = {}", Rc::strong_count(&a));
    println!("b initial rc count = {}", Rc::strong_count(&b));
    println!("b next item = {:?}", (*b).tail());

    println!("");

    if let Some(link) = a.tail() {
        (*(*link).borrow_mut()) = Rc::clone(&b);
    }

    println!("a rc count after changing a = {}", Rc::strong_count(&a));
    println!("b rc count after changing b = {}", Rc::strong_count(&b));

    //println!("a next item = {:?}", a.tail()); // it will overflow the stack
}
```

#### 创建树形结构, 带有子节点的 Node
```rust
use std::cell::RefCell;
use std::rc::Rc;
use std::rc::Weak;

// 创建树形数据结构: 带有子节点的 Node
#[derive(Debug)]
struct Node {
    value: i32,
    parent: RefCell<Weak<Node>>,
    children: RefCell<Vec<Rc<Node>>>,
}

fn main() {
    let leaf = Rc::new(Node {
        value: 3,
        parent: RefCell::new(Weak::new()),
        children: RefCell::new(vec![]),
    });

    println!("leaf strong = {}, weak = {}", 
            Rc::strong_count(&leaf),
            Rc::weak_count(&leaf));

    {
    let branch = Rc::new(Node {
        value: 5,
        parent: RefCell::new(Weak::new()),
        children: RefCell::new(vec![Rc::clone(&leaf)]),
    });

    *(leaf.parent.borrow_mut()) = Rc::downgrade(&branch);
    
    println!("leaf strong = {}, weak = {}", 
            Rc::strong_count(&leaf),
            Rc::weak_count(&leaf));
    
    println!("branch strong = {}, weak = {}", 
            Rc::strong_count(&branch),
            Rc::weak_count(&branch));
    }

    println!("leaf parent = {:?}", leaf.parent.borrow().upgrade());

    println!("leaf string = {}, weak = {}", 
            Rc::strong_count(&leaf),
            Rc::weak_count(&leaf));
}
```

## CAP 16 并发

### 16.1 线程概念和 API

#### 使用 spawn 创建新线程
```rust
use std::thread;
use std::time::Duration;

fn main() {
    let handle = thread::spawn(| | {
        for i in 1.. 10 {
            println!("hi number {i} from the spawned thread!");
            thread::sleep(Duration::from_millis(1));
        }
    });

    handle.join().unwrap();

    for i in 1..5 {
        println!("hi number {i} from the main thread!");
        thread::sleep(Duration::from_millis(1));
    }

    //handle.join().unwrap();
}
```
#### 在线程中使用闭包
```rust
use std::thread;

fn main() {
    let nums = vec![1, 2, 3];

    let handle = thread::spawn(move | | {
        println!("{:?}", nums);
    });

    handle.join().unwrap();
}
```

### 16.2 使用消息传递在线程中传送数据
#### 使用消息传递在线程中传送数据
```rust
use std::thread;
use std::sync::mpsc;

fn main() {
    // mpsc::channel() 产生的信道是多个生产者, 单个消费者模型
    let (tx, rx) = mpsc::channel();

    thread::spawn(move | | {
        let msg = String::from("Hi");
        tx.send(msg).unwrap();
        //println!("{msg}"); // error, msg is moved
    });

    // 这个方法会阻塞主线程执行直到从信道中接收一个值
    let received = rx.recv().unwrap();
    println!("Got: {received}");

    // try_recv 不会阻塞，相反它立刻返回一个 Result<T, E>：Ok 值包含可用的信息，而 Err 值代表此时没有任何消息。
    // 如果线程在等待消息过程中还有其他工作时使用 try_recv 很有用：可以编写一个循环来频繁调用 try_recv，
    // 在有可用消息时进行处理，其余时候则处理一会其他工作直到再次检查。
}
```

#### 发送多个值并观察接受者的等待
```rust
use std::thread;
use std::time::Duration;
use std::sync::mpsc;

fn main() {
    // mpsc::channel() 产生的信道是多个生产者, 单个消费者模型
    let (tx, rx) = mpsc::channel();

    thread::spawn(move | | {
        let msgs = vec![
            String::from("hi"),
            String::from("from"),
            String::from("the"),
            String::from("thread"),
        ];
        for msg in msgs {
            tx.send(msg).unwrap();
            thread::sleep(Duration::from_secs(1));
        }
    });

    // 此处没有显式调用 'recv' 函数, 将 'rx' 当作迭代器
    // 信道被关闭时, 迭代器也将结束
    for received in rx {
        println!("Got: {received}");
    }
}
```

#### 通过克隆发送者来创建多个生产者
```rust

use std::thread;
use std::time::Duration;
use std::sync::mpsc;

fn main() {
    // mpsc::channel() 产生的信道是多个生产者, 单个消费者模型
    let (tx, rx) = mpsc::channel();
    let tx_n = tx.clone();

    thread::spawn(move | | {
        let msgs = vec![
            String::from("hi"),
            String::from("from"),
            String::from("the"),
            String::from("thread"),
        ];
        for msg in msgs {
            tx.send(msg).unwrap();
            thread::sleep(Duration::from_secs(1));
        }
    });

    thread::spawn(move | | {
        let msgs = vec![
            String::from("more"),
            String::from("messages"),
            String::from("for"),
            String::from("you"),
        ];
        for msg in msgs {
            tx_n.send(msg).unwrap();
            thread::sleep(Duration::from_secs(1));
        }
    });

    // 此处没有显式调用 'recv' 函数, 将 'rx' 当作迭代器
    // 信道被关闭时, 迭代器也将结束
    for received in rx {
        println!("Got: {received}");
    }
}
```

### 16.3 共享状态的并发
#### 单线程中使用 Mutex<T>
```rust
use std::sync::Mutex;
fn main() {
    let m_i32 = Mutex::new(5);

    {
        let mut num = m_i32.lock().unwrap();
        // nums: &mut i32
        *num += 10;
    }

    println!("m = {m_i32:?}");
    // Mutex<T> 是一个智能指针。lock() 返回 一个叫做 MutexGuard 的智能指针。
    // MutexGuard 实现了 Deref 来指向其内部数据；它也实现了 Drop，当 MutexGuard 离开作用域时，自动释放锁。
    // 有了这个特性，就不会有忘记释放锁的潜在风险（忘记释放锁会使互斥器无法再被其它线程使用），因为锁的释放是自动发生的。
}
```

#### 在线程间共享 Mutex<T>
```rust
use std::sync::Mutex;
use std::sync::Arc;
use std::thread;
fn main() {
    let counter = Arc::new(Mutex::new(0));
    let mut handles = vec![];

    for _ in 0..10 {
        let counter = Arc::clone(&counter);
        let handle = thread::spawn(move | | {
            // counter 是不可变的，仍然可以获取其内部值的可变引用；
            // 这意味着 Mutex<T> 提供了内部可变性
            let mut num = counter.lock().unwrap();
            *num += 1;
        });
        handles.push(handle);
    }

    for handle in handles {
        handle.join().unwrap();
    }

    println!("counter = {}", *(counter.lock().unwrap()));

}
```

### 16.4 使用 Sync 和 Send trait 的可扩展并发
有两个并发概念是内嵌于语言中的：std::marker 中的 Sync 和 Send trait

#### 通过 Send 允许在线程间转移所有权
Send 标记 trait 表明实现了 Send 的类型值的所有权可以在线程间传送

任何完全由 Send 的类型组成的类型也会自动被标记为 Send。
几乎所有基本类型都是 Send 的，除了第十九章将会讨论的裸指针（raw pointer）。

#### Sync 允许多线程访问
Sync 标记 trait 表明一个实现了 Sync 的类型可以安全的在多个线程中拥有其值的引用。

对于任意类型 T，如果 &T（T 的不可变引用）是 Send 的话 T 就是 Sync 的，这意味着其引用就可以安全的发送到另一个线程。
类似于 Send 的情况，基本类型是 Sync 的，完全由 Sync 的类型组成的类型也是 Sync 的。

## CAP 17 面向对象编程特性

### 17.1 不同类型值的 trait 对象
trait 对象具体的作用是允许对通用行为进行抽象

当使用 trait 对象时，Rust 必须使用动态分发。编译器无法知晓所有可能用于 trait 对象代码的类型，所以它也不知道应该调用哪个类型的哪个方法实现。
为此，Rust 在运行时使用 trait 对象中的指针来知晓需要调用哪个方法。
```rust
// lib.rs

pub trait Draw {
    // self: &Self
    fn draw(&self);
}

pub struct Screen {
    pub components: Vec<Box<dyn Draw>>,
}
impl Screen {
    pub fn run(&self) {
        for component in self.components.iter() {
            component.draw();
        }
    }
}

pub struct Button {
    pub width: u32,
    pub height: u32,
    pub label: String,
}
impl Draw for Button {
    fn draw(&self) {
        println!("draw behavior of button");
    }
}

```
```rust
// main.rs
use adder::{Draw, Screen, Button};

struct SelectBox {
    width: u32,
    height: u32,
    options: Vec<String>,
}
impl Draw for SelectBox {
    fn draw(&self) {
        println!("draw behavior of selectbox");
    }
}

fn main() {
    let button = Box::new(Button{
        width: 10,
        height: 10,
        label: String::from("click!"),
    });

    let selectbox = Box::new(SelectBox{
        width: 10,
        height: 10,
        options: vec![],
    });

    let mut screen = Screen {
        components: vec![],
    };
    screen.components.push(button);
    screen.components.push(selectbox);

    screen.run();
}
```

### 17.2
#### code 1
```rust
// main.rs
use blog::Post;

fn main() {
    let mut post = Post::new();

    post.add_text("I ate a salad for lunch today");
    assert_eq!("", post.content());

    post.request_review();
    assert_eq!("", post.content());

    post.approve();
    assert_eq!("I ate a salad for lunch today", post.content());
}
```
```rust
// lib.rs

pub struct Post {
    state: Option<Box<dyn State>>,
    content: String,
}

impl Post {
    pub fn new() -> Self {
        Post {
            state: Some(Box::new(Draft{

            })),
            content: String::new(),
        }
    }

    pub fn content(&self) -> &str{
        self.state.as_ref().unwrap().content(self)
    }

    pub fn add_text(&mut self, text: &str) {
        self.content.push_str(text);
    }

    pub fn request_review(&mut self) {
        if let Some(s) = self.state.take() {
            self.state = Some(s.request_review());
        }
    }

    pub fn approve(&mut self) {
        if let Some(s) = self.state.take() {
            self.state = Some(s.approve());
        }
    }
}

trait State {
    fn request_review(self: Box<Self>) -> Box<dyn State>;
    fn approve(self: Box<Self>) -> Box<dyn State>;
    fn content<'a>(&self, post: &'a Post) -> &'a str {
        "" // 默认实现
    }
}

struct Draft {

}
impl State for Draft {
    fn request_review(self: Box<Self>) -> Box<dyn State> {
        Box::new(PendingReview {

        })
    }
    fn approve(self: Box<Self>) -> Box<dyn State> {
        self
    }
}

struct PendingReview {

}
impl State for PendingReview {
    fn request_review(self: Box<Self>) -> Box<dyn State> {
        self
    }
    fn approve(self: Box<Self>) -> Box<dyn State> {
        Box::new(Published{

        })
    }
}

struct Published {

}
impl State for Published {
    fn request_review(self: Box<Self>) -> Box<dyn State> {
        self
    }
    fn approve(self: Box<Self>) -> Box<dyn State> {
        self
    }
    fn content<'a>(&self, post: &'a Post) -> &'a str {
        &post.content
    }
}

```

#### code 2
```rust
// main.rs
use blog::Post;

fn main() {
    let mut post = Post::new();

    post.add_text("I ate a salad for lunch today");
    
    let post = post.request_review();

    let post = post.approve();

    assert_eq!("I ate a salad for lunch today", post.content());   
}
```
```rust
// lib.rs
pub struct Post {
    content: String,
}
impl Post {
    pub fn new() -> DraftPost {
        DraftPost {
            content: String::new(),
        }
    }

    pub fn content(&self) -> &str {
        &self.content
    }
}

pub struct DraftPost {
    content: String,
}
impl DraftPost {
    pub fn add_text(&mut self, text: &str) {
        self.content.push_str(text);
    }

    pub fn request_review(self) -> PendingReviewPost {
        PendingReviewPost {
            content: self.content,
        }
    }
}

pub struct PendingReviewPost {
    content: String,
}
impl PendingReviewPost {
    pub fn approve(self) -> Post {
        Post {
            content: self.content,
        }
    }
}
```

## CAP 18 模式与模式匹配
### 18.1 使用模式的位置
```rust
// match
let x = Some(10_i32);
let x = match x {
    Some(i) => Some(i+1),
    None => None,
};

// if let
let favorite_color: Option<&str> = None;
let is_tuesday = false;
let age: Result<u8, _>  = "30".parse();

if let Some(color) = favorite_color {
    println!("Using your favorite color, {color}, as the background");
} else if is_tuesday {
    println!("Tuesday is green day!");
} else if let Ok(age) = age {
    if age > 30 {
        println!("Using purple as the background color");
    } else {
        println!("Using orange as the background color");
    }
} else {
    println!("Using blue as the background color");
}

// while let
let mut stack = Vec::new();

stack.push(1);
stack.push(2);
stack.push(3);

while let Some(top) = stack.pop() {
    println!("{}", top);
}

// for
let v = vec!['a', 'b', 'c'];
for (idx, val) in v.iter().enumerate() {
    println!("{}: {}", idx, val);
}
println!("{:?}", v);

// let
let x = 5_i32;

let (x, y, z,) = (1, 2, 3,);

let (x, y, _,) = (1, 2, 3,);
let (x, ..) = (1, 2, 3,);

// func
fn foo(x: i32) {
    println!("{}", x);
}

fn sum((n1, n2,): (i32, i32,)) {
    println!("{}", n1 + n2);
}

fn main() {
    foo(10);
    sum((2, 3,));
}
```

### 18.2 Refutability（可反驳性）: 模式是否会匹配失效
模式有两种形式：refutable（可反驳的）和 irrefutable（不可反驳的）。
能匹配任何传递的可能值的模式被称为是 不可反驳的（irrefutable）。
    一个例子就是 let x = 5; 语句中的 x，因为 x 可以匹配任何值所以不可能会失败。
对某些可能的值进行匹配会失败的模式被称为是 可反驳的（refutable）。
    一个这样的例子便是 if let Some(x) = a_value 表达式中的 Some(x)；
    如果变量 a_value 中的值是 None 而不是 Some，那么 Some(x) 模式不能匹配。

函数参数、let 语句和 for 循环只能接受不可反驳的模式，因为当值不匹配时，程序无法进行有意义的操作。
if let 和 while let 表达式可以接受可反驳和不可反驳的模式，但编译器会对不可反驳的模式发出警告，
    因为根据定义它们旨在处理可能的失败：条件表达式的功能在于它能够根据成功或失败来执行不同的操作。

### 18.3 模式语法
```rust
// 匹配字面值
let x = 1;

match x {
    1 => println!("one"),
    2 => println!("two"),
    3 => println!("three"),
    _ => println!("anything"),
}

// 匹配命名变量
let x = Some(5);
let y = 10;

match x {
    Some(50) => println!("Got 50"),
    Some(y) => println!("Matched, y = {y}"),
    _ => println!("Default case, x = {x:?}"),
}

println!("at the end: x = {x:?}, y = {y}");

// 多个模式
```rust
let x = 1;
match x {
    1 | 2 => println!("one or two"),
    3 => println!("three"),
    _ => println!("angthing"),
}

let x = 10;
match x {
    // 1..=10 => println!("[1, 10]"),
    1..10 => println!("[1, 10)"),
    _ => println!("angthing"),
}
// 编译器会在编译时检查范围不为空，而 char 和数字值是 Rust 仅有的可以判断范围是否为空的类型，
// 所以范围只允许用于数字或 char 值。
let x = 'a';
match x {
    'a'..='z' => println!("['a', 'z']"),
    'A'..='Z' => println!("['A', 'Z']"),
    _ => println!("angthing"),
}

// 解构 元组 枚举 结构体
// 1.结构体
struct Point {
    x: i32,
    y: i32,
}
fn main() {
    let p = Point { x: 0, y: 1 };
    let Point { x: a, y: b } = p;
    let Point { x, y } = p; // let Point { x: x, y: y } = p;
}
fn main() {
    let p = Point { x: 0, y: 1 };
    match p {
        Point { x, y: 0 } => println!("On the x axis at {x}"),
        Point { x: 0, y } => println!("On the y axis at {y}"),
        Point { x, y } => println!("On neither axis: ({x}, {y})"),
    }
}
// 2.枚举
enum Message {
    Quit,
    Move { x: i32, y: i32, },
    Write ( String, ),
    ChangeColor( i32, i32, i32, ),
}
fn main() {
    let msg = Message::ChangeColor(0, 160, 255);
    match msg {
        Message::Quit => {
            println!("The Quit variant has no data to destructure.");
        }
        Message::Move { x, y } => {
            println!("Move in the x direction {x} and in the y direction {y}");
        }
        Message::Write(text) => {
            println!("Text message: {text}");
        }
        Message::ChangeColor(r, g, b) => {
            println!("Change the color to red {r}, green {g}, and blue {b}");
        }
    }
}


```

```

## CAP 19

### 19.5
宏:
使用 macro_rules! 的 声明（Declarative）宏，和三种 过程（Procedural）宏：

自定义 #[derive] 宏在结构体和枚举上指定通过 derive 属性添加的代码
类属性（Attribute-like）宏定义可用于任意项的自定义属性
类函数宏看起来像函数不过作用于作为参数传递的 token



