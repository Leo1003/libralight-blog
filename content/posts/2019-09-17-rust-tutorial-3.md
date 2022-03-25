+++
title = "Rust 導引筆記系列 #03 -- 變數宣告"
description = "Rust 的變數宣告使用 let 關鍵字"
date = 2019-09-17
updated = 2019-11-13

[taxonomies]
categories = ["coding"]
tags = ["coding", "rust", "rust-tutorial"]
+++

## 變數宣告

Rust 的變數宣告使用 `let` 關鍵字，像這樣：

```rust
let x = 5;
```

從之前的部份應該不難發現，Rust 是強型別語言，但是為什麼宣告時都沒有指定型別呢？其實 Rust 編譯器會自動判斷變數的類型，在大部分情況都不需要程式設計師去明確指定 (當然你也可以手動去指定它，但其實沒必要...)。但若是遇到編譯器無法判斷的時候，則會出現編譯錯誤，要求手動指示：

```rust
// Same as above
let x: i32 = 5;

// Can't determine should parse to which type...
// We need to specify it!
let num: i32 = "7122".parse().expect("Not a number!");
```

## 可變性 (Mutability)

在預設的情況下，所有的變數及參數都是不可變的 (immutable)，跟 C/C++ 宣告成 `const` 是類似的

```rust
fn main() {
    let x = 5;
    println!("x = {}", x);
    x += 1; // Compile Error: cannot assign twice to immutable variable x
    println!("x = {}", x);
}
```

要避免這個錯誤的方法就是在變數名稱前面加上 `mut` 關鍵字，使其，編譯器才會允許你更動它的值

```rust
fn main() {
    let mut x = 5;
    println!("x = {}", x);
    x += 1;
    println!("x = {}", x);
}
```

最後提醒一下，這裡介紹的變數宣告只適用於區域變數。由於全域變數會在多執行緒的情況下出現競搶條件 (Race condition)，是 Rust 所不允許的行為，所以全域變數的宣告方式不同且使用方式會嚴格得多，以後會再說明。

## 常數宣告

在 Rust 中也有常數，與變數宣告類似，但使用的關鍵字為 `const`，方式如下：

```rust
const ONE_MILLION: i32 = 1000000;
const HELLO_STRING: &str = "Hello, world";
const TEN_MILLION: i32 = 10 * ONE_MILLION;
```

在常數宣告中，變數型別是**必須**要明確定義的，編譯器不會幫你自動判斷！

你可能會想問常數與不可變的變數究竟有何不同？以下就來整理這兩者之間的差異：

- 常數的數值是在編譯時期就計算好的固定值；變數的數值則為執行時期去計算出的結果（不討論編譯器優化的狀況）
- 常數可以被宣告在全域；變數則只能宣告於某個執行區域內
- 常數的賦值來源只能為能夠在編譯時期得出數值的表達式（稱作常數表達式）
- 編譯器會要求常數名稱不應出現小寫英文字母（編譯警告）
- 最後一點是給對記憶體佈局有認識的人：常數會位於 `.rodata` 中；變數會位在 `stack` 上或 `register` 裡

---

基本上，從變數就可以看出 Rust 的設計會不讓你生出 bug 百出的 toy program，所以趕時間的時候不要用 Rust 寫程式，不然絕對會被搞死的 XD

上一篇：[#02 -- 數字加總程式範例](@/posts/2019-09-13-rust-tutorial-2.md)
上一篇：[#02 -- 數字加總程式範例](@/posts/2019-09-13-rust-tutorial-2.md)

下一篇：[#04 -- 基本資料型態 I](@/posts/2019-09-21-rust-tutorial-4.md)
下一篇：[#04 -- 基本資料型態 I](@/posts/2019-09-21-rust-tutorial-4.md)

[Rust 導引筆記系列目錄](@/pages/2019-09-07-rust-index.md)
[Rust 導引筆記系列目錄](@/pages/2019-09-07-rust-index.md)
