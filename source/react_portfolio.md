---
title: Reactを使ったポートフォリオページをGitHub Pagesにデプロイする
date: 2019-10-6
tags: ["node", "React"]
excerpt: 備忘録
---


# nodeのバージョン管理ツールnodenvをインストールする。
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
# create-react-appコマンドをインストールする
```bash
npm install -g create-react-app
```
