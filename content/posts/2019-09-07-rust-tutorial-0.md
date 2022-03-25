+++
title = "Rust 導引筆記系列 #00 -- 安裝 Rust"
description = "安裝編譯器大概是最重要的第一件事了，在這篇文章中，會說明如何在 Windows 及 Linux 中安裝 Rust 編譯器在這邊會用官方提供的安裝程式 Rustup 來安裝"
date = 2019-09-07
updated = 2019-11-13

[taxonomies]
categories = ["coding"]
tags = ["coding", "rust", "rust-tutorial"]
+++

安裝編譯器大概是最重要的第一件事了，在這篇文章中，會說明如何在 Windows 及 Linux 中安裝 Rust 編譯器

在這邊會用官方提供的安裝程式 Rustup 來安裝，詳情可以參照 [rustup.rs](https://rustup.rs/)

---

# Linux & WSL & Mac

如果你用的發行版有提供 rustup 套件包（像是：Arch Linux），可以直接從套件庫裝。不過目前好像大部份都還沒做...所以主要是用以下的方式進行安裝

```bash
curl https://sh.rustup.rs -sSf | sh
```

然後會出現

```
info: downloading installer
 
Welcome to Rust!
 
This will download and install the official compiler for the Rust programming
language, and its package manager, Cargo.
 
It will add the cargo, rustc, rustup and other commands to Cargo's bin
directory, located at:
 
  /home/leo/.cargo/bin
 
This path will then be added to your PATH environment variable by modifying the
profile files located at:
 
  /home/leo/.profile
  /home/leo/.bash_profile
 
You can uninstall at any time with rustup self uninstall and these changes will
be reverted.
 
Current installation options:
 
   default host triple: x86_64-unknown-linux-gnu
     default toolchain: stable
  modify PATH variable: yes
 
1) Proceed with installation (default)
2) Customize installation
3) Cancel installation
```

基本上輸入 1，用預設的設定就好了，接著就會自動安裝完了 XD

如果用套件管理器安裝 rustup，則只需要在安裝完之後於終端機執行

    rustup install stable

沒錯，就是這麼簡單...

---

# Windows

到 [rustup.rs](https://rustup.rs/) 網站之後，它會提示你下載一個叫 rustup-init.exe 的程式，下載後執行就會出現和上面雷同的畫面

```
Welcome to Rust!

This will download and install the official compiler for the Rust programming
language, and its package manager, Cargo.

It will add the cargo, rustc, rustup and other commands to Cargo's bin
directory, located at:

  C:\Users\Leo\.cargo\bin

This path will then be added to your PATH environment variable by modifying the
HKEY_CURRENT_USER/Environment/PATH registry key.

You can uninstall at any time with rustup self uninstall and these changes will
be reverted.

Current installation options:

   default host triple: x86_64-pc-windows-msvc
     default toolchain: stable
  modify PATH variable: yes

1) Proceed with installation (default)
2) Customize installation
3) Cancel installation
```

不過在 Windows 下有兩種 ABI，簡單來說就是有兩種體系，gnu 和 msvc，這邊還是建議使用預設值，也就是 msvc。這牽扯的部份比較深，就不贅述了，有興趣的人可以自行研究。

然後等它跑完就裝好了，感覺完全不費功夫 XD

---

# 測試

打開你的終端機（Windows 不管是 powershell 或 cmd 都一樣啦~），執行

```
rustc --version
# rustc 1.37.0 (eae3437df 2019-08-13)
```

若是成功顯示出你所安裝的版本就是成功啦~~

如果提示找不到指令的話，則可以重開終端機試試；不然就是 PATH 環境變數沒設好，理當要有指向這個資料夾

```
# Linux
/home/<username>/.cargo/bin

# Windows
C:\Users\<username>\.cargo\bin
```

---

Rust 的安裝相當的簡單 (應該吧 ，稍微對終端機與指令有些認識應該都很好上手。相信各位都能安裝成功~~

上一篇：[前言](@/posts/2019-09-05-rust-pre-0.md)

下一篇：[#01 -- Hello world](@/posts/2019-09-12-rust-tutorial-1.md)

[Rust 導引筆記系列目錄](@/pages/2019-09-07-rust-index.md)
[Rust 導引筆記系列目錄](@/pages/2019-09-07-rust-index.md)
