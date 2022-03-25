+++
title = "Rust 導引筆記系列 #11 -- 列舉 (enum)"
date = 2020-04-10
updated = 2020-04-10

[taxonomies]
categories = ["coding"]
tags = ["coding", "rust", "rust-tutorial"]
+++

Rust 中也有 enum，用來表示一系列的數值，與其他語言不太一樣的地方在於：它可以儲存額外的資料。

```rust
enum State {
    Init,
    Success(String),
    Error {	
        message: String,
        code: u32,
    },
}
```

如上所示，每個列舉數值可以帶 Tuple-Like 或 Struct-Like 的資料，這部份有點像 C 的 Enum + Union。這個特質相當有用，在標準函式庫中也大量被使用，其中最重要的就屬 `Option<T>` 和 `Result<T, E>` 了。等一下會介紹 `Option<T>` 的使用，而 `Result<T, E>` 則會有專門的章節講解。

### 以 Option<T> 為範例

在此之前要先介紹這個內建的資料結構

```rust
pub enum Option<T> {
    None,
    Some(T),
}
```

Definition of Option
 在 Rust 中，所有有可能不存在的數值都會用這個類型表現。像是 `Option<u32>` ，代表有可能存在一個整數。我們可以用以下的程式碼來判斷並取出裡面的數值：

```rust
fn a(val: Option<u32>) {
    if let Some(v) = val {
        println!("Value exists: {}", v);
    } else {
        println!("Not exist...");
    }
}
```

除了運用以上方法之外，Rust 還提供了很多其他方式讓我們取出或轉換裡面的資料。因為太多了，所以這邊就不多介紹，可以參考[官方的文件](https://doc.rust-lang.org/std/option/enum.Option.html)，裡面也都有範例可以參考。

### 建構 Enum

```rust
enum State {
    Init,
    Success(String),
    Error {	
        message: String,
        code: u32,
    },
}
```

以最前面定義的 `enum` 為例，當要使用 `enum` 的時候，可以用以下的方法來建構它：

```rust
fn main() {
   let i = State::Init;
   // All of them are `State` type
   let s: State = State::Success("Successful Message".into());
   let e = State::Error {
      message: "Network Error".into(),
      code: 0x02,
   };
}
```

跟 `struct` 的建構方式很像，只是需要另外加上 Enum 的類型名稱，除此之外都和 `struct` 一樣。

### 判斷 Enum 的類型

這和 Rust 的 `match` 有關，因為又是另一個訊息量很大的部份，所以就留到下一篇去了。

---

上一篇：[#10 -- 結構](@/posts/2020-02-01-rust-tutorial-10.md)
上一篇：[#10 -- 結構](@/posts/2020-02-01-rust-tutorial-10.md)

下一篇：待更新...

[Rust 導引筆記系列目錄](@/pages/2019-09-07-rust-index.md)
[Rust 導引筆記系列目錄](@/pages/2019-09-07-rust-index.md)
