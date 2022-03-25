+++
title = "Rust 導引筆記系列 #02 -- 數字加總程式範例"
description = "在討論基本語法及型別之前，我想再用一個範例讓大家了解一下要如何處理使用者的輸入以及 Rust 程式的風格"
date = 2019-09-13
updated = 2019-11-13

[taxonomies]
categories = ["coding"]
tags = ["coding", "rust", "rust-tutorial"]
+++

在上一篇的 Hello world 之後，相信大家對於 Rust 的輸出有了一些概念，在討論基本語法及型別之前，我想再用一個範例讓大家了解一下要如何處理使用者的輸入以及 Rust 程式的風格。

這次要寫的程式是從我這學期上的「演算法概論」中，教授給的測試題借來的。

題目輸入給一行以空白分開的整數，輸出他們加總的結果

```text
Sample Input:
1 12 4 56 17 0 3

---
Sample Output:
93
```

看起來很簡單吧～假如用 Rust 寫會是怎麼樣呢？

```rust
use std::io::stdin;

fn main() {
    let mut linebuf = String::new();
    stdin()
        .read_line(&mut linebuf)
        .expect("Failed to read from stdin");
    let sum: i32 = linebuf
        .split_whitespace()
        .filter_map(|s| s.parse::<i32>().ok())
        .sum();
    println!("{}", sum);
}
```


額…這不是很簡單的程式嗎？為什麼看起來那麼複雜啊？相信所有人一開始都會有這個疑問，明明C++寫出來只需要不到10行，為什麼用 Rust 寫會變兩倍長度啊？

以下我就來慢慢解說

---

## 程式碼解說

最上方的 `use std::io::stdin;` 是和 `using std::cin;` 類似的東西，由於 Rust 不像 C++ 把所有東西都丟在 std 下，所以往後經常會用到它，之後會提到更進階的用法。

`let mut linebuf = String::new();` 這裡宣告一個新的字串結構，叫做 linebuf。注意 `mut` 這個關鍵字，在 Rust 中所有的變數預設都是『不可更動（immutable）』的，加上這個關鍵字才能讓變數的資料可以被改變（mutable）。這似乎違反了我們以前學程式的想法，但在實際寫了之後，會發現用到 `let mut` 的次數其實沒想像中的多。很多時候，我們宣告變數只是暫存用，或是代表一個抽象的物件（如：Http Request、Socket…），這個設計可以避免變數被不小心誤改，更加強調安全性。

`stdin().read_line(&mut linebuf).expect("Failed to read from stdin");` 這整段算是一個陳述式。呼叫 `stdin()` 會回傳一個 `Stdin` 物件。再對它使用 `.read_line(&mut linebuf)` 讀取一行並存入方才宣告的字串內。在參數的部份可以看到傳入的是 `&mut` ，它代表一個可變的參考（mutable reference），跟 C 的指標有點像吧 XD。最後的 `.expect("Failed to read from stdin")` 是當發生錯誤時，要提示使用者的訊息，如果成功則會回傳結果。

`let sum: i32 = linebuf.split_whitespace().filter_map(|s| s.parse::().ok()).sum();`

最後，也是最複雜的部份，這裡用到了迭代器（Iterator），如果有 Functional Programming 經驗的人應該會感到蠻熟悉的。先將讀入的數字們用 `.split_whitespace()` 分割後，我們會拿到一個迭代器，對於每個分割出的子字串，我們要把它轉換為整數。但問題是字串並不一定能轉換為整數啊…，所以 `s.parse::<i32>()` 會回傳一個代表成功與否的物件 `Result` 。在此，使用 `.ok()` 來取得成功轉換為數字的部份， `filter_map` 就會負責過濾掉。至此，我們已經取得內含整數型別的迭代器了，最後對整串迭代器用 `.sum()` 取得全部的加總，輸出之後就完成了~~

---

雖然 Rust 比起其他語言更加複雜一些，但卻能在追求效能的同時兼顧安全性，不會出現未定義行為（Undefined behavior）。另外，這次出現的迭代器也是 Rust 中被廣泛運用的部份，可以方便操作大量物件並增加程式碼可讀性，卻不會額外消耗過度資源。

附上本文程式碼：[https://play.rust-lang.org/?version=stable&mode=debug&edition=2018&gist=bb79c09075527fc078eaf631784b4746](https://play.rust-lang.org/?version=stable&amp;mode=debug&amp;edition=2018&amp;gist=bb79c09075527fc078eaf631784b4746)

上一篇：[#01 -- Hello world](@/posts/2019-09-12-rust-tutorial-1.md)

下一篇：[#03 -- 變數宣告](@/posts/2019-09-17-rust-tutorial-3.md)
下一篇：[#03 -- 變數宣告](@/posts/2019-09-17-rust-tutorial-3.md)

[Rust 導引筆記系列目錄](@/pages/2019-09-07-rust-index.md)
[Rust 導引筆記系列目錄](@/pages/2019-09-07-rust-index.md)
