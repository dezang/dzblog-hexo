---
title: Git
categories:
tags:
---

## 1. INTRODUCE
깃은 레포를 복사하여 로컬에 가지고 있다. DVCS 리누스 토발즈가 리눅스 커널 프로젝트를 수행하기 위해 만들어졌다. CLI와 많은 GUI있다.

`git help config`

먼저 유저 네임과 이메일을 설정해야 한다.

`git config --global user.name dezang`
`git init`
`git status`

`git add` 를 통해 스테이징 영역에 올릴 수 있다.

`git commit -m "Message..."`

`git add --all`

`git log`

## 2. STAGING & REMOTES
`git diff`
`git diff --staged`
`git reset HEAD <filename>`
`git -a -m "Message"`
`git reset --soft HEAD^`
`git commit --amend -m "Message"`
`git reset --hard HEAD^`

push pull

`git remote`

github bitbucket

`git remote add origin https://github.com/dezang/gs-git.git`
`git remote -v`
`git push -u origin master`

-u 옵션은 다음부터 브랜치 명을 적어주지 않아도 기본적으로 동작함.

`git pull`
`git remote rm <name>`

푸쉬하고 나서 푸쉬된 로그를 변경하는 행위는 위험하다.

## 3. CLONING & BRANCHING
## 4. COLLABORATION BASICS
 
## 관례
- Add
<!-- more -->

----
#### 참고
