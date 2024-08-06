---
author: Kshitijaa Jaglan
title: Switch between accents and repeated letters on mac
date: 2024-08-06
description: Choose what behaviour you want when you long press a key on Mac
tags: ["mac"]
draft: False
---

Mac offers the functionality to type [accented](https://support.apple.com/guide/mac-help/enter-characters-with-accent-marks-on-mac-mh27474/mac) (is that a real word?) letters directly through keyboard by showing an accent menu like the following if you long press a key.

![accents in the wild](../../images/accents.png)

But what about people like meeeeeeeee who likkkeee to write like thisss!!? (and are not regular users of a language that requires accents) \
Our specie can simply toggle this behaviour by using the following terminal command:

``` bash
defaults write NSGlobalDomain ApplePressAndHoldEnabled -bool false
```

However, if you ever change your mind and want the accent functionality back, you can do it using:

``` bash
defaults write NSGlobalDomain ApplePressAndHoldEnabled -bool true
```

> P.S. You might have to restart all your applications where you want the change to be reflected

Source of info: [iDB](https://www.idownloadblog.com/2015/01/14/how-to-enable-key-repeats-on-your-mac/)
