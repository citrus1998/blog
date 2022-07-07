---
title: "kaggle: command not found"
subtitle: ""
date: 2022-07-07T20:44:56+09:00
lastmod: 2022-07-07T20:44:56+09:00
draft: false
author: ""
authorLink: ""
description: ""
license: ""
images: []
resources:
- name: "kaggle"
  src: "kaggle.jpeg"

tags: ["kaggle", "技術メモ"]
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

## はじめに

csvファイルのデータセットをkaggleからダウンロードするのに，kaggleコマンドをubuntuにインストールしました．

その際，`kaggle: command not found` が生じてしまいましたので忘備録として残しておきます．

なお，少しでもお役に立てれば嬉しい限りです．

## 設定環境
WSL(Windows Subsystem for Linux)のubuntu18.04上にて行いました．
なお，[kaggle](https://www.kaggle.com/)にユーザー登録済みとします．

## 通常通りWSLにkaggleコマンドをインストール
まず普通にWSL上にkaggleコマンドをインストールします．

Github上の[Kaggle API](https://github.com/Kaggle/kaggle-api)にあるREAD.ME通りに進めました．

`kaggle: command not found` 発生時まで手順を追っていきます．

### 1. kaggleをpipでインストール
まず以下のこれらのコマンドを入力しました．

```
pip3 install --user kaggle
python3 -m pip install kaggle
```

その後 `kaggle --version` と入力しましたが，`kaggle: command not found` が発生しました．

失敗です．

### 2. kaggle.jsonを[kaggle](https://www.kaggle.com/)からダウンロードし，`~/.kaggle/kaggle.json`へ設置
右上のアイコンをクリックして，kaggleのマイページに行きます．

下の画像にある，API中の"Create New API Token"を押して，`kaggle.json`をダウンロードします．

![kaggle_top](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2310260%2F2540332a-bdec-65ce-9911-2a75ebb2d392.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&w=1400&fit=max&s=c9aae3f4809f81e2de6faff5daee56ea)

その後，WSLのホームディレクトリに`.kaggle`ファイルを作成し，その下にkaggle.jsonを移しました（`~/.kaggle/kaggle.json`となるようにしました）．

そして，以下のコマンドでパーミッションを所有者のみ読み書き可能にしました．

```
chmod 600 ~/.kaggle/kaggle.json
```


その後 `kaggle --version` と入力しましたが，`kaggle: command not found` が発生しました．

2度目の失敗です．

### 3. `~/.local/bin`にパズが通っているか確認
Github上の[Kaggle API](https://github.com/Kaggle/kaggle-api)にあるREAD.MEを見てみますと，以下の文言が書かれていることが分かります．

![kaggle_top](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2310260%2Fb4f20e86-3524-6716-797f-76fdae5722a0.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&w=1400&fit=max&s=547752b0216227da8c091db3eaae59c9)

"If you run into a `kaggle: command not found` ~ "の部分に注目．

要約すると，`pip uninstall kaggle`でkaggleのアンインストールの途中で，`~/.local/bin`にパズが通っているか確認してくださいということです．

なお，実際の画面はこのような感じでした（pipのバージョンについて言っておりますが，WARNINGは気にしないで下さい）．
```
WARNING: pip is being invoked by an old script wrapper. This will fail in a future version of pip.
Please see https://github.com/pypa/pip/issues/5599 for advice on fixing the underlying issue.
To avoid this problem you can invoke Python with '-m pip' instead of running pip directly.
Found existing installation: kaggle 1.5.12
Uninstalling kaggle-1.5.12:
  Would remove:
    /home/ユーザー名/.local/bin/kaggle
    /home/ユーザー名/.local/lib/python3.8/site-packages/kaggle-1.5.12.dist-info/*
    /home/ユーザー名/.local/lib/python3.8/site-packages/kaggle/*
Proceed (Y/n)? 
```

`Would remove:`の下に`/home/ユーザー名/.local/bin/kaggle`とあるので，一応パズは通っていることが分かります．

もし通っていなければ，以下のコマンドを入力してパズの追加と確認を行ってください．

```
export PATH=".local/bin/"
echo $PATH
```

そしてもう一度 1. からインストールをし直すことをお勧めします．

`~/.local/bin`にパズが通っているので，この状態で `kaggle --version` と入力します．


すると，


`kaggle: command not found` が発生しました．

3度目の失敗です．

### 4. シンボリックリンクの作成
[Kaggle API](https://github.com/Kaggle/kaggle-api)に書かれていることは一通りに行いましたが，解決できなかったので，必死に探しました．

そしてついに以下のサイトにたどり着きました．

https://stackoverflow.com/questions/48370064/pip-install-kaggle-works-fine-but-kg-command-not-found/49398939

該当箇所を引用します．

> I located the Kaggle executable in `~/.local/bin/kaggle` and solved it creating a symbolic link to `/usr/bin`
>>`ln -s ~/.local/bin/kaggle /usr/bin/kaggle`

`ln`でファイルのリンクを作成することができます．

特に今回は`ln -s`でシンボリックリンクが作成できます．

`ln -s pathA pathB`で，pathBにpathAのシンボリックリンクを作成するという意味になります．

上記の通り， `/usr/bin/kaggle`に`~/.local/bin/kaggle`のシンボリックリンクを作成しなければならないので，以下の通りにコマンドを入力しました．

```
sudo ln -s ~/.local/bin/kaggle /usr/bin/kaggle
```

その後 `kaggle --version` と入力しました．

すると，


```
$ kaggle --version
Kaggle API 1.5.12
```

と表示されたので，成功していることが分かります．

Congratulations:tada:

## おわりに
ご清聴ありがとうございました．

少しでもお役に立てれば嬉しい限りです．

