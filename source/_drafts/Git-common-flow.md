---
title: Git common flow
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
* 만약 global option을 삭제하고 싶은 경우는 <code>unset</code> 옵션을 추가하여 해당 property를 지우시면 됩니다.
ex)<code>git config --global --unset user.name</code>


### # Initialize a repository

``` bash
$ git init
```

* 프로젝트 root 디렉토리에서 <code>init</code> 명령어를 통하여 초기화 시킵니다.

### # Create a gitignore file

{% gist 9f4a098f09452ac869722e1192f679bf %}

* root 디렉토리에 <code>.gitignore</code> 파일을 만듭니다.
* <code>.gitignore</code> 파일은 git을 통해 소스관리를 할 필요 없는 파일이나 폴더명을 써주시면 됩니다.

``` flow
op1=>operation: Working Directory
op2=>operation: Staging Area
op3=>operation: Local repo
op4=>operation: Remote repo

op1(right)->op2
op2(right)->op3
op3(right)->op4
```


{% codeblock lang:bash %}
$ git status
{% endcodeblock %}
