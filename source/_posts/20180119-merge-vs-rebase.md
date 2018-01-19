---
title: merge vs rebase
tags:
  - git
thumbnail: ../../../../images/git/git-logo.png
categories:
  - Git
date: 2018-01-19 16:52:11
---



![](../../../../images/git/git-logo.png)

프로젝트 결과물도 중요하지만 작업 과정을 기록하는 일 또한 협업 업무에 있어서 중요하다고 생각합니다. 그래서 히스토리를 어떻게 정리할 것인가에 대해서 다뤄볼 예정인데요. 보통은 각 기능마다 브랜치를 생성해서 작업이 끝난 뒤 다른 브랜치로 병합하는 과정을 진행합니다. 이렇게 병합을 진행하는데 있어서 git에는 **merge**와 **rebase**가 있습니다. 이 두 차이를 알아보고 어떤 상황에서 써야되는지 알아봅시다.

![](../../../../images/git/git-merge-vs-rebase-01.png)


현재 위 상황처럼 *test*브랜치와 *master*브랜치가 엇갈리게 만들어놓고 병합을 한번 진행해보겠습니다.


### # Merge

![](../../../../images/git/git-merge-vs-rebase-02.png)

``` shell
$ git merge 'branch name'
```

* master 브랜치에서 <code>git merge test</code>한 커밋 로그입니다.
* 로그에서 보듯이 병렬로 작업된 구조가 그대로 보존되면서 *Merge branch 'test'*라는 새로운 커밋 로그가 맨 위에 있습니다. 작업한 과정을 고스란히 보여줍니다.
* 두 브랜치의 최종결과만을 가지고 합칩니다.

이제 다시 **merge** 전으로 돌린 이후 이전과 똑같은 상황에서 **rebase** 작업을 진행해보겠습니다.

### # Rebase

![](../../../../images/git/git-merge-vs-rebase-03.png)

``` shell
$ git rebase 'branch name'
```

* 왼쪽 터밀널은 test 브랜치에서 <code>git rebase master</code>를 한 커밋로그이며 오른쪽 터미널은 **rebase** 작업이 끝난 뒤 master 브랜치에서 <code>git merge test</code>를 한 커밋로그입니다.
* 로그를 살펴보면 **merge**한 로그와 달리 히스토리가 선형으로 보기 좋게 나옵니다. 
* **rebase**는 브랜치의 변경사항을 순서대로 다른 브랜치에 적용하면서 합칩니다.

지금까지는 각 브랜치에서 다른 파일을 작업했기 때문에 충돌이 나지 않고 auto-merging이 잘 되었습니다.
이번에는 충돌 상황을 고의로 만든 이후에 **rebase** 작업을 해나가며 충돌을 해결하는 작업을 진행해보겠습니다.

![](../../../../images/git/git-merge-vs-rebase-04.png)

파일 하나만을 가지고 브랜치를 바꿔가며 작업하여 로그를 보여준 모습입니다.

![](../../../../images/git/git-merge-vs-rebase-05.png)

test 브랜치에서 **rebase** 작업을 하였습니다.
하지만 *CONFLICT(content): Merge conflict in index.html*라는 메세지가 보입니다.
충돌을 해결해줍시다.

![](../../../../images/git/git-merge-vs-rebase-06.png)

충돌을 해결한 뒤 파일을 staging area로 옮깁니다. git 상태 체크를 다시 해봅니다. 
*all conflicts fixed: fun "git rebase --continue"*라는 메세지가 보입니다.
충돌이 더 이상 없으면 Auto-merging 작업이 충돌없이 잘 진행됩니다.

![](../../../../images/git/git-merge-vs-rebase-07.png)


``` shell
$ git add 'file name'
$ git rebase --continue
```

* *mark resolution*은 충돌을 해결했으면 git에게 알려주어야 다음 **rebase** 작업을 진행할 수 있습니다.
ex) <code>git add index.html</code>
* <code>continue</code> 옵션과 **rebase** 작업을 하면 기존 작업을 이어서 작업해줍니다.

### Wrap-up

개인적으로는 로컬 브랜치에서 병합을 진행할 때 히스토리를 깔끔하게 보기 위해서 **rebase**를 즐겨쓰는 편이기는 하지만 **merge** 작업 또한 히스토리를 변경하지 않고 로그가 남기 때문에 프로젝트 회고 시간을 가질 때 좋은 것 같습니다. 각각 장단점이 있기 때문에 어떤 것이 우월하다고 말하기 곤란하며 각자의 상황과 판단에 맞춰서 쓰시길 추천합니다. 

### Reference
<https://git-scm.com/book/ko/v2/Git-%EB%B8%8C%EB%9E%9C%EC%B9%98-Rebase-%ED%95%98%EA%B8%B0>