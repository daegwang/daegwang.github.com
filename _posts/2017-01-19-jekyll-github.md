---
layout: post
title: "Build A Blog using Jekyll And Github Pages"
description: "Github Repository에 Jekyll을 사용하여 블로그를 만드는 방법"
date: 2017-01-19
tags: [jekyll, github]
comments: true
share: true
---

개발 관련 포스트를 작성하기 위한 블로그를 하나 만들기로 하고 방법들을 알아보던 중 jekyll + github pages를 사용하여 블로그를 만들 수 있다는 것을 알게 되어 블로그 생성 및 내용 정리를 위해 이 포스트를 작성하게 되었습니다.


시작하기에 앞서 루비 버전을 확인하여 업데이트가 필요할 경우에 업데이트를 해주어야 합니다.

``` bash
$-ruby v
$sudo gem update —system
```


## 1. jekyll 설치하기

다음과 같이 jekyll 설치 후 로컬에 github 폴더를 생성합니다.

``` bash
$sudo gem install jekyll
$jekyll new [깃헙 아이디].github.com 
```


폴더가 생성된 후 로컬에서 확인을 하기 위해서는 다음과 같이 해 주면 됩니다.

``` bash
$jekyll serve —watch
```


__(추가)__ bundler 관련 오류 발생 시에는 아래와 같이 bundler설치가 필요합니다.

``` bash
$brew install ruby
$gem install bundler
$bundle install
$bundle exec jekyll serve
```

이제 http://localhost:4000/ 에서 생성된 페이지를 확인할 수 있습니다.

## 2. github에 블로그 올리기

먼저 작업중이던 폴더에서 init을 통해 git 저장소를 생성합니다.

``` bash
$git init
```

이후에 로컬 저장소와 github을 연동하는 과정이 필요한데요. 
로컬의 git 내용이 github에 올라가게 하기 위해서는 ‘git add’명령어가 필요합니다.

``` bash
$git remote add origin https://github.com/daegwang/daegwang.github.com.git
$git add .
$git commit -m “update”
```

로컬 저장소에 commit이 완료되면 이후 해당 저장소를 push하면 반영이 되는데요. 블로그는 별도의 branch가 없으므로 master에 push를 하여도 됩니다.

``` bash
$git push origin master
```

## 3. 추가 사항
이제까지 jekyll을 사용하여 github에 블로그가 생성하는 방법을 알아보았습니다. 기능을 추가하고 싶으면 google analytics를 활용하여 방문자에 대한 정보를 얻을 수 있고, disqus를 사용하여 댓글 기능 추가도 가능합니다 :) 

## 4. 참고 사이트
- [https://nolboo.kim/blog/2013/10/15/free-blog-with-github-jekyll/][link1]
- [https://xho95.github.io/blog/github/jekyll/git/2016/01/11/Make-a-blog-with-Jekyll.html][link2]

[link1]: https://nolboo.kim/blog/2013/10/15/free-blog-with-github-jekyll/
[link2]: https://xho95.github.io/blog/github/jekyll/git/2016/01/11/Make-a-blog-with-Jekyll.html

