+++
title = "Rust 導引筆記系列 #09 -- 迴圈"
description = "迴圈是程式中不可或缺的一部分，在 Rust 中，迴圈就有三種。雖然大部分情況下是可以用不同種的迴圈去做到一樣的效果，但會影響寫程式的效率及程式碼的美觀。因此，這三種迴圈都有其適用的情況。"
date = 2020-01-17
updated = 2020-02-01

[taxonomies]
categories = ["coding"]
tags = ["coding", "rust", "rust-tutorial"]
+++

@_@ 終於把幾何造形的期末專題弄完了，寒假現在才正式開始了...

迴圈是程式中不可或缺的一部分，在 Rust 中，迴圈就有三種。雖然大部分情況下是可以用不同種的迴圈去做到一樣的效果，但會影響寫程式的效率及程式碼的美觀。因此，這三種迴圈都有其適用的情況。

---

## 無限迴圈 -- `loop`

```rust
fn main() {
    loop {
        doSomething();
    }
}
```

毫無反應，就是一個無限迴圈。除了用 `break` 之外，沒有辦法讓它跳離迴圈，最好確認一下有沒有適當的 `break` 條件。

在 Rust 中， `loop` 有一個特別之處，就是裡面所用的 `break` 是可以回傳值的

```rust
fn main() {
    let result = loop {
        let x = doSomething();
        if x > 100 {
           break x;
        }
    }; // <-- semicon here!
    println!("{}", result);
}
```

接在 `let` 的時候別忘了分號喔～ 反正忘了就只是編譯失敗而已

---

## 條件迴圈 -- `while`

```rust
fn main() {
    let mut x = 0;
    while x < 100 {
        println!("{}", x);
        x += 1;
    }
}
```

是熟悉的 `while` 迴圈呢，基本上除了和 `if` 一樣，條件表達式的外圍不需要加上小括號之外，用起來跟大部分的語言都一樣，就不多贅述了。

---

## 疊代迴圈 -- `for`

```rust
fn main() {
    let scores = vec![55, 30, 87, 94, 68];
    let mut max_score = 0;
    for s in scores {
        println!("{}", s);
        if s > max_score {
           max_score = s;
        }
    }
    println!("Your maximum score: {}", max_score);
}
```

疊代迴圈，顧名思義就是會將疊代器內的所有元素都跑過一次，所以只能在可以轉換為疊代器的型別上使用（像是 `Vec` , `Array` …）。

`for` 的後面是接疊代的元素， `in` 後面接的是可以疊代的容器

 有人會問說要怎樣用 `for` 跑一個數字範圍，這時候就需要一個特殊的疊代器 `[Range](https://doc.rust-lang.org/std/ops/struct.Range.html)` ，用法如下

```rust
fn main() {
    for x in 0..100 {
        println!("{}", x);
    }
}
```

`0..100` 會創造一個 `[0, 100)` 的區間，它具有疊代器的特性，就可以給 `for` 用了，其他的一些用法：

- `0..=100` : `[0, 100]` 的閉區間
- `(0..100).rev()` : 倒著數回來 `99, 98, 97, ...`
- `(0..100).step_by(2)` : 間隔2個數 `0, 2, 4, 6, 8, ...`

值得思考的小問題：

- `(0..100).rev().step_by(2)` : ??
- `(0..100).step_by(2).rev()` : ??

※註： `.rev()` 是由 [`Iterator::rev()`](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.rev) 提供的， `.step_by()` 則是 `[Iterator::step_by()](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.step_by)`

---

## 迴圈標籤

Rust 支援迴圈標籤，可以直接 `break` 或 `continue` 到外層的迴圈

```rust
fn main() {
    'outer: for x in 0..10 {
        'inner: for y in 0..10 {
            if (x + y) > 10 {
                continue 'outer;
            }
            println!("({}, {})", x, y);
        }
    }
}
```

`break` 的用法亦同，就不解釋了

---

上一篇：[#08 -- if 條件敘述](@/posts/2019-11-14-rust-tutorial-8.md)
上一篇：[#08 -- if 條件敘述](@/posts/2019-11-14-rust-tutorial-8.md)

下一篇：[#10 -- 結構](@/posts/2020-02-01-rust-tutorial-10.md)
下一篇：[#10 -- 結構](@/posts/2020-02-01-rust-tutorial-10.md)

[Rust 導引筆記系列目錄](@/pages/2019-09-07-rust-index.md)
[Rust 導引筆記系列目錄](@/pages/2019-09-07-rust-index.md)
