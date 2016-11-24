---
title: 지킬을 사용한 포트폴리오 사이트 만들기
categories:
- tip
tags:
- jekyll
---

정적 웹 사이트를 만들어주는 [지킬](https://jekyllrb-ko.github.io/)을 사용해서 개인 포트폴리오나 회사 랜딩 페이지를 만드는 방법을 알아보자.

## 지킬 설치
지킬 사용법 자체는 그렇게 어렵지 않으나, 윈도우의 경우 지킬을 설치하기 위해 필요한 절차가 꽤나 복잡하다. 맥에는 루비, 파이썬등이 기본적으로 설치되어 있으니 맥 사용자라면 아래 명령어만으로 실행이 지킬을 사용할 수 있다.

```sh
~ $ gem install jekyll
~ $ jekyll new my-awesome-site
~ $ cd my-awesome-site
~/my-awesome-site $ jekyll serve
# => 이제 브라우저로 http://localhost:4000 에 접속
```

*공식 홈페이지는 저렇게 되어있지만 루비를 처음 사용하는 나는 `gem install bundler` 명령어를 입력해야했다.*

윈도우 사용자라면, [잘 설명된 글](http://tech.whatap.io/2015/09/11/install-jekyll-on-windows/)을 참조하여 설치하도록 하자. 설치가 끝났다면 이제 지킬을 사용할 준비가 된 것이다.

## 포트폴리오 테마
구글에 `jekyll theme` 라고 검색하면 수 많은 공개된 테마들을 볼 수 있다. 그 중 대장이 찾은 테마는 **agency-jekyll-theme** 이다. 미려한 하나의 페이지를 만들 수 있도록 도와준다. [깃허브](https://github.com/y7kim/agency-jekyll-theme), [데모](https://y7kim.github.io/agency-jekyll-theme/)
