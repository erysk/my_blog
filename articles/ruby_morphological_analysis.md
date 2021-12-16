---
title: Rubyで形態素解析
date: 2021-12-16
tags: ["Ruby"]
---

MeCab

```zsh
brew install mecab mecab-ipadic
ghq get git@github.com:neologd/mecab-ipadic-neologd.git
cd ~/.ghq/github/neologd/mecab-ipadic-neologd
bin/install-mecab-ipadic-neologd -n -a
vi /usr/local/etc/mecabrc
```

```diff
- dicdir =  /usr/local/lib/mecab/dic/ipadic
+ dicdir =  /usr/local/lib/mecab/dic/mecab-ipadic-neologd
```

```zsh
bundle init
```

```Gemfile
gem 'mecab'
```

```zsh
bundle install
```

```ruby
require 'macab'
tagger = MeCab::Tagger.new
puts tagger.parse('鬼滅の刃の映画を見た')
# 鬼滅の刃	名詞,固有名詞,一般,*,*,*,鬼滅の刃,キメツノヤイバ,キメツノヤイバ
# の	助詞,連体化,*,*,*,*,の,ノ,ノ
# 映画	名詞,一般,*,*,*,*,映画,エイガ,エイガ
# を	助詞,格助詞,一般,*,*,*,を,ヲ,ヲ
# 見	動詞,自立,*,*,一段,連用形,見る,ミ,ミ
# た	助動詞,*,*,*,特殊・タ,基本形,た,タ,タ
# EOS
```
