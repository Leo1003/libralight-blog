---
title: Rust 導引筆記系列 #06 -- 物件所有權 (Ownership)
slug: rust-tutorial-6
date_published: 2019-10-09T06:41:54.000Z
date_updated: 2019-10-18T05:54:17.000Z
tags: Rust, #RustTutorial, Coding
---
+++
title = "Rust 導引筆記系列 #06 -- 物件所有權 (Ownership)"
description = "從本篇開始，就要進入核心部份了..."
date = 2019-10-09
updated = 2019-10-18

[taxonomies]
categories = ["coding"]
tags = ["coding", "rust", "rust-tutorial"]
+++

從本篇開始，就要進入核心部份了。所有權的傳遞與借用是 Rust 能在記憶體安全性保證之下卻又不需要垃圾回收器 (Garbage Collector, GC) 的關鍵，也是讓效能足以與 C++ 批敵的特性。

**溫馨提醒：本篇具有一些重要觀念，請帶著清楚的腦袋閱讀～～**

---

## 值與變數與所有權 (Value & Variable & Ownership)

首先，我們需要建立一個觀念：變數和它所帶有的值是不同的兩種東西

變數就如同是一個容器，可以在裡面裝入數值（內容物）。當在讀取變數時，實際上是在讀取內容物的部份，因此，變數必須要有數值才可以讀取它。在一般使用上，也是如此。

在 Rust 中，每個數值（內容物）都對應到一個變數（容器），這個變數稱作「擁有者」(Owner)；也就是每個數值只能被裝在一個變數裡。

當變數脫離它的作用域時，裡面的數值就會自動**被捨棄** (dropped)，這個變數也不再能被使用。

```rust
fn main() {
    {
        let lucky = 1003; // This variable is in scope

        println!("My lucky number is: {}", lucky);
    } // <--- The variable goes out of scope here!

    //println!("My lucky number is: {}", lucky);
    // It's invalid to use the variable here,
    // and you will get a compile error if you do that!!!
}
```

---

## 記憶體分配 (Memory Allocation)

在程式中，往往會有一些類型需要動態配置記憶體。俗話說，「有借有還，再借不難」，這道理大家都知道，但有時就是會有意外或忘記的時候，就會造成所謂的記憶體洩漏 (Memory Leak)。

Rust 在變數脫離作用域時捨棄數值的時候，會對具有 `Drop` 特徵的物件，呼叫一個類似解構子 (destructor) 的函式，稱為 `drop()` 。如此一來，就可以在裡面將記憶體釋放，或是將一些清理的工作放在其中，搭配上其他所有權的規則，就能完美的避開這類問題了～

P.s. 關於特徵的部份未來會再說明…

### 字串 (`String`)

`String` 是一種動態配置的字串物件，由於牽涉到記憶體分配，它也是具有 `Drop` 特徵的物件之一。和上面有87%像的範例：

```rust
fn main() {
    {
        let name = String::from("Leo"); // This variable is in scope
        // And it also allocated some memory

        println!("My name is: {}", &name);
    } // <--- The variable goes out of scope here!
      // drop() would be called before the value dropped.
      // Thus, String can free the memory!!!

    //println!("My name is: {}", &name);
    // It's invalid to use the variable here,
    // and you will get a compile error if you do that!!!
}
```

在字串變數脫離空間之後，就會執行 `drop()` ，將配置的記憶體釋放，再將堆疊上的資料釋放。

---

## 所有權轉移

在 Rust 中，數值的指派、傳遞都會使數值的所有權轉移到另一個變數上。

```rust
fn main() {
    // String will transfer when reassigned
    let name = String::from("Leo");

    let name2 = name;
    // Now, `name` lost its ownership.
    // You can't use it anymore!

    //println!("My name is: {}", &name);
    // It's invalid to use the variable,
    // But you can use the new variable with the ownership

    println!("My name is: {}", &name2);
}
```

除了**具有 `Copy` 特徵的型態會自動進行複製**（整數、浮點數、布林…等基本型態）。其他的型態除非明確指示要複製它（前提是可被複製），否則都會發生所有權的轉移。

### 函式與所有權轉移

函數的參數與回傳也會導致所有權轉移的發生，直接看範例比較快：

```rust
fn print_string(s: String) -> String {
    println!("The string is: {}", &s);

    // Return the string here,
    // it makes the ownership move back!
    s
}

fn main() {
    // String will transfer when reassigned
    let str = String::from("My name is Leo, nice to meet you.");

    let ret_str = print_string(str);
    // Now, `name` gave its ownership to the function.
    // You can't use here anymore!
    // But we got the same data back, stored in `ret_str`

    println!("{}", &ret_str);
}
```

我們建構了一個字串 `str` ，在將它傳入 `print_string()` ，這個函式時，所有權就轉移到其中的參數 `s` 了。但是在最後，可以發現到，它也把同一個字串傳回來了。在 `main()` 函式中用另一個變數儲存回傳值就可以將原字串取回來。

但是這樣的作法顯得相當累贅，顯然有比較好的方式可以完成這件事。在保留所有權的狀態下，讓其他函式或程式碼使用某個數值就會需要參照 (Reference) 了。就留到下一篇再說吧～

---

上一篇：[#05 -- 基本資料型態 II](@/posts/2019-09-29-rust-tutorial-5.md)
上一篇：[#05 -- 基本資料型態 II](@/posts/2019-09-29-rust-tutorial-5.md)

下一篇：[#07 -- 借用與參照](@/posts/2019-10-18-rust-tutorial-7.md)
下一篇：[#07 -- 借用與參照](@/posts/2019-10-18-rust-tutorial-7.md)

[Rust 導引筆記系列目錄](@/pages/2019-09-07-rust-index.md)
[Rust 導引筆記系列目錄](@/pages/2019-09-07-rust-index.md)
