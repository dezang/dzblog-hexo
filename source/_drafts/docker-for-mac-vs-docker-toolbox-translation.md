---
title: Docker for Mac vs. Docker Toolbox 한글 번역
categories: docker
tags: docker
---
도커가 처음 나온 시절. 기본적으로 리눅스에서만 돌아가는 도커를 위해 도커 커뮤니티에서 docker toolbox 를 만들었다. 어느 정도 시간이 흐른 뒤 다시 확인해보니 docker for mac 이라는 어플리케이션이 나왔더라. 그럼 기존의 툴박스는 제거해야 하는건가? 도커 홈페이지에 이에 관련하여 [작성된 글](https://docs.docker.com/docker-for-mac/docker-toolbox/)이 있어 읽어가면서 차근히 번역하여 작성하려 한다.(Feat. 구글 번역기)

<!-- more -->
---

Docker Toolbox가 이미 설치되어있는 경우 이 항목을 먼저 읽고 그 둘의 차이점과 공존 할 수 있는 방법에 대해서 알아보자.

## Docker Toolbox 환경
도커 툴박스는 맥의 `/usr/local/bin` 경로에 `docker`, `docker-compose`, `docker-machine`을 설치한다. 또한 VirtualBox 도 설치한다. 설치시 Toolbox는 docker-machine을 사용하여 boot2docker Linux 배포판을 실행하는 default라는 VirtualBox VM을 제공하고 Mac의 인증서가있는 Docker Engine을 `$HOME/.docker/machine/machines/default`에 배치한다.

Mac에서 `docker` 또는 `docker-compose`를 사용하기 전에 일반적으로 `eval $(docker-machine env default)` 명령을 사용하여 `docker` 또는 `docker-compose` 가 VirtualBox에서 실행 중인 Docker Engine과 통신할 수 있도록 환경 변수를 설정한다.

이 설정은 아래 다이어그램으로 확인할 수 있다.

![](https://docs.docker.com/docker-for-mac/images/toolbox-install.png)

## Docker For Mac 환경
Docker For Mac은 맥 네이티브 어플리케이션으로 `/Applications` 에 설치한다. 설치시 `/Applications/Docker.app/Contents/Resources/bin`에 있는 `docker`와 `docker-compose`의 심볼릭 링크를 `/usr/local/bin`에 만든다.

*직접 까보니 심볼릭 링크로 만드는 것은 `docker`, `docker-compose`, `docker-machine`, `notary` 이렇게 네 개다. 툴박스를 설치하면서 있던 파일은 `.backup`을 붙여서 백업해두는 듯하다.*

시작하기 전에 Docker for Mac에 대해 알아야 할 핵심 사항은 아래와 같다.

- Docker for Mac은 VirtualBox가 아니라 macOS 10.10 요세미티 이상의 Hypervisor.framework 위에 구축된 경량 macOS 가상화 솔루션인 [HyperKit](https://github.com/docker/HyperKit/)을 사용한다. ~~이건 또 뭥미!~~
- Docker for Mac 을 설치해도 도커 머신으로 설치한 머신들에는 영향을 미치지 않는다. 이 설치는 로컬 기본 컴퓨터(존재하는 경우)에서 맥용 새 Docker HyperKit VM으로 컨테이너 와 이미지를 복사한다. 이 옵션을 선택하면 기본값의 컨텐츠가 새로운 맥용 도커 HyperKit VM으로 복사되고 기존 시스템은 그대로 유지된다.
- Docker for Mac 은 해당 VM을 재공하기 위해 도커 머신을 사용하지 않는다. 직접 생성하고 관리한다.
- Docker for Mac 은 도커 엔진을 실행하는 Alpine Linux 기반에 HyperKit VM을 제공한다. 이는 `/var/tmp/docker.sock`의 소켓에 docker API를 제공한다. 따라서 환경 변수가 설정되지 않은 경우에도 도커가 바라보는 기본 위치이므로 어떠한 환경 변수 설정없이도 `docker`와 `docker-compose`를 사용할 수 있다.

이 설정은 아래 다이어그램으로 확인할 수 있다.

![](https://docs.docker.com/docker-for-mac/images/docker-for-mac-install.png)


----
#### 참고
