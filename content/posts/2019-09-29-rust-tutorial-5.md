+++
title = "Rust 導引筆記系列 #05 -- 基本資料型態 II"
description = "接續上次沒寫完的部份...補充一下陣列或複合類的資料結構吧"
date = 2019-09-29
updated = 2019-11-13

[taxonomies]
categories = ["coding"]
tags = ["coding", "rust", "rust-tutorial"]
+++

接續上次沒寫完的部份...補充一下陣列或複合類的資料結構吧

---

## 陣列

Type Annotation: `[T; n]`

Rust 的陣列必須是固定長度的物件，當長度小於等於32時，就會幫你實作一些功能（比較、預設值…等）

```rust
fn main()
{
    let prices = [25, 35, 40, 50, 55];

    // Initialize 8 elements as 100
    let array = [100; 8];

    // 64 Bytes Empty Binary Data
    let bin = [0u8; 64];

    // Access index 1
    println!("Price of the second item: {}", prices[1]);

    // Decompose array elements
    let colors = ["Red", "Yellow", "Green", "Blue"];
    let [red, yellow, green, blue] = colors;
    println!("My favorite color: {}", green);
}
```

---

## 切片（Slice）

Type Annotation: `[T]`

其實大部分都叫 slice 啦…但是前面的標題都寫中文，所以就...

和陣列不同，slice並沒有固定的大小，因此不能夠直接宣告出來，且通常會搭配上參照（Reference）一起出現 `&[T]` 。基本上是作為陣列一類物件的參照用的。

```rust
fn main()
{
    let prices = [25, 35, 40, 50, 55];
    
    // A slice to the array
    let prices_ref: &[i32] = &prices;
    println!("{:?}", prices_ref);
    
    // A slice to the first 3 items of the array
    // [0, 3)
    let prices_ref: &[i32] = &prices[0..3];
    println!("{:?}", prices_ref);
    
    // A slice to the 2nd ~ 4th items of the array
    // [1, 3]
    let prices_ref: &[i32] = &prices[1..=3];
    println!("{:?}", prices_ref);
}
```

利用 slice，可以方便的取得子陣列（subarray），就不用什麼 `start` 、`end` 這種變數啦。

---

## 元組（Tuple）

Type Annotation: `(T, U, V)`

還是一樣，其實大部分都叫 tuple 啦…但是前面...

有寫過 Python 的朋友們應該都很清楚這東西，基本上就是一種可以組合不同類型物件的資料類型。

```rust
fn main()
{
    let mut char_count: (char, usize) = ('A', 12);
    
    // Access element
    char_count.1 += 1;
    
    // Destruct tuple
    let (c, cnt) = char_count;
    
    println!("Char '{}', appeared {} times.", c, cnt);
}
```

在存取時，最左方的元素從0開始編號，如上面所示，第2個元素的編號就是1，依此類推...

---

## 單元（Unit）

Type Annotation: `()`

這算是 Rust 獨有的資料類型，跟 C/C++ 中的 `void` 有些類似，但又不完全一樣。它在程式實際運作中不消耗任何記憶體，可以被建構、放進類別參數，以達到抽象化的目的。

舉個例子：大部分的人都知道 Map 和 Set 是類似的資料結構，Map 可以為每個鍵值指派一個值，但 Set 的鍵值則是不帶值。這時，Unit 的使用可以讓 Set 的定義變成： `BTreeSet<T> = BTreeMap<T, ()>` ，嗯…計畫通 XD

---

這週又再趕社課和弄作業啦，當幹部真是忙，不過我還是會繼續寫的。接下來，是時候進入到 Rust 的核心 – 物件所有權與生命週期了。

上一篇：[#04 -- 基本資料型態 I](@/posts/2019-09-21-rust-tutorial-4.md)
上一篇：[#04 -- 基本資料型態 I](@/posts/2019-09-21-rust-tutorial-4.md)

下一篇：[#06 -- 物件所有權](@/posts/2019-10-09-rust-tutorial-6.md)
下一篇：[#06 -- 物件所有權](@/posts/2019-10-09-rust-tutorial-6.md)

[Rust 導引筆記系列目錄](@/pages/2019-09-07-rust-index.md)
[Rust 導引筆記系列目錄](@/pages/2019-09-07-rust-index.md)
