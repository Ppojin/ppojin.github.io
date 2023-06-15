---
title:  "start rbenv"
date:   2023-06-07 17:38:00 +0900
categories: 
  - infrastructure
tags: 
  - ruby
  - mac
  - rbenv
  - cheet_sheet
---

Install ruby on mac using rvenb

## 1. Install ruby on rbenv

```bash
# install rbenv
brew install rbenv ruby-build

# check install
rbenv -v

# show ruby versions on rbenv
rbenv install

# install ruby
rbenv install 3.1.4

# set installed ruby to global
rbenv global 3.1.4
```

## 2. Set profile
```bash
vi ~/.zshrc
```

```bash
[[ -d ~/.rbenv  ]] && \
  export PATH=${HOME}/.rbenv/bin:${PATH} && \
  eval "$(rbenv init -)"
```

```bash
source ~/.zshrc
```