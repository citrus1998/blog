---
title: "VScodeでWSLからSSH接続"
subtitle: ""
date: 2022-07-08T02:43:47+09:00
lastmod: 2022-07-08T02:43:47+09:00
draft: false
author: ""
authorLink: ""
description: ""
license: ""
images: []

tags: ["WSL", "VScode", "技術メモ"]
categories: ["Technology"]

featuredImage: ""
featuredImagePreview: ""

hiddenFromHomePage: false
hiddenFromSearch: false
twemoji: false
lightgallery: true
ruby: true
fraction: true
fontawesome: true
linkToMarkdown: true
rssFullText: false

toc:
  enable: true
  auto: true
code:
  copy: true
  maxShownLines: 50
math:
  enable: false
  # ...
mapbox:
  # ...
share:
  enable: true
  # ...
comment:
  enable: true
  # ...
library:
  css:
    # someCSS = "some.css"
    # located in "assets/"
    # Or
    # someCSS = "https://cdn.example.com/some.css"
  js:
    # someJS = "some.js"
    # located in "assets/"
    # Or
    # someJS = "https://cdn.example.com/some.js"
seo:
  images: []
  # ...
---

<!--more-->

タイトル通りWSLからSSH接続を行ってVScodeを動かしたので，個人の忘備録として残していきたいと思います．その際トラブルが生じましたので，その解決方法と共に後述しています．
## VScodeのパスをWSLに変更  
以下のURLが参考となります．
https://qiita.com/_masa_u/items/d3c1fa7898b0783bc3ed

VScodeの`setting.json`を開き，`terminal.integrated.cwd`を追加しました．その後，Cドライブ上でのWSLのパスを`terminal.integrated.cwd`の後に書きました．
## VScodeからSSH接続  
ssh-keygenで秘密鍵id_rsaを設定し，WSL下の.sshに置きました．
その後，WSLからSSH接続を試みましたが，以下のようなエラーが出ました．
`no such identity: C:user\ユーザー名\.ssh\id_rsa: No such file or directory`

どうやらWindowsのCドライブの下に直接id_rsaを置かなければならないようです．
[紆余曲折](https://ja.stackoverflow.com/questions/83517/vscode%e3%81%aessh%e7%a7%98%e5%af%86%e9%8d%b5%e3%81%ae%e3%82%a2%e3%83%89%e3%83%ac%e3%82%b9%e8%a8%ad%e5%ae%9a%e3%81%ae%e5%a4%89%e6%9b%b4%e3%81%8c%e3%81%a7%e3%81%8d%e3%82%8b%e3%81%8b%e3%81%a9%e3%81%86%e3%81%8b)を得て，Cドライブ下にWSL下の秘密鍵のシンボリックリンクを置いたところ，VScodeでSSH接続に成功しました．

シンボリックリンクの作成法は以下のURLを参考にしました．
https://www.koikikukan.com/archives/2020/04/16-235555.php

## おわりに
もう少しできれば図を挟みながら説明したいと思っております．更新頑張ります．