+++
title = "Rust 導引筆記系列 #10 -- 結構 (struct)"
date = 2020-02-01
updated = 2020-04-10

[taxonomies]
categories = ["coding"]
tags = ["coding", "rust", "rust-tutorial"]
+++

在程式語言中，要定義自訂的類型一般都會使用 `struct` 或 `class` 等之類的關鍵字來定義。

#### 定義

在 Rust 中是用 `struct` 這個關鍵字進行定義。基本上寫起來會長這樣：

```rust
struct Time {
    hour: u32,
    minute: u32,
    second: u32,
}
```

每一個欄位都是以 `名稱: 類型` 的方式定義，這個模式跟變數定義 (通常類型會省略) 與函數參數的定義方式是一樣的。

#### 建構

當定義好一個結構之後，就可以用以下方式建立它：

```rust
struct Time {
    hour: u32,
    minute: u32,
    second: u32,
}

fn main() {
    let t = Time {
       hour: 8,
       minute: 12,
       second: 56,
    };
}
```

#### 欄位存取

使用 `obj.field` 的形式去存取欄位的資訊。也可以修改值，但要注意有沒有宣告成 `mut`

```rust
struct Time {
    hour: u32,
    minute: u32,
    second: u32,
}

fn main() {
    let mut t = Time {
       hour: 8,
       minute: 12,
       second: 56,
    };
    println!("{}:{}:{}", t.hour, t.minute, t.second);
    t.second += 1;
    println!("{}:{}:{}", t.hour, t.minute, t.second);
}
```

## Tuple-Like Struct

顧名思義，長得很像 Tuple 的結構，宣告或用起來也很像。

```rust
struct Color(u8, u8, u8);
```

#### 建構&欄位存取

跟上面差不多，只是全部改成 Tuple 的方式：

```rust
struct Color(u8, u8, u8);

fn main() {
    let red = Color(255, 0, 0);
    println!("R:{} G:{} B:{}", red.0, red.1, red.2);
}
```

也可以指定索引值 (index)：

```rust
struct Color(u8, u8, u8);

fn main() {
    let red = Color {0: 255, 1: 0, 2: 0};
    println!("R:{} G:{} B:{}", red.0, red.1, red.2);
}
```

## Unit-Like Struct

跟 `()` 一樣，不具任何欄位的結構：

```rust
struct Context;

fn main() {
    let ctx = Context;
}
```

你說這要用來幹嘛？基本上它需要搭配後面會提到的方法實作，才比較有用。

## 其他建構結構的要點

### 縮寫

當欄位名稱和目前現有的區域變數名稱相同時，可以縮寫

Example:

```rust
struct Time {
    hour: u32,
    minute: u32,
    second: u32,
}

fn main() {
    let hour = 8;
    let minute = 12;
    let t = Time {
       hour: hour,
       minute: minute,
       second: 56,
    };
}
```

`hour: hour` 以及 `minute: minute` 顯得很累贅，可以簡化成：

```rust
struct Time {
    hour: u32,
    minute: u32,
    second: u32,
}

fn main() {
    let hour = 8;
    let minute = 12;
    let t = Time {
       hour,
       minute,
       second: 56,
    };
}
```

### Functional update

呃...我真的不知道該怎麼翻這個...

它可以利用現有的結構，自動填補缺少的欄位資料：

```rust
#[derive(Debug)]
struct Data {
    id: u8,
    value: i32,
    text: String,
}

fn main() {
    let data1 = Data {
        id: 5,
        value: 512,
        text: "some description...".to_owned(),
    };
    println!("{:?}", &data1);
    let data2 = Data { value: 6, ..data1 };
    println!("{:?}", &data2);
}
```

如上範例，我們從舊的 `data1` 只修改了 `value` 建構出了 `data2` 。但要注意的是，若是填補的部分有無法直接複製的型態 (Ex: `String` ...) 時，就會轉移所有權囉。

### 建構函式???

Rust 並沒有這東西，但是會有其他方式可以做到相同的功能。以後會提到

---

Rust 具有許多程式語言的特性，包含函數式、指令式、物件導向等...。而結構是其物件導向的第一步，之後的文章會先往這個方向前進。

上一篇：[#09 -- 迴圈](@/posts/2020-01-17-rust-tutorial-9.md)
上一篇：[#09 -- 迴圈](@/posts/2020-01-17-rust-tutorial-9.md)

下一篇：[#11 -- 列舉](@/posts/2020-04-10-rust-tutorial-11.md)
下一篇：[#11 -- 列舉](@/posts/2020-04-10-rust-tutorial-11.md)

[Rust 導引筆記系列目錄](@/pages/2019-09-07-rust-index.md)
[Rust 導引筆記系列目錄](@/pages/2019-09-07-rust-index.md)
