---
title: Fixing common mistakes
tags:
  - git
thumbnail: ../../../../images/git-logo.png
categories:
  - Git
date: 2018-01-04 16:25:06
---


![](../../../../images/git-logo.png)

이번 포스팅은 git을 하면서 사소한 실수를 했을때 대처하는 방법에 대해서 정리해보겠습니다.
우선 git 연습을 하기 위한 빈 디렉토리와 파일을 만들고 커밋을 진행해주세요.
혹시 git을 처음 사용하시는 분들은 지난번 [포스팅](https://jason0853.github.io/2017/12/16/Git-basic/)을 참고해주시기 바랍니다.

### # Restore code

![](../../../../images/git/git-fixing-common-mistakes-01.png)

``` html index.html
<html>
  <head></head>
  <body>
    지워야 될 텍스트
    지워야 될 텍스트
    지워야 될 텍스트
    지워야 될 텍스트
    지워야 될 텍스트
    지워야 될 텍스트
    지워야 될 텍스트
    ...
  </body>
</html>
```

``` shell
$ git diff
$ git checkout 'filename'
```

* <code>git diff</code> 명령어는 파일의 변경된 코드를 확인할 수 있습니다. 터미널 캡처 오른쪽을 확인해보면 새로 추가된 코드는 녹색으로 표시되어 나옵니다.
* <code>git checkout index.html</code>을 실행시키면 변경되기 전의 코드로 돌아갑니다.

### # Change a commit message

![](../../../../images/git/git-fixing-common-mistakes-02.png)

![](../../../../images/git/git-fixing-common-mistakes-03.png)

``` shell
$ git commit --amend -m 'message'
```

* 커밋 메세지에 오타를 수정하고 싶거나 변경하고 싶은 경우 다른 커밋 작업을 진행하기 직전에 바로 <code>amend</code>옵션과 함께 명령어를 실행시키면 메세지가 수정됩니다.

### # Reset

``` sequence
local repo-->staging area: git reset --soft
staging area-->working directory: git reset --mixed
Note right of working directory: git reset --hard\n git clean -df
```

![](../../../../images/git/git-fixing-common-mistakes-04.png)

![](../../../../images/git/git-fixing-common-mistakes-05.png)

``` shell
$ git reset --soft 'hash'
$ git reset --mixed 'hash'
$ git reset --hard 'hash'
$ git clean -df
```

* 커밋로그 보시면 unique 값들이 나열되어 있는데 이것을 **hash(해쉬)**라고 부릅니다. <code>git reset</code> 명령어를 사용할 때 hash[약7~8자 정도 copy and paste]를 통하여 원하는 커밋 로그 상태로 돌아갈 수 있습니다.
* <code>soft</code> 옵션을 통하여 staging area로 돌아갑니다.
* <code>mixed</code> 옵션은 working directory로 돌아갑니다.
***주의해서 봐야할 점은 index.html 파일은 이미 이전에 커밋을 했던 파일이며 script.js 파일은 새로 만들어져서 untracked files로 분류되어 있습니다.***
* <code>hard</code> 옵션은 변경된 코드는 제거되고 이전 커밋에 저장해놓았던 코드 상태로 되돌아갑니다.
***대신 untracked files로 분류된 파일들은 영향을 받지 않습니다.***
* <code>git clean -df</code> - undrecked file들을 삭제해줍니다.

``` shell
$ git reset --hard HEAD~1
```

* 최신 커밋 로그 메세지를 지우고 싶은 경우 <code>HEAD~1</code>을 추가하면 됩니다.
ex)예를 들어 최신 커밋 로그 2개를 지우고 싶은 경우는 <code>HEAD~2</code>을 추가하면 됩니다.
* 만약 원격저장소에 저장되어 있는 커밋 로그까지 삭제하고 싶다면 <code>git push origin HEAD --force</code> 명령어를 실행하시면 됩니다.

### # Revert

![](../../../../images/git/git-fixing-common-mistakes-06.png)

![](../../../../images/git/git-fixing-common-mistakes-07.png)

``` shell
$ git revert 'hash'...'hash'
```

* 이미 <code>push</code>까지 작업한 커밋을 되돌리고 싶을때는 <code>revert</code>를 사용합니다.
***<code>revert</code>작업할 때 최신 커밋 로그부터 차례대로 작업하는 것을 권고드립니다. conflict를 최소화하기 위해서입니다.***
* 하나의 커밋만 <code>revert</code>하고 싶은 경우는 <code>git revert 'hash'</code>를 하시면 됩니다.
* 여러 커밋을 하고 싶은 경우는 <code>'hash'...'hash'</code> 되돌아가고 싶은 커밋 시점부터 마지막 커밋 시점까지를 뜻합니다.

### Reference
<https://stackoverflow.com/questions/1338728/delete-commits-from-a-branch-in-git>
