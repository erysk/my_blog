---
title: nodenvをインストールする
date: 2019-10-6
tags: ["node"]
excerpt: 備忘録
---

nodeをrubyのrbenvと同じ感覚でバージョン管理できるというnodenvをインストールする。

```bash
brew install nodenv
```
バスを通す。
```bash
vi ~/.bash_profile
```
```.bash_profile
export PATH="$HOME/.nodenv/bin:$PATH"
eval "$(nodenv init -)"
```
ターミナルを再起動すればバージョンが表示される。
```bash
nodenv --version
```
あとはrbenvと同じ感覚でバージョン管理ができる！
```bash
nodenv install --list
nodenv install 12.10.0
nodenv global 12.10.0
nodenv local 12.10.0
nodenv versions
```
