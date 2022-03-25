+++
title = "Rust 導引筆記系列 #08 -- if 條件"
date = 2019-11-13
updated = 2020-01-21

[taxonomies]
categories = ["coding"]
tags = ["coding", "rust", "rust-tutorial"]
+++

## `if` 判斷式

雖然說在前面應該有見過不少次了，但還是要正式介紹一下。

Rust 的 `if` 判斷式大致上長這樣：

```rust
if a > 20 {
    do_something();
} else {
    do_otherthing();
}
```

整體上，看起來和目前主流的大多數語言都是一樣的語法。不過還是有許多地方是不同的，最明顯之處莫過於 `if` 之後的條件判斷是不需要加括號的，而且編譯器也會建議你拿掉它。

至於另一個不同之處也是我喜歡 Rust 的原因之一。

## 陳述式與表達式 (Statement & Expression)

一般在程式語言中，語句會分為兩類：陳述式與表達式，兩者的概念大概是這樣：

- **陳述式**：代表去執行一件工作，不會具有回傳值。在 C/C++/Java 等語言中，分號就是一個陳述式的結尾。
- **表達式**：程式中的一部分往往可以計算出一個值，像是 `a < b` 、 `100` 、 `8 + 5` 等等，被稱為表達式。

Rust 和大多數語言的其中一點不同是，它是一個以表達式為主的語言，絕大多數的語句都是一個表達式，而表達式更可以由

```
    陳述式
    陳述式
    陳述式
    ...
    表達式
```

所組成，最後面只能存在一個表達式，若沒有表達式的話，就會視為回傳了一個單元 (Unit) `()` 哦！

基本上，在 Rust 中，兩者最簡單的區分方式就是看有沒有分號啦～

### 作為表達式的 `if`

說了那麼多， `if` 要如何作為表達式呢？以下就用一個判斷成績等第的範例展示一下吧～

```rust
use std::io::stdin;

fn main() {
    print!("Enter your grade: ");
    let mut linebuf = String::new();
    stdin()
        .read_line(&mut linebuf)
        .expect("Failed to read from stdin");
    let score: i32 = linebuf
        .split_whitespace()
        .filter_map(|s| s.parse::<i32>().ok())
        .next()
        .unwrap();
    let grade_char = if score >= 90 {
        println!("Nice Job!!!"); // <-- Statement
        'A' // <-- Expression
    } else if score >= 80 {
        'B'
    } else if score >= 70 {
        'C'
    } else if score >= 60 {
        'D'
    } else {
        println!("Oh, no!");
        'F'
    }; // Don't forget to put a semicolon here!!!
       // Since it's a part of the `let` statement.

    println!("Your grade is {}.", grade_char);
}
```

整個 `if` 變成了一個表達式，每個條件的大括弧都是一個表達式，在裡面的字元也是一個表達式。最後再回傳並賦值給 `grade_char` 變數。值得注意的是，在這種用法之下，一定要有 `else` 的部份，才能保證所有路徑都有回傳值。

這種用法其實就像 `(a > b ? a : b)` ，但相較之下更加直觀與美觀，也因此 Rust 並沒有 `? :` 這個三元條件運算子。

---

這次因為期中考的關係斷更了一個月，還請多多見諒…

上一篇：[#07 -- 借用與參照](@/posts/2019-10-18-rust-tutorial-7.md)
上一篇：[#07 -- 借用與參照](@/posts/2019-10-18-rust-tutorial-7.md)

下一篇：[#09 -- 迴圈](@/posts/2020-01-17-rust-tutorial-9.md)
下一篇：[#09 -- 迴圈](@/posts/2020-01-17-rust-tutorial-9.md)

[Rust 導引筆記系列目錄](@/pages/2019-09-07-rust-index.md)
[Rust 導引筆記系列目錄](@/pages/2019-09-07-rust-index.md)
