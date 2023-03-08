---
title: jekyll blog 생성하기
category: jekyll
layout: post
---

***

1. jekyll 설치
2. jekyll 생성
3. GitHub Repository 연결

***

# 1. jekyll 설치

**windows**
```bash
# ruby
$ winget search ruby
$ winget install "Ruby 3.1"
$ ruby -v
ruby 3.1.2p20 (2022-04-12 revision 4491bb740a) [x64-mingw-ucrt]
$ gem -v
3.3.7

# jekyll
$ gem install jekyll bundler
$ jekyll -v
jekyll 4.3.1

# if MSYS2 error :
# 1. Install the installer from https://www.msys2.org/
# 2. start "Start Command prompt with Ruby"
# 3. $ ridk install
```

## 2. jekyll 생성

```bash
$ cd my-path/dev/projects/jekyllBlog
$ jekyll new jekyllBlog
$ cd jekyllBlog
$ bundelr exec jekyll serve
... localhost:4000/
```

## 3. GitHub Repository 연결