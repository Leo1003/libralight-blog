+++
title = "架部落格的大小事"
description = "前面有提到要寫寫架這個站的過程..."
date = "2019-05-21"

[taxonomies]
categories = ["blog"]
tags = ["blog", "ghost"]
+++

# 架部落格的大小事
前面有提到要寫寫架這個站的過程。不過先來談談為什麼要自己架一個？

## 自己架站的好處
- 不用錢，也不會長出廣告
- 資料是自己的
- 可以改一些東西
- 自己練習一些設定以及觀摩別人的網站架構
- ~~資工系學生應該要會~~

## 架設部落格
因為之前知道了交大學生有免費的DNS可以用，再加上小宇那邊的伺服器沒有在用，覺得很浪費。剛好想寫點東西，等等眾多原因就乾脆生一個出來。看來看去，能符合我的要求的框架貌似只有Ghost了。（或是有什麼我不知道的歡迎跟我說）而且官方都提供安裝腳本及說明了，就開來玩玩吧！

正當要連上伺服器時......咦，沒開...密一下小宇好了

小宇：「開不了機，貌似SSD爆了...」

我：「那SSD不是還沒用一年嗎？」

小宇：「保5年 穩，那等我考完拿去修」

我：「...」

所以後來就把還有一些餘額的Google VM拿出來用了。基本上，按照官方的教學，架設起來根本沒有難度。只要會用Unix，就會半自動化的生出來了。

```bash
# 基本上是靠這程式自動化安裝
$ sudo npm install ghost-cli@latest -g
# 開個資料庫
$ sudo mysql
CREATE DATABASE ghost;
CREATE USER 'ghost'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON ghost.* TO 'ghost'@'localhost';
quit
$ mkdir ghost
$ cd ghost
$ ghost install
```

之後會問一些問題：

- blog URL：你註冊到的網址，如果有要用https的話，前面也要改成https
- MySQL hostname：應該是localhost吧，除非你有遠端的SQL Server
- MySQL username / password：剛剛你自己設的
- Ghost database name：資料庫的名稱，如果不是用root登入SQL或是用現有的就要輸入正確的名稱
- Set up a ghost MySQL user?：如果是用root的話，他會幫你開資料庫和一個user
- Set up NGINX?：自動nginx設定，這超級方便的，以前在資訊社的Server上架網站都要自己寫...。如果前面的URL亂打，這邊弄出來也會是錯的...
- Set up SSL?：自動的Let's Encrypt...以前INFOR架網站也是都自己來，現在竟然都有自動化了...
- Set up systemd?：Systemd Service設定檔，這跟上面兩項一樣，都是在資訊社當網管時一天到晚都在做的事，所以這些全都被腳本取代了...
- Start Ghost?：幫你直接啟動

如果上面有任何一項要再調整，可以用這個指令調整

```bash
$ ghost setup
```

簡簡單單就架好了，本來以為會很麻煩，但不得不說，Ghost對於Self-Hosted的部份蠻友善的，讓新手或是對系統管理不是很熟的人也能有自己的部落格可以用。雖然上面提到Self-Hosted有很多好處，但是備份這部份就要自行處理了，所以就有研究一下備份方式。不過目前還沒找到很滿意的方式，可能等之後再來研究吧！

