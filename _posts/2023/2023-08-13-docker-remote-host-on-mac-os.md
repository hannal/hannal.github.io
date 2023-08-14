---
layout: post
status: publish
published: true
title: 맥을 Docker remote host로 사용하기
author:
  display_name: Kay
  login: Kay
  email: kay@hannal.net
  url: "https://blog.hannal.com"
author_login: Kay
author_email: kay@hannal.net
date: "2023-08-13 12:30:00"
categories: [devlife]
tags:
  - docker
  - mac
  - 아이맥
  - imac
  - docker remote host
  - orbstack
permalink: "/2023/08/use-mac-as-docker-remote-host/"
---

## 배경

작업 PC로 인텔 아이맥 27인치를 사용한다. 디스플레이가 괜찮은데다 램을 128기가로 넉넉하게 구성해서 유용하다. 그런데 CPU 발열이 심한 편이라서 CPU 과부하가 심할 땐 과열로 시스템이 뻗곤 한다. 그래서 작업 PC로 사용할 맥 스튜디오를 샀다. 가격이 부담스러워서 램 용량이 적은 32기가 메모리 모델을 샀는데, 램을 주로 점유하는 것들을 아이맥에서 띄워 서버용으로 사용할 계획이기 때문이다.

램을 많이 사용하는 것 중 하나는 Docker이다. 그래서 아이맥을 Docker remote host용으로, 간단히 말해서 Docker 서버용으로 사용하도록 설정하기로 했다. 그런데 Docker를 서버로 구동할 `dockerd`는 Linux에서 동작하며, Mac OS는 지원하지 않기 때문에 `virtualbox`를 이용해 Linux를 가상화하여 Guest OS로 구동하여 Docker를 사용한다. 따라서 아이맥에 virtualbox로 Linux를 설치하고, 그 가상화로 구동하는 Linux에 구성한 dockerd를 사용해야 한다. Host OS인 Mac OS와 Guest OS인 Linux 간 네트워크도 연결해주고.

귀찮기도 하고 웬지 무겁고 비효율적일 것 같은데, 이 글에서 사용할 Docker desktop 대체재인 [OrbStack](https://orbstack.dev)을 사용하여 쉽게 설정하고 가볍게 동작하는 환경을 구성할 수 있다.

- 아이맥 : Ventura 13.4
- Docker client : OrbStack

## Docker 설치

- Docker remote host인 아이맥에 [OrbStack 설치](https://docs.orbstack.dev/install).
- 원격으로 접근할 client인 맥스튜디오에도 설치.

### Docker engine을 daemon으로 구동할 Ubuntu 서버 구성

- 아이맥에 OrbStack으로 Linux machine을 만든다.
  - 참고 : [Using Linux machines](https://docs.orbstack.dev/machines/)

```bash
$ orb create --arch amd64 ubuntu my-ubuntu
```

아이맥이 인텔맥이라서 아키텍쳐를 `amd64`로 지정했다.

Linux machine을 만들고 나면 ssh로 접근 가능하다.

```bash
$ orb -m my-ubuntu -u default
```

기본으로 생성되는 계정 이름이 `default`이다. 기본으로 존재하는 `root`로 접근해도 된다.

Linux machine을 생성하면 OrbStack이 이 ubuntu에 ssh로 접근할 수 있게 ssh host configuration을 자동으로 만들어줘서 더 적게 타이핑하여 접근할 수 있다.

```bash
$ ssh orb
```

`orb`가 OrbStack이 만든 ssh host configuration이다.

```
# ~/.orbstack/ssh/config
# 이 파일은 ~/.ssh/config 에서 Include 하도록 OrbStack이 설정한다.

Host orb
  Hostname 127.0.0.1
  Port 32222
  # SSH user syntax:
  #   <container>@orb to connect to <container> as the default user (matching your macOS user)
  #   <user>@<container>@orb to connect to <container> as <user>
  # Examples:
  #   ubuntu@orb: container "ubuntu", user matching your macOS user
  #   root@fedora@orb: container "fedora", user "root"
  User default

  # replace or symlink ~/.orbstack/ssh/id_ed25519 file to change the key
  IdentityFile ~/.orbstack/ssh/id_ed25519

  # only use this key
  IdentitiesOnly yes
  ProxyCommand '/Applications/OrbStack.app/Contents/Frameworks/OrbStack Helper (VM).app/Contents/MacOS/OrbStack Helper (VM)' ssh-proxy-fdpass /Users/hannal
  ProxyUseFdpass yes
```

### Docker engine 설치

- Linux machine에 접근하여 docker engine을 설치한다.
- 참고 : [Install Docker Engine on Ubuntu](https://docs.docker.com/engine/install/ubuntu/)

### 충돌날만한 불필요한 패키지 제거

```bash
$ for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done
```

### Docker repository 설정

HTTPS로 repository에서 패키지를 가져와 설치해야 하므로 관련 패키지를 설치한다.

```bash
$ sudo apt update
$ sudo apt install ca-certificates curl gnupg
```

Docker의 공식 GPG 키를 추가한다.

```bash
$ sudo install -m 0755 -d /etc/apt/keyrings
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
$ sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

Docker repository를 등록한다.

```bash
$ echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# repository를 새로 등록했으니 update 해준다.
$ sudo apt update
```

Docker 공식 repository에서 패키지를 가져와 설치한다.

```bash
$ sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

### Docker를 system 서비스로 등록한다

- 나는 systemd를 사용하므로 systemd 설정을 했다.
- 참고 : [Configure remote access for Docker daemon](https://docs.docker.com/config/daemon/remote-access/)

```bash
$ sudo systemctl enable docker.service
$ sudo systemctl enable containerd.service
```

### Remote access되도록 docker 설정

`/etc/docker/daemon.json`에 설정하거나 systemd에서 docker 서비스 실행에서 인자로 지정하거나. 여기에선 후자 방법으로 했다. daemon 설정이 더 늘어나면 그때 분리하려 한다.

후자 방법은 `/etc/systemd/system/docker.service.d/override.conf` 파일을 편집하는 것이다. 이런 [단위 구조는 `edit` 명령을 사용](https://www.digitalocean.com/community/tutorials/how-to-use-systemctl-to-manage-systemd-services-and-units#editing-unit-files)하면 구성된다.

```bash
$ sudo systemctl edit docker.service
```

이제 `/etc/systemd/system/docker.service.d` 디렉터리가 생기고 그 안에 `override.conf` 파일이 만들어지는데, 그곳에 `dockerd` 실행 유닛을 작성한다.

```conf
[Service]
ExecStart=
ExecStart=/usr/bin/dockerd -H fd:// -H tcp://127.0.0.1:2375
```

`-H` 옵션은 Docker daemon 소켓이 연결할(사용할) 호스트를 지정한다. `-H fd://`는 Unix file descriptor로 호스트 연결을 하는 것이고, `-H tcp://127.0.0.1:2375`는 `127.0.0.1:2375` TCP IP로 연결하는 것이다. `-H fd://`는 기본(default) 옵션이고, TCP로 원격 접근하도록 `-H tcp://...` 옵션을 추가한 것이다.

systemd service 스크립트를 수정하지 않고 `override.conf`를 수정하는 이유는 자동으로 `override.conf`가 포함되기 때문이다. `sudo systemctl status docker.service` 명령을 실행해 확인해보자.

```bash
$ sudo systemctl status docker.service

● docker.service - Docker Application Container Engine
     Loaded: loaded (/lib/systemd/system/docker.service; enabled; preset: enabled)
    Drop-In: /etc/systemd/system/docker.service.d
             └─http-proxy.conf, override.conf
     Active: active (running) since Sun 2023-08-13 17:54:09 KST; 1h 24min ago
TriggeredBy: ● docker.socket
       Docs: https://docs.docker.com
   Main PID: 12127 (dockerd)
      Tasks: 20
     Memory: 24.6M
        CPU: 973ms
     CGroup: /system.slice/docker.service
             └─12127 /usr/bin/dockerd -H fd:// -H tcp://127.0.0.1:2375
```

`Drop-In`에 보면 include한 conf 파일 중 하나인 `override.conf`가 명시되어 있다.

systemd service script를 변경했으니 systemd가 해당 script를 다시 적재하도록 하고, docker를 재실행한다.

```bash
$ sudo systemctl reload docker.service

$ sudo systemctl restart docker.service
```

## Mac OS에 TCP forwarding 설정

Docker remote host에 Linux machine을 만들어 docker daemon을 띄웠으니 외부에서 이 host에 접근해 docker host에 접근하도록 TCP 포워딩을 한다. Linux machine에 하는 것이 아니라 Linux machine를 guest OS로 구동한 Host OS인 Mac OS에 설정하는 것이다. 내 경우는 아이맥에 하는 것이다.

### 원격 로그인 설정

[System Settings] - [General] - [Sharing]에 들어간다(한국어로 뭐라고 나오는지는 모르겠다). `Remote Login`(원격 로그인?)을 활성화한다. ssh daemon을 사용할지를 설정하는 것이다. 느낌표 아이콘을 클릭하면 ssh로 접근을 허용할 계정을 지정하는 화면이 나온다. 난 `Administrators`는 빼고 내 개인 계정을 추가했다.

다른 Host에서 ssh로 접근 가능한지 테스트 해본다. 난 맥 스튜디오에서 아이맥으로 접근하는 것이다.

```bash
$ ssh -i ~/.ssh/id_rsa hannal@ip-address
```

잘 된다. 타이핑하기 귀찮으니 ssh host 설정을 해준다.

```conf
# ~/.ssh/config 파일

Host imac
  HostName ip주소
  User hannal
  IdentityFile ~/.ssh/id_rsa
  IdentitiesOnly yes
  PasswordAuthentication no
```

이제 호스트명인 `imac`으로 간편하게 접근된다.

```bash
$ ssh imac
```

이 ssh host인 `imac`은 ssh 호스트가 필요한 곳엔 어디에서든지 사용할 수 있으므로 설정해두면 편리하다.

### SSH daemon에 TCP forwarding 허용 설정

`/etc/ssh/sshd_config` 파일을 열어 `AllowTcpForwarding`과 `GatewayPorts` 설정을 추가하거나 주석을 해제한다.

```conf
AllowTcpForwarding yes
GatewayPorts yes
```

이런 설정을 하는 이유는 OrbStack은 보안을 이유로 OrbStack이 관리하는 Linux machine에 원격으로 접근하지 못하기 때문이다. OrbStack이 구동한 localhost에서만 접근 가능하다.

ssh daemon 설정을 변경하면 ssh daemon에 반영되도록 재실행해야 한다. 앞서 [Sharing]에 있던 [Remote Login]을 비활성화했다가 다시 활성화하면 된다.

## Client 설정

Client, 즉, 램이 32기가 뿐인 맥 스튜디오에 있는 docker가 아이맥에 있는 docker를 서버로 사용하도록 지정하는 건 대개 두 가지 방법 중 하나로 한다.

### DOCKER_HOST 환경 변수 사용

환경변수인 `DOCKER_HOST`에 docker가 사용할 host를 지정하는 방법이다.

```bash
$ export DOCKER_HOST=imac
$ docker-compose up

# 또는

$ DOCKER_HOST=imac docker-compose up
```

하지만 환경변수는 실수하기 십상이니 다른 방법을 사용한다.

### docker context 사용

docker에서 제공하는 context 기능을 이용하는 것이다. 먼저 Docker remote host인 imac에 대한 context를 만든다.

```bash
$ docker context create --docker host=ssh://imac imac-docker
```

`imac-docker`는 context 이름이다. 잘 만들어졌는지 확인해본다.

```bash
$ docker context list

NAME            DESCRIPTION   DOCKER ENDPOINT       ...
default *       Current ...   unix:///var/run/docker.sock
imac-docker                   ssh://imac
orbstack        OrbStack      unix:///Users/hannal/.orbstack/ ...
```

`default`가 기본으로 존재하고, OrbStack을 설치하면 `orbstack`도 생긴다. 이들을 포함하여 방금 만든 `imac-docker` context가 목록에 나온다.

docker context는 `use` 보조 명령어로 사용한다.

```bash
$ docker context use imac-docker

Current context is now "imac-docker"
```

이제 docker는 `imac-docker` context에 지정된 Host에 연결한다. 확인해보자. `imac-docker` context를 지정한 상태에서 docker container를 구동한다.

```bash
$ docker-compose up

[+] Running 4/0
 ✔ Container redis      Created                     0.0s
Attaching to redis
```

아이맥에 있는 docker에 컨테이너가 구동된 것이라면 맥 스튜디오에 있는 docker에는 구동된 컨테이너가 없을테니 그걸 확인한다.

```bash
$ docker context use default

$ docker ps

CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

아무 컨테이너도 없다. 아이맥에 있는 docker를 확인해본다.

```bash
$ ssh imac

$ docker ps

CONTAINER ID   IMAGE                ...
2b806a9e441e   redis:7              ...
```

잘 떠있다. 이렇게 5K 해상도 디스플레이가 달린 Docker 서버를 마련했다.

## 뜻밖의 공지

그나저나 오랜만에 블로그에 글 올리려고 jekyll을 실행하니 설치에서 시간 낭비했고, 동작도 느리다. hugo로 이전해보려 했는데, 깔끔하게 이전되지도 않는다.

요즘엔 글을 안 쓰기도 하고, 관리하는 것도 은근히 신경 쓰이니 이 참에 개인 블로그를 닫아야겠다.

2003년부터 운영해온 개인 블로그는 2023년 8월 13일자로 닫습니다. 그동안 한날의 개인 블로그에 와주신 분들께 감사 인사 드립니다.
