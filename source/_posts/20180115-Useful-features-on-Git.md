---
title: Useful features on Git
tags:
  - git
thumbnail: ../../../../images/git/git-logo.png
categories:
  - Git
date: 2018-01-15 15:56:46
---


![](../../../../images/git/git-logo.png)

이번 포스팅은 Git을 사용하면서 필자가 자주 사용하는 유용한 기능들에 대해서 살펴보겠습니다.

### # Stash

``` shell
$ git stash save 'stash message'
$ git stash apply stash@{index}
$ git stash pop
$ git stash drop stash@{index}
$ git stash clear
$ git stash list
```

* <code>git stash</code> 특징
  1. **unstaged files**들은 백업해두면서 *working directory*는 최신 커밋상태로 돌아갑니다.
  2. **untracked files**들은 백업되지 않습니다.
  3. 브랜치를 변경해도 stash는 공유됩니다.
* <code>pop</code>은 apply<code></code>와 달리 적용되면서 stash에서 제거됩니다.
* 주로 커밋작업할 준비가 아직 되어 있지 않을때 <code>stash</code>를 이용하여 백업해둡니다.

### # Cherry-pick

``` shell
$ git cherry-pick 'hash'
```

* 다른 브랜치의 커밋 내역을 다 복사해오지 않고 **특정 커밋**만 가져오고 싶은 경우 사용됩니다.

### # Tag

Git 태그는 **Lightweight tag**와 **Annotated tag**로 분류되어 있습니다.

![](../../../../images/git/git-useful-features-on-git-01.png)

![](../../../../images/git/git-useful-features-on-git-02.png)

![](../../../../images/git/git-useful-features-on-git-03.png)

``` shell
$ git tag 'tag name' 'hash or branch name'
$ git tag
$ git tag -d 'tag name'
```

* 태그 이외에 다른 정보를 저장할 필요가 없다면 위와 같이 아무 옵션 없이 명령어를 실행시키면 됩니다. 위와 같은 방법을 **Lightweight tag**라 부릅니다.
* 태그를 이용하여 <code>git checkout 'tag name'</code>을 이용하면 태그가 위치한 커밋 소스로 이동합니다. 

![](../../../../images/git/git-useful-features-on-git-04.png)

![](../../../../images/git/git-useful-features-on-git-05.png)

``` shell
$ git tag -a 'tag name' -m 'tag message'
$ git tag -v 'tag name'
```

* <code>a</code>옵션은 annotated의 약자로 **Annotated tag**를 지정하는 것입니다. 
* <code>git tag -v</code> - 위 그림의 빨강 박스에서 확인할 수 있듯이 태그를 생성한 사람의 username, email, date, tag message가 보여집니다.


![](../../../../images/git/git-useful-features-on-git-06.png)

![](../../../../images/git/git-useful-features-on-git-07.png)

``` shell
$ git push --tags
```

* tag를 원격저장소에도 반영하기 위해서는 <code>tags</code>옵션과 함께 푸쉬 작업을 해줍니다.
* 소프트웨어 버전을 작성할 때 어떤 기준으로 해서 작성을 할 것인가에 대해 고민하신다면 이 [사이트](https://semver.org/)를 참고바랍니다.

### Wrap-up

Git 작업을 하면서 **stash**, **cherry-pick**, **tag**를 상황에 맞게 적절히 이용하면서 사용하길 바랍니다.

### Reference
<https://git-scm.com/book/ko/v1/Git%EC%9D%98-%EA%B8%B0%EC%B4%88-%ED%83%9C%EA%B7%B8>
