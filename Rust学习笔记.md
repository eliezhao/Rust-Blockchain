### Cargo：

cargo.toml是像java的maven中的包管理工具一样， 可以在depencies中加上需要用的依赖：

```plain
[dependencies]
ferris-says = "0.2"
```
编好包之后， 可以用
```plain
cargo build
```
下载依赖到本地
### Rust语法：

```rust
println!("a is {0}, b is {1} {{!}}", 0, 1);
```
输出字符， 前面是格式化字符串， 后面是对应的参数列表。 {}可以认为是一个数组， {}内的数字可以认为是变量数组的下标。
输出特殊字符,用{}包括， {{!}} = {!}

不可变变量与变量：

```rust
let a = 123
a = 456 错
let a = 456 对

let mut a = 1
a = 2 对
```
第二条语句不合法， 第三条语句合法。
因为let声明的是不可变变量， 这种变量只能重新绑定，而不能直接修改。

在Rust中， 变量有类型推理的功能，但是如果要特殊绑定，比如u64是uint 64，如果默认赋值，就是u32，而不是64.

第四条语句， 加上了mut关键词， 就可以对变量进行改变。

重影：

重影是指用同一个名字重新代表另一个变量实体，其类型、可变属性和值都可以变化

数据类型：

整数型：

```plain
有符号 : i8 ， 无符号 : u8
```

浮点型：

```plain
let x = 2.0; // f64
let y: f32 = 3.0; // f32
```
默认64位， 可以声明f32为32位
运算：

Rust支持 + - * /  %

不支持 ++ 和 --

boolean： true || false

元组 Tuple:

```plain
let tup: (i32, f64, u8) = (500, 6.4, 1);
tup.0 = 500 : i32
```

数组Array：

```plain
let a = [1, 2, 3, 4];
```
声明：
长度为5，每个元素为i32的数组

```plain
let c: [i32; 5] = [1, 2, 3, 4, 5];
```

```plain
let d = [3; 5]; 等同于 [3,3,3,3,3]
访问 ： d[0] -> 3
```

修改数组：

```plain
let c = [3;4] ->(3,3,3,3)
c[0] = 1 错
let mut c = [3;4]; -> (3,3,3,3)
c[0] = 1 对
```

注释：Rust的注释支持类似Markdown的注释

```plain
//
或者
/**
* #docs
* xxx
*/
```
cargo doc可以将代码中的说明文档转换为类似于Html的说明文档
函数

```plain
fn exper(x : i32, y : i32){
    println!("{0}, {1}", x, y)
}
exper(1,2)
-> "1, 2"
```
函数体表达式：
```plain
fn main() {
    let x = 5;
    let y = {
        let x = 3;
        x + 1
    };
    println!("x 的值为 : {}", x);
    println!("y 的值为 : {}", y);
}
```
和Motoko一样， 可以用一个表达式进行函数表达，注意，变量可能会造成覆盖。let就不会嘿嘿嘿嘿。
Rust中， 函数可以嵌套：

```plain
fn main() {
    fn five() -> i32 {
        5
    }
    println!("five() 的值为: {}", five());
}
在参数声明之后用 -> 来声明函数返回值的类型
```
返回值也可以用return xxx;
注意： 函数体表达式不能return，因为他不是一个函数，只是一个执行过程，要用xxx一个式子直接返回结果；Rust不支持函数类型自动判断，要声明返回值的类型

 

条件判断：

```plain
if boolean {}
else if boolean {}
else {}
在C/C++ 中， 非0值代表true， 这在Rust中是不支持的。
```
在Go语言中的三元表达符：
```plain
for i := 0; i < 0; i++ {  }
```
Rust中需要用while代替：
```plain
let mut i = 0;
while i < 0 {
  i += 1;
}
```
在Rust中， for循环和python非常像 ： 
```plain
let a = [3;5];
for i in a.iter(){ println!("value is {0}", i) }
```
eg：
```plain
fn main() {
let a = [10, 20, 30, 40, 50];
    for i in 0..5 {
        println!("a[{}] = {}", i, a[i]);
    }
}
```

无限循环 ： loop: 比while简洁一点

```plain
let ch = [1,2,3,4,5,6];
let mut i = 0;
let location = loop {
 c = ch[i]
 if c == 4 { break i};
 println!("number is {}", c);
 i += 1;
}
```
在这段代码中， loop可以作为一个代码段，赋值给一个变量，在此处为location。
Rust核心之  所有权， 移动， 克隆， 引用（可变 || 不可变）， 租借

所有权和可变引用一样可以读写

直接 s1 = xxx , s2 = s1，会发生所有权转让。

s1不再拥有对xxx的所有权， 所有对s1的引用都需要转到s2

克隆是对值的复制， 租借是不可变引用，


结构体实例化：

```plain
let runoob = Site {
    domain: String::from("www.runoob.com"),
    name: String::from("RUNOOB"),
    nation: String::from("China"),
    found: 2013
};
```

Rust接口：

trait:

```plain
trait Summery {
  fn summerize(&self) -> string;
  //xxx
}

struct article {
  article_title : string,
  article_body : string,
}

impl Summery for article{
  fn summerize(&self) -> string {}
}
```

所有权：

所有权的转移只会在堆内存指针进行赋值的时候。

不可变引用被认为是读者，所以没有数量限制， 可变引用有两个限制：

第一个是，对一个变量，  不能在不可变引用的作用范围内存在可变引用

第二个是， 对一个变量， 不能存在两个及以上的可变引用。即一个变量只能有一个可变引用。


### 异常处理：

对于可恢复错误用 Result<T, E> 类来处理，对于不可恢复错误使用 panic! 宏来处理。



### 数据结构：

```plain
HashMap:
// hashmap initralize
use std::collections::HashMap;
let mut scores = HashMap::new();
scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Yellow"), 50);

//hashmap element refresh
// if key has existed, refresh value, or insert
map.insert(key, value) 

//if key existed, do nothing, or insert key and value
map.entry(key).or_insert(value)

//hashmap ownership
use std::collections::HashMap;
let field_name = String::from("Favorite color");
let field_value = String::from("Blue");
let mut map = HashMap::new();
map.insert(field_name, field_value);
// 这里 field_name 和 field_value 不再有效，使用的话会出现编译错误

//traverse hashmap
for (key, value) in &map { //pay attention to the ownership of map
  println!("{0}, {1}", key, value);
}

```


Rust - crate

```plain
mod back_of_house {
    pub struct Breakfast {
        pub toast: String, //pub结构体字段默认还是私有 因此需要pub才可访问
        seasonal_fruit: String, //对私有字段 需要用impl一个函数来更改
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
```

panic

```plain
// ? 可以替代match，如果是error，?返回error，ok则向下执行
// ？ 用于返回结果为Result<T, Error>的函数， fn test() -> Result<T, //io::Error>

let mut m = File::open("second.txt")?;
let mut n = String::new();
m.read_to_string(&mut n)?;
Ok(n);

let mut s = String::new();
File::open("third.txt")?.read_to_string(&mut s)?;
Ok(s);

```


泛型编程：

```plain
比如 fn higher:
pub fn higher<T>(list : [T]) -> T {}

//多参数 结构体泛型
struct Point<T, U> {
    x: T,
    y: U,
}
fn main() {
    let both_integer = Point { x: 5, y: 10 };
    let both_float = Point { x: 1.0, y: 4.0 };
    let integer_and_float = Point { x: 5, y: 4.0 };
}

//结构体中的泛型
struct Point<T> {
    x: T,
    y: T,
}
impl<T> Point<T> {
    fn x(&self) -> &T {
        &self.x
    }
}

性能 ： 单态化 ： 编译时用什么类型就转换为什么类型。即编译时泛型具有具体类型

```


trait：

```rust
trait是行为的集合，类似于java中的interface
每个实现trait的类型都要实现trait中对应的方法体：
// summary trait :
pub trait Summary {
    fn summarize(&self) -> String;
}
//trait中的方法签名都由 ";" 结尾
//具体用法
pub struct NewsArticle {
    pub headline: String,
    pub location: String,
    pub author: String,
    pub content: String,
}
impl Summary for NewsArticle {
    fn summarize(&self) -> String {
        format!("{}, by {} ({})", self.headline, self.author, self.location)
    }
}
pub struct Tweet {
    pub username: String,
    pub content: String,
    pub reply: bool,
    pub retweet: bool,
}
impl Summary for Tweet {
    fn summarize(&self) -> String {
        format!("{}: {}", self.username, self.content)
    }
}

//可以对trait方法进行默认实现， 在struct进行Impl trait的时候， 可以重载或者不重载

pub trait Summary {
    fn summarize(&self) -> String {
        String::from("(Read more...)")
    }
}

//默认实现允许调用相同 trait 中的其他方法，哪怕这些方法没有默认实现。
pub trait Summary {
    fn summarize_author(&self) -> String;
    fn summarize(&self) -> String {
        format!("(Read more from {}...)", self.summarize_author())
    }
}

//统一调用实现了trait的struct等类型 : apub fn notify(item: impl Summary) {
    println!("Breaking news! {}", item.summarize());
}

//参数类型不同
pub fn notify(item1: impl Summary, item2: impl Summary) {
//参数类型相同
pub fn notify<T : Summary>(item1: T, item2: T) {

//实现多个trait:
pub fn notify(item: impl Summary + Display) {

//类泛型函数签名写法：
fn some_function<T: Display + Clone, U: Clone + Debug>(t: T, u: U) -> i32 {

//函数返回trait:
fn realize(params) -> imple Summary{}



Rust测试函数
需要在 fn 行之前加上 #[test]。当使用 cargo test 命令运行测试时，Rust 会构建一个测试执行程序用来调用标记了 test 属性的函数，并报告每一个测试是通过还是失败。

//eg:
#[cfg(test)]
mod tests {
        //test注解
        #[test]
    fn it_works() {
        //断言宏
        assert_eq!(2 + 2, 4);
    }
}

//测试的时候： 
cargo test

//结果：
//注意 measure 是针对性能测试的 
test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.00s
   Doc-tests addr
running 0 tests
test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.00s



//一个有效的assert断言书写
//assert断言是为了验证程序在按照既定的规则运行
#[test]
fn greeting_contains_name() {
    let result = greeting("Carol");
    assert!(
        result.contains("Carol"),
        "Greeting did not contain name, value was `{}`", result
    );
}



//Panic检测 如果报panic则会通过，如果不报panic则不会通过
pub struct Guess {
    value: i32,
}
impl Guess {
    pub fn new(value: i32) -> Guess {
        if value < 1 || value > 100 {
            panic!("Guess value must be between 1 and 100, got {}.", value);
        }
        Guess {
            value
        }
    }
}
#[cfg(test)]
mod tests {
    use super::*;
    #[test]
    #[should_panic]
    fn greater_than_100() {
        Guess::new(200);
    }
}

//2021 0717 19 42 -> 21 .00



```







