+++
title = "Visual Studio Code Memory Leaks 偵錯紀錄"
description = "之前在用Visual Studio Code寫Rust專案時，會經常性的發生記憶體被用光的情況，稍微看了一下，發現vscode的watcherService會隨著使用而開始消耗愈來越多的記憶體（然後就當機了...）。因為這問題困擾我有一陣子了，所以就決定來Debug了..."
date = 2019-07-12

[taxonomies]
categories = ["coding"]
tags = ["coding", "debug", "vscode"]
+++

## 緣起

之前在用Visual Studio Code寫Rust專案時，會經常性的發生記憶體被用光的情況，稍微看了一下，發現vscode的watcherService會隨著使用而開始消耗愈來越多的記憶體（然後就當機了...）。因為這問題困擾我有一陣子了，所以就決定來Debug了。

## 過程
### Visual Studio Code 的檔案監測器

現在的編輯器都會配備檔案監測器，當檔案在外部被改變時，編輯器可以做出適當的動作（例：提示使用者是否重新載入）。在檢視過一些原始碼及紀錄後，發現在單一個工作區開啟多個資料夾時，vscode 會使用nsfw這個檔案監控器，而它有很大的嫌疑...

nsfw為axosoft所開發的高效能檔案監控器，以node-gyp寫成，所以基本上是C++專案。為了有效率的除錯，就需要一些工具來輔助啦！

### Valgrind

[valgrind](http://www.valgrind.org/)是一款在Linux下分析記憶體、程式執行效能的工具。其中的memcheck在開發C/C++專案時，可以有效率的找出有關記憶體的問題。詳細用法就請自行上網找吧！

首先，先準備一個單純的專案，只使用了nsfw套件監控某個Rust專案，再用Rust Language Server (rls) 觸發大量的檔案更新，然後以valgrind監測，結果bug就浮現了...

```cpp
class InotifyService {
public:
  InotifyService(std::shared_ptr<EventQueue> queue, std::string path);

  std::string getError();
  bool hasErrored();
  bool isWatching();

  ~InotifyService();
private:
  void create(int wd, std::string name);
  void createDirectory(int wd, std::string name);
  void createDirectoryTree(std::string directoryTreePath);
  void dispatch(EventType action, int wd, std::string name);
  void dispatchRename(int fromWd, std::string fromName, int toWd, std::string toName);
  void modify(int wd, std::string name);
  void remove(int wd, std::string name);
  void removeDirectory(int wd);
  void rename(int fromWd, std::string fromName, int toWd, std::string toName);
  void renameDirectory(int fromWd, std::string fromName, int toWd, std::string toName);

  InotifyEventLoop *mEventLoop;
  std::shared_ptr<EventQueue> mQueue;
  InotifyTree *mTree;
  int mInotifyInstance;

  friend class InotifyEventLoop;
};

void InotifyEventLoop::work() {

    //...
    
    auto create = [&event, &isDirectoryEvent, &inotifyService]() {
        if (event == NULL) {
          return;
        }

        if (isDirectoryEvent) {
          inotifyService->createDirectory(event->wd, strdup(event->name));
        } else {
          inotifyService->create(event->wd, strdup(event->name));
        }
    };
    
    //...
}
```

（以上為部份程式碼節錄）

相信眼尖的人應該找出問題所在了吧。問題就出在那個多餘的 `strdup()`，由於 `std::string`會自行分配記憶體，所以多複製一份就會造成記憶體洩漏。

Pull Request: [https://github.com/Axosoft/nsfw/pull/74](https://github.com/Axosoft/nsfw/pull/74)

有時候bug就在我們身邊，或許它可以很簡單地被解決，但是能否找出來才是最大的關鍵（為了找出bug就花了我4小時...），所以在寫 C/C++ 時，記憶體管理的觀念以及檢查是極度重要的！（但超麻煩的...）也因為這樣，讓我跳坑到[Rust](https://www.rust-lang.org/)去了（Rust大好！！！

## 傳教時間～～～

Rust 擁有與 C++ 相同的執行效能，但它有編譯時的安全檢查、優秀的語法、先進的套件及專案管理器、官方開發的 Formatter 及 Language Server。2016～2018蟬聯[開發人員最喜歡的程式語言](https://insights.stackoverflow.com/survey/2018#most-loved-dreaded-and-wanted)，好語言不學嗎？

Rust繁中板官網：[https://www.rust-lang.org/zh-TW/](https://www.rust-lang.org/zh-TW/)
