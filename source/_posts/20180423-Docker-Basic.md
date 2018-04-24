---
title: Docker Basic
tags:
  - Docker
thumbnail: ../../../../images/docker/docker-logo.png
categories:
  - Ops
  - Docker
date: 2018-04-23 13:36:13
---


![](../../../../images/docker/docker-logo.png)


요즘 인프라에 대해 관심이 생기면서 자연스럽게 **Docker**에 대해 관심을 가지게 되었습니다. Docker에 대해 1도 몰랐던 개발자들에게 도움이 되고자 포스팅을 한 번 해보겠습니다. 이번 포스팅은 **Docker**의 기본 개념과 자주 쓰는 명령어에 대해 간락히 살펴보겠습니다.

### What is Docker?

**Docker**에 대해 검색을 해보니 Virtual Machine(VM Ware / Virtual Box)과 유사한 기능을 가진 것처럼 보입니다. 기존의 Virtual Machine 같은 경우 Host OS가 깔리고 그 위에 VM Ware나 Virtual Box를 설치한 이후에 다양한 종류의 OS(Linux나 Windows)를 설치합니다. 즉, 하드웨어를 가상화 한 것이라고 보면 됩니다. 상당히 무겁고 운영환경에서 쓰기에 이슈가 많습니다. 그와 반대로 **도커**는 Host OS가 깔리고 그 위에 Docker Engine이 실행되며 Container 위에서 Linux 기반의 OS만 수행이 가능합니다. 여기서 중요하게 봐야할 부분은 **컨터이너**라는 격리된 공간에서 프로세스가 동작하는 기술입니다. 에를 들어 여러 개의 컨테이너를 동시에 띄워도 독립적으로 실행되기 때문에 아주 가볍고 빠르게 동작합니다. 또한 컨테이너를 설치하는 시간은 기존의 가상머신(VM Ware나 Virtual Box) 위에 OS를 설치하는 것과 비교할 수 없을 정도로 빠릅니다.

### Install Docker

우선 Docker를 설치해보겠습니다. 이 [사이트](https://www.docker.com/community-edition#/download)에서 각자 자신의 운영체제에 맞게 설치하면 되겠습니다.

``` shell
$ docker --version
```

* 설치하신 이후 docker가 잘 깔렸는지 확인하기 위해 version 체크를 해보겠습니다.

### Docker Command Line

개인적으로 CLI를 선호하기 때문에 자주 쓰는 명령어를 요약해보겠습니다. CLI보다 GUI Tool을 선호하시는 분은 [kitematic](https://kitematic.com/)을 추천드립니다.

``` shell
$ docker --help
```

* docker와 관련된 command line 정보를 보여줍니다.

``` shell
$ docker pull 'image name'
$ docker images
```

* <code>pull</code> 명령어는 docker 이미지 내려 받기. Tag에 버전을 따로 적어주지 않으면 최신버전(latest)을 내려받게 됩니다.
ex) <code>docker pull ubuntu</code> / <code>docker pull ubuntu:14.04</code>
* <code>docker images</code>는 내려받는 docker 이미지 리스트들을 노출시켜줍니다.

``` shell
$ docker rm 'container id or name'
$ docker rmi 'image id'
```

* docker의 container와 image를 삭제시킵니다.

``` shell
$ docker run -it --name 'new name' 'image name:tag' /bin/bash
$ docker run -d --name 'new name' -p 'host port:container port' 'image name:tag'
```

* 새로운 컨테이너를 만들면서 /bin/bash를 실행시키는 명령어입니다.
* 옵션 <code>i</code>는 interactive의 약자로 사용자가 입출력을 할 수 있는 상태를 만들어줍니다.
* 옵션 <code>t</code>는 가상 터미널 환경을 emulate 해주는 기능입니다.
* <code>name</code> 옵션을 주지 않아도 random으로 name이 부여됩니다.
* /bin/bash 안으로 들어오고 나서 다시 container 밖으로 나오려면 <code>exit</code>를 입력해주시면 됩니다. 이때 container는 종료됩니다. 종료하지 않고 빠져나오고 싶은 경우는 <code>Ctrl + p + q</code> 단축키를 사용합니다.
* <code>d</code> 옵션 같은 경우는 ubuntu 같은 os 이미지 말고 nginx 같은 어플리케이션을 이미지로 내려받을 경우 입출력이 따로 필요없기 때문에 백그라운드 형태에서 실행시켜주는 역할을 합니다.
예) nginx pull로 받고 나서 실행 시킬 경우 : <code>docker run -d --name webserver -p 8000:80 nginx:latest</code> 명령어 실행시킨 후 브라우저에서 localhost:8000으로 접속하면 nginx 서버에 접속하는걸 확인하실 수 있습니다.

``` shell
$ docker ps
$ docker ps -a
```

* <code>docker ps</code>는 현재 실행되고 있는 container 리스트를 보여줍니다.
* <code>a</code>옵션은 모든 container 리스트(종료된 리스트 포함)까지 보여줍니다.

``` shell
$ docker start 'container id or name'
$ docker stop 'container id or name'
```

* docker container의 실행시켜주고 종료시키는 명령어입니다.

``` shell
$ docker attach 'container id or name'
$ docker exec -it 'container id or name' /bin/bash
```

* 실행되고 있는 container에서 /bin/bash 안으로 들어가서 입출력을 할 수 있게 해줍니다.
* <code>attach</code> 같은 경우는 명령어 이후 enter키를 한 번 쳐주셔야지 /bin/bash 안으로 접근할 수 있습니다.

``` shell
$ docker exec 'container id or name' echo 'hello docker'
$ docker exec 'contianer id or name' touch /jason.txt
```

* <code>exec</code>는 실행되고 있는 컨테이너 외부에서 컨테이너 내부를 contorl 할 수 있는 명령어입니다.

### Wrap-up

Docker의 기본 개념과 빈번히 사용되는 명령어에 대해서 알아보았습니다. 다음 시간에는 Docker를 활용한 CI(Continuous Integration) 환경 구축을 연습해보도록 하겠습니다.

### Reference

[Docker 소개](http://bcho.tistory.com/805)
[초보를 위한 도커 안내서 - 도커란 무엇인가?](https://subicura.com/2017/01/19/docker-guide-for-beginners-1.html)