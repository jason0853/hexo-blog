---
title: Git basic
thumbnail: ../../../../images/git-logo.png
categories:
  - Git
tags: 
  - git
---

![](../../../../images/git-logo.png)

개발시 협업할 때 일반적인 Git의 흐름 상황을 Command cli로 요약 정리해보겠습니다.

## Quick Start

### # Check version

``` bash
$ git --version
```

* git 제대로 설치되어 있는지 확인하려면 version 체크를 하시면 됩니다.

### # Set config values

``` bash
$ git config --global user.name 'jason0853'
$ git config --global user.email 'jason0853@gmail.com'
$ git config --list
```

* 보통 <code>global</code> 옵션을 주어 git의 사용자와 이메일을 셋팅해 줄 수 있습니다.
* <code>list</code> 옵션을 통하여 셋팅된 목록을 볼 수 있습니다.
* 만약 global property를 삭제하고 싶은 경우는 <code>unset</code> 옵션을 추가하여 해당 property를 지우시면 됩니다.
ex)<code>git config --global --unset user.name</code>


### # Initialize a repository

``` bash
$ git init
```

* 프로젝트 root 디렉토리에서 <code>init</code> 명령어를 통하여 초기화 시킨 이후 .git이라는 파일이 생성됩니다.

### # Create a gitignore file

{% gist 9f4a098f09452ac869722e1192f679bf %}

* root 디렉토리에 <code>.gitignore</code> 파일을 만듭니다.
* <code>.gitignore</code> 파일은 git을 통해 소스관리를 할 필요 없는 파일이나 폴더명을 써주시면 됩니다.

### # Common workflow

``` sequence
working directory->staging area: git add
staging area->local repo: git commit
local repo->remote repo: git push
remote repo-->local repo: git pull
local repo-->working directory: git checkout
local repo-->working directory: git merge
```

git의 흐름을 단계별로 diagram으로 정리해보았습니다.
그럼 위 도표에서 git push까지 진행해보겠습니다.

``` bash
$ git status
$ git add -A
$ git commit -m 'initial commit'
$ git remote origin 'https://github.com/jason0853/git-test.git'
$ git push -u origin master
```

1. <code>git status</code> - 파일의 현재 상태를 알려줍니다. commit을 한번도 하지 않았기 때문에 새로 생성된 file은 Untracked files로 분류되어 있을겁니다. 
2. <code>git add -A</code> - <code>A</code> 옵션을 통하여 디렉토리에 있는 모든 파일을 staging area로 저장됩니다.
3. <code>git commit -m 'your message'</code> - commit message와 함께 local repository로 저장됩니다.
4. <code>git remote origin 'your remote url'</code> - 원격 저장소로 저장하기 전에 원격 저장소 url를 추가합니다. 원격저장소는 github 및 bitbucket 등 아무거나 사용해주셔도 됩니다.
5. <code>git push -u origin master</code> - <code>u</code> 옵션은 향후에 <code>origin master</code> 를 적지 않아도 원격 저장소로 저장됩니다.

보통 회사에서 일할 때는 master branch에서 작업하는 경우는 거의 없습니다. 다수의 개발자들과 서로 협업을 해야하므로 각각 branch를 생성해서 checkout 한 다음에 작업을 진행합니다. 작업이 끝난 이후에는 각 개발자들의 작업내역을 master branch로 merge 시킵니다. 그리고 배포를 진행합니다.
그럼 이어서 한번 진행해보겠습니다.

``` bash
$ git branch test
$ git branch
$ git checkout test
$ git add -A
$ git commit -m 'Add login method'
$ git push -u origin test
$ git branch -a
$ git checkout master
$ git pull
$ git branch --no-merged
$ git merge test
$ git push
$ git branch --merged
$ git branch -d test
$ git push origin --delete test
$ git log --all --decorate --graph --oneline --stat
```

명령어 순서대로 설명해보겠습니다.
1. <code>git branch test</code> - branch name을 생성합니다.
2. <code>git branch</code> - local branch list를 보여줍니다. 여기서 test 브랜치에 active 상태로 되어있는 확인하실 수 있습니다.
3. <code>git checkout test</code> - master(local) branch에 있는 source를 test로 복사합니다.
그 이후 작업해야할 코드를 작성합니다. 코드 작성이 끝난 이후 우리는 git 작업을 이어서 진행합니다.
4. working directory에서 staging area로 저장됩니다.
5. 'Add login method'라는 메세지와 함께 staging area에서 local repository로 저장됩니다.
6. local repository에서 test branch인 remote repository로 저장됩니다.
7. <code>git branch -a</code> - <code>a</code> 옵션을 통하여 remote branch list까지 보여줍니다. remotes/origin/test 브랜치가 생성되어 있는걸 확인하실 수 있습니다.
8. <code>git checkout master</code> - local master branch로 이동합니다. 이때 test 브랜치로 checkout 했을 때와 달리 test 브랜치에 있는 작업내역이 master 브랜치로 복사되지 않습니다.
***이것을 통해서 우리는 이미 만들어져 있던 branch로 checkout했을 경우는 이전 작업내역을 그대로 유지하고 있으며 새로 생성한 branch를 checkout 했을 경우는 현재 위치해 있는 branch 작업 내역을 그대로 복사한다는 것을 알게 되었습니다.***
9. <code>git pull</code> - remote repository에서 local repository으로 코드를 저장 및 병합을 시켜줍니다. 이전에 <code>-u</code>옵션을 주었기 때문에 <code>git pull origin master</code>로 다 써주지 않아도 됩니다. 
***항상 branch를 checkout한 경우에는 conflict를 최소화하기 원격 작업 내역을 병합한 다음 자신의 작업내역을 진행하는 습관을 들이는 것이 좋습니다. ***
10. <code>no-merged</code> 옵션을 통하여 merge 하지 않은 branch list를 확인합니다.
11. <code>git merge test</code> - test 브랜치를 master 브랜치로 merge 시킵니다. 이때 ***Fast-forward***라는 메세지가 나오면서 순식간에 병합이 일어납니다. 즉, merge 할 test 브랜치가 master 브랜치가 가리키는 것보다 앞으로 진행한 commit 이기 때문에 master 브랜치 포인터는 최신 커밋으로 이동하는 방식을 일컫는 말입니다.
12. merge가 잘 되었다면 remote repository로 저장시킵니다. <code>u</code>옵션을 처음에 주고 push했기 때문에 <code>git push origin master</code>를 다 쓰지 않아도 됩니다.
13. <code>merged</code> 옵션을 통하여 merge한 브랜치 list를 보여줍니다.
14. 로컬 branch(master, test) 코드가 동일해졌으니 <code>d</code> 옵션을 주어 필요없는 test branch를 삭제해줍니다.
15. 원격 branch(remotes/origin/master, remotes/origin/test) 코드도 동일해졌으니 원격 test branch도 삭제시켜줍니다. <code>delete</code> 옵션을 통해 push 합니다.
16. commit history를 확인합니다.
  * <code>all</code> - refs/의 모든 참조들의 log들을 출력시켜줍니다.
  * <code>decorate</code> - commit id 오른쪽에 현재 위치해있는 브랜치 name을 보여줍니다.
  * <code>graph</code> - commit id 왼쪽에 line을 생성하여 commit log를 좀 더 자세히 보여줍니다. 이 옵션을 통하여 각 브랜치 commit log들을 좀 더 시각화하여 branch들간의 차이를 파악할 수 있습니다.
  * <code>oneline</code> - 한줄로 요약해서 보여줍니다.
  * <code>stat</code> - 변경된 파일들의 목록을 보여줍니다.

지금까지 git의 기초와 일반적인 workflow에 대해 알아보았습니다. 실무에서는 이보다 훨씬 더 복잡한 구조로 되어 있기 때문에 기본 개념을 제대로 이해하셔야 conflict 났을 경우 대처하기가 쉽습니다. command line 이 익숙하지 않은 분들에게는 [SourceTree](https://www.sourcetreeapp.com/)라는 GUI 툴을 추천합니다.

### Reference
<https://git-scm.com/book/ko/v1/Git-%EB%B8%8C%EB%9E%9C%EC%B9%98-%EB%B8%8C%EB%9E%9C%EC%B9%98%EC%99%80-Merge%EC%9D%98-%EA%B8%B0%EC%B4%88>
