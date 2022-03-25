+++
title = "Rust 導引筆記系列 #01 -- Hello world"
description = "學習每種語言的第一支程式往往是 Hello world，相信也已經成為資工的一種傳統了"
date = 2019-09-11
updated = 2019-11-13

[taxonomies]
categories = ["coding"]
tags = ["coding", "rust", "rust-tutorial"]
+++

學習每種語言的第一支程式往往是 Hello world，相信也已經成為資工的一種傳統了。

廢話不多說，先來看看到底怎麼寫

```rust
fn main() {
    println!("Hello, world.");
}
```

從中可以發現，在 Rust 中，函式的定義是用 `fn` 關鍵字定義的，接下來的部份就和C++長得很像了。

接著是 `println!("Hello, world.");` ，不難理解，這是一個輸出用的函式，不，它其實是 macro，也就是巨集。Rust 在呼叫 macro 時，需要在名稱的後面加上 ! 作為識別，往後會有更詳細的解釋。

為了支援 format string，Rust 中有許多格式化字串的部份都會使用 macro

```rust
fn main() {
    println!("Hello, {}.", "Leo");
    // Output: Hello, Leo.
}
```

另外，還有一些類似的 macro

- `println!()`：輸出並換行
- `print!()`：輸出但不換行
- `eprintln!()`：輸出到標準錯誤（stderr）並換行
- `eprint!()`：輸出到標準錯誤（stderr）但不換行

## 編譯

將以上程式碼儲存為 `hellohorld.rs` ，接著開啟 Shell，輸入

```bash
rustc helloworld.rs
./helloworld
```

就完成第一支程式的編寫啦~~~

---

其實這篇文應該會再早幾天出爐的，只是 server 跟神X之塔一樣同時炸開了，所以現在才打好。（這根本是說好的吧...害我什麼事都做不了）

上一篇：[#00 -- 安裝 Rust](@/posts/2019-09-07-rust-tutorial-0.md)

下一篇：[#02 -- 數字加總程式範例](@/posts/2019-09-13-rust-tutorial-2.md)
下一篇：[#02 -- 數字加總程式範例](@/posts/2019-09-13-rust-tutorial-2.md)

[Rust 導引筆記系列目錄](@/pages/2019-09-07-rust-index.md)
[Rust 導引筆記系列目錄](@/pages/2019-09-07-rust-index.md)
