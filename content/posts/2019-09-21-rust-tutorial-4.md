+++
title = "Rust 導引筆記系列 #04 -- 基本資料型態 I"
description = "今天就來介紹一下 Rust 的基本資料型態，之前介紹了變數的宣告，也簡介型別的指定方式，該是時候介紹常見的型態了"
date = 2019-09-21
updated = 2019-11-13

[taxonomies]
categories = ["coding"]
tags = ["coding", "rust", "rust-tutorial"]
+++

事情很多又拖了幾天，果然我不適合去打鐵人賽呢 (茶

今天就來介紹一下 Rust 的基本資料型態，之前介紹了變數的宣告，也簡介型別的指定方式，該是時候介紹常見的型態了！

---

## 布林值

Type Annotation: `bool`

```rust
const THIS_IS_TRUE: bool = true;
const THIS_IS_FALSE: bool = false;
```

毫無反應，就是 `true` 和 `false` ...

---

## 整數

Type Annotation:
SizeSignedUnsigned8`i8``u8`16`i16``u16`32`i32``u32`64`i64``u64`128`i128``u128`32/64`isize``usize`
不同大小的整數構成 12 種類型，當沒有特別註明時，會以 `i32` 做為預設類型。另外，Rust 的數字寫法也可以指定類型：

```rust
#![allow(non_snake_case)]
#![allow(unused_assignments)]
#![allow(unused_variables)]

fn main()
{
    let n = 7;
    // You can separate number by '_' to make it more readable!!!
    let separated = 512_151_789;
    let separated2 = 1_0000_0000;

    // Binary Presentation
    let bin = 0b1101_1001;
    // Octal Presentation
    let oct = 0o7122;
    // Heximal Presentation
    let hex: u64 = 0xDEADBEEF_71221234;
    // `u8` Can use Byte Presentation
    let upperA: u8 = b'A';

    // Specify Type
    let l = 50usize;
    let def_val = -100i16;
    let mut verylarge_int = 9_223_372_036_854_775_807i64;

    // OH, NO! Overflow!!!
    verylarge_int += 1;
}
```

可以參考上面的範例程式碼對數字的寫法，而在最後的地方顯然會發生溢位 (Overflow)，遇到此類情形時，程式就會印出訊息然後終止：

```
thread 'main' panicked at 'attempt to add with overflow', src/main.rs:25:5
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace.
```

---

## 浮數點

Type Annotation:

- 32 bits: `f32`
- 64 bits: `f64`

和整數一樣，當沒有特別註明時，會以 `f64` 做為預設類型。

```rust
#![allow(non_snake_case)]
#![allow(unused_assignments)]
#![allow(unused_variables)]

fn main()
{
    let a_float = 7.0;
    let pi = 3.1415926;

    // Some Special Value~~~
    let nan = std::f64::NAN;
    let inf = std::f64::INFINITY;
    let ninf = std::f64::NEG_INFINITY;
}
```

---

## 字元

Type Annotation: `char`

在程式碼以**單引號**括住的單一字元，在這裡有一點要注意！！Rust 的字元乃至字串都一律用 UTF-8 編碼，所以不能直接轉換為 `u8` ，附上一張轉換表

```
    char -------> u32
    ^     -------^
    |    /
    |   /
    |  /
    | /
    u8
```

 在上圖中，只有箭頭所指的方向是保證成功的轉換，逆向轉換皆有可能失敗！

```rust
#![allow(non_snake_case)]
#![allow(unused_assignments)]
#![allow(unused_variables)]

fn main()
{
    let ascii_char = 'a';
    let uppercase_char = ascii_char.to_ascii_uppercase(); // 'A'

    // Unicode Characters Friendly!!!
    let heart = '❤';
    let libra = '♎';
}
```

---

好吧，基本資料型態比想像中多呢，只好分成兩篇了 (累

上一篇：[#03 -- 變數宣告](@/posts/2019-09-17-rust-tutorial-3.md)
上一篇：[#03 -- 變數宣告](@/posts/2019-09-17-rust-tutorial-3.md)

下一篇：[#05 -- 基本資料型態 II](@/posts/2019-09-29-rust-tutorial-5.md)
下一篇：[#05 -- 基本資料型態 II](@/posts/2019-09-29-rust-tutorial-5.md)

[Rust 導引筆記系列目錄](@/pages/2019-09-07-rust-index.md)
[Rust 導引筆記系列目錄](@/pages/2019-09-07-rust-index.md)
