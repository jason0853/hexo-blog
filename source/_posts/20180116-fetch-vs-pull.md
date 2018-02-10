---
title: fetch vs pull
tags:
  - git
thumbnail: ../../../../images/git/git-logo.png
categories:
  - Tool
  - Git
date: 2018-01-16 13:27:40
---



![](../../../../images/git/git-logo.png)

회사에서 Git 작업을 하면서 원격저장소로부터 소스를 동기화시키기 위해서 항상 **pull**을 사용했었는데 **fetch**라는 기능도 있다는 것을 팀원을 통해 알게 되었습니다. 그럼 이 둘의 미묘한 차이를 한 번 다뤄보겠습니다.

``` sequence
working directory->staging area: git add
staging area->local repo: git commit
local repo->remote repo: git push
remote repo-->local repo: git fetch
remote repo-->working directory: git pull
```

![](../../../../images/git/git-fetch-vs-pull-01.png)

![](../../../../images/git/git-fetch-vs-pull-02.png)

``` shell
$ git fetch
$ git pull
```

* <code>git fetch</code> - 원격저장소로부터 소스를 로컬저장소로 가져오지만 merge 작업을 하지 않습니다.
* <code>git pull</code> - 원격저장소로부터 소스를 로컬저장소로 가져와서 working directory에 있는 소스와 함께 merge 작업을 진행합니다.

``` shell
$ git merge 'remote branch`
```
* <code>git fetch</code>를 한 이후 소스를 working directory와 병합하고 싶으면 merge 작업을 진행해줍니다.
ex) <code>git merge origin/master</code>

### Wrap-up

결국 <code>git pull</code> = <code>git fetch</code> + <code>git merge</code>를 합한 결과라고 보면 될 것 같습니다.

### Reference

[Git documentation - 리모트 브랜치](https://git-scm.com/book/ko/v1/Git-%EB%B8%8C%EB%9E%9C%EC%B9%98-%EB%A6%AC%EB%AA%A8%ED%8A%B8-%EB%B8%8C%EB%9E%9C%EC%B9%98)
