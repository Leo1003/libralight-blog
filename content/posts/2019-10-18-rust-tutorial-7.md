+++
title = "Rust 導引筆記系列 #07 -- 借用與參照"
date = 2019-10-18
updated = 2019-11-13

[taxonomies]
categories = ["coding"]
tags = ["coding", "rust", "rust-tutorial"]
+++

## 物件借用 (Borrowing)

在上次的最後有提到，若是要暫時取得某個不具 `Copy` 特徵的使用權，就要把所有權傳給該函式，再傳回來。

這顯然不是一個好方法，所以這次就要來介紹「借用」這個機制。

與 C 語言中的取址運算子相當類似，在一個變數的前方加上 `&` 符號，就可以借用一個變數的數值。在 Rust 中，「借用」會讓我們拿到一個參照，指向被借用的數值。

利用這個機制，我們可以將上一篇最後的範例改為

```rust
fn print_string(s: &String) {
    println!("The string is: {}", s);
}

fn main() {
    let str = String::from("My name is Leo, nice to meet you.");

    print_string(&str);
    // Only borrow the string here,
    // we still hold the ownership.

    println!("{}", &str);
}
```

這樣就只需要傳一個指向 `String` 的參照進去，不會導致所有權的轉移。

## 參照 (Reference)

在「借用」一個數值之後，會拿到一個參照，參照究竟是什麼呢？

概念上，參照和指標是極為相似的東西，他們都指向某個在記憶體中存在的數值，可以避免大量數值的複製、修改存在於其他位置的數值等。

但這也是許多棘手問題的根源，因此，Rust 在參照上加了一些限制，若是違反了這些規則，就會導致編譯失敗 (Compile Error)。

- 參照必須永遠指向有效的數值
- 若要更動數值，必須使用**可變參照**（用 `&mut str` 取得可變參照）
- 對於每個數值，同時間只能有**多個不可變參照**或是**一個可變參照**指向它

### 參照有效性

在使用或回傳時，參照不能指向已經被拋棄的數值

```rust
fn dangling() -> &mut String {
    let s = String::new();

    // Return a reference of the string
    &mut s
} // But the string dropped here!!!
```

上面的範例中，回傳了一個參照指向只存活在這個函式中的 `String` ，因此在離開函式之後，它就變成一個無效的參照了！

除此之外，在 `if` / `while` 迴圈等區塊內部宣告的變數也要注意這個問題喔，Rust 是以大括號作為變數存活區域的判斷喔！！

### 可變參照

Rust 的參照也是有不可變 / 可變之分，預設我們只會拿到不可變參照 (immutable reference)，若要修改數值，就要用 `&mut` 取得可變參照 (mutable reference)，另外，原本的變數也必須宣告為**可變變數**才能取它的可變參照喔。

```rust
fn append_world(s: &mut String) {
    s.push_str(", world");
}

fn main() {
    let mut s = String::from("Hello");
    append_world(&mut s);

    // Print: Hello, world
    println!("{}", &s);
}
```

在使用可變參照時，有時候會需要重新指派數值，在這種情況下，就會需要在參照前加上 `*` 運算子 (Dereference Operator)，來強制表示我們是要對數值進行操作。

```rust
fn set_to_five(num: &mut i32) {
    *num = 5;
}

fn main() {
    let mut n = 150;

    // Print: 150
    println!("{}", n);

    set_to_five(&mut n);

    // Print: 5
    println!("{}", n);
}
```

### 可變參照互斥性

為了避免**資料競搶** (Data Race) [[1]](#note)，Rust 限制每個數值同時只能有**多個不可變參照**或是**一個可變參照**指向它，以上的兩種狀況是**互斥**的，當有可變參照存在時，就不可以有不可變參照；反之亦然。

以下是剛剛出現的範例，但這次同時存在兩種參照，所以會編譯失敗！

```rust
fn append_world(s: &mut String) {
    s.push_str(", world");
}

fn main() {
    let mut s = String::from("Hello");

    // Get a reference here
    let s_ref = &s;
    // Get a mutable reference here
    // Oops, mutable & immutable reference
    // exists at the same time!!!
    append_world(&mut s);

    // Print: Hello, world
    println!("{}", s_ref);
}
```

---

以上的三種限制是 Rust 對於避免參照被不當的使用，進而導致各種非預期行為所作的保護。對於經常編寫 C++ 的人來說，這些都是一些基本的安全原則，但是卻沒有被語言強制要求遵守。

對於新進入 Rust 的人可能會對於經常性的編譯失敗很感冒，但是我必須說，後續的除錯往往才是更消耗時間與精力的。試著改變寫程式的習慣與觀念吧！

---

## Note

1. **資料競搶** (Data Race) 與 **競搶條件** (Race Condition) 是不同的東西，詳情請看 [Race Condition vs. Data Race](https://blog.regehr.org/archives/490) (English)

---

終於來到參照了呢，在接下來會轉而細談 Rust 的判斷、迴圈與資料結構。

也許會有人關心多執行緒程式的撰寫，基於以上的規則，可以避免往後發生**資料競搶**與**競搶條件**，不過也需要一些額外的同步方式才能寫入資料到共有的變數就是了…

上一篇：[#06 -- 物件所有權](@/posts/2019-10-09-rust-tutorial-6.md)
上一篇：[#06 -- 物件所有權](@/posts/2019-10-09-rust-tutorial-6.md)

下一篇：[#08 -- if 條件敘述](@/posts/2019-11-14-rust-tutorial-8.md)
下一篇：[#08 -- if 條件敘述](@/posts/2019-11-14-rust-tutorial-8.md)

[Rust 導引筆記系列目錄](@/pages/2019-09-07-rust-index.md)
[Rust 導引筆記系列目錄](@/pages/2019-09-07-rust-index.md)
