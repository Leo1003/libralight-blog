+++
title = "在Ghost中加入程式碼語法標示 (1/2)"
date = 2019-05-25

[taxonomies]
categories = ["blog"]
tags = ["blog", "ghost"]
+++

# 在Ghost中加入程式碼語法標示 (1/2)
Ghost的預設主題為Casper，是一個外觀優秀、功能普遍齊全的主題。但是對身為程式設計師的使用者來說，經常會需要插入程式碼。雖然Ghost的文章是以Markdown撰寫的，可以快速插入一個程式碼區塊，但在這個區塊中，卻沒有語法高亮標示...

現在在網路上有許多許多提供這類功能的Javascript Library，像是
- [Prism.js](https://prismjs.com)
- [highlight.js](https://highlightjs.org/)
- [Rainbow](http://rainbowco.de)
- ...

附註一下，Markdown的程式碼區塊與HTML的 `<pre><code></code></pre>` 區塊是相同的，所以網路上大多數的語法高亮都是可以用的

## 插入程式碼語法高亮

在Ghost的頁面中加入資料有兩種方式：主控台的 Code injection、修改佈景主題

### Code injection
最簡便的方式，只要把CSS放在Site Header、Javascript放在Site Footer，就收工了。但是缺點是不方便做一些微調，甚至某些語法高亮會切割成一種語言一個檔案，管理起來就非常麻煩了...

Site Header
```html
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/prism/1.16.0/themes/prism.min.css" />
```

Site Footer
```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/prism/1.16.0/prism.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/prism/1.16.0/components/prism-c.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/prism/1.16.0/components/prism-cpp.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/prism/1.16.0/components/prism-json.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/prism/1.16.0/components/prism-javascript.min.js"></script><script src="https://cdnjs.cloudflare.com/ajax/libs/prism/1.16.0/components/prism-markup.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/prism/1.16.0/components/prism-css.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/prism/1.16.0/components/prism-rust.min.js"></script>
```

### 修改佈景主題
通常語法高亮的套件都可以自行選擇需要的功能，就可以下載到CSS和Javascript，再修改原本的Casper主題中的default.hbs檔案，最後上傳回Ghost就行了

詳細紀錄之後再打在下一篇好了...先附個已經改好的[Repo](https://github.com/Leo1003/Casper)

