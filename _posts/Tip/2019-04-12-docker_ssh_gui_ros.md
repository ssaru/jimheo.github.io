---
layout: post
title: build ROS development environment with docker using ssh, GUI - (1)
comments: true
category: Tip
tags:
- docker
- ssh
- GUI
- ROS
---

얼마전에 우분투 17.01에서 ROS 개발환경 구축을 위해서 작업하던 도중 우분투를 날려먹는 사태를 만들었다. 

덕분에 우분투 18.04를 재설치하고 Docker를 이용해 ROS 개발환경을 구축하기로 마음 먹었는데, 쉽게 풀릴 줄 알았던 문제가 생각보다 쉽지 않았다. 이를 해결하는 과정을 풀어보고 마지막에는 ROS개발 환경을 위한 Dockerfile을 만들어보겠다.



작업환경은 우분투 18.04에서 테스트했다.

<br/>

## Common setting

기본적으로 Dockerfile은 base image를 이용해서 셋팅하고 의존성 패키지를 설치해줘야하는데, 우분투 mirror 서버의 네트워크 속도로 인해서 `Hash Sum mismatch` error를 마주하게 된다. 

따라서 자신의 취향에 맞게 우분투 mirror 서버를 `/etc/apt/sources.list` 를 먼저 수정하는 작업을 진행한다.

```docker
# install basic environment
RUN rm -rf /var/lib/apt/lists/*
RUN apt-get clean
RUN apt-get autoremove
# change mirrors in ubuntu server: us to korea
RUN sed -i 's/security.ubuntu.com/ftp.daum.net/g' /etc/apt/sources.list
RUN sed -i 's/us.archive.ubuntu.com/ftp.daum.net/g' /etc/apt/sources.list
RUN sed -i 's/archive.ubuntu.com/ftp.daum.net/g' /etc/apt/sources.list

RUN apt-get -y update && \
        apt-get install -y [PACKAGES] \
            && rm -rf /var/lib/apt/lists/*
```

<br/>

## SSH 접속이 가능한 Docker container만들기

내가 학습하고있는 [ROS 강좌](https://www.youtube.com/watch?v=ot_D9N-H4lQ&list=PLRG6WP3c31_VIFtFAxSke2NG_DumVZPgw)에서는 하나의 PC에서 여러 터미널을 띄워놓고 시뮬레이션을 돌려야하는 경우가 있는데, Docker attach command를 이용할 경우 하나의 클라이언트만 붙을 수 있기 때문에 적합하지 않았다.

그래서 찾은게 Container에 ssh로 붙는 방법을 찾게 되었는데, 중요한 것은 GUI환경까지 고려한다면 `root` 가 아닌 Local의 계정과 같은 사용자 계정을 Docker Container에 구축해서 SSH 접속 환경을 만들어야하는 점이 중요했다.

`root` 계정을 이용해서 SSH에 접속하는 내용은 구글에 조금만 검색해보면 나오는 내용이였는데, 그 외의 계정을 만들어서 SSH환경을 만드는 내용은 많이 없어서 작업하는데 애를 먹었다.

결국 사용자 계정기반의 SSH container에서 중요한 내용은 아래와 같다.

- 필요한 의존성 패키지 설치

- 사용자 계정 생성

- SSH관련 셋팅

<br/>

### Install dependency package

먼저 ssh를 위한 패키지를 설치 및 ssh를 위한 디렉토리를 생성한다.

```docker
RUN apt-get -y update && \
        apt-get install -y \
                sudo \
                vim \
                sudo \
                curl \
                wget \
                apt \
                base-files \
                libapt-pkg5.0 \
                libc-bin \
                libkmod2 \
                libudev1 \
                multiarch-support \
                systemd-sysv \
                openssh-server \
                    && rm -rf /var/lib/apt/lists/*

RUN mkdir /var/run/sshd
```

<br/>

### Docker build parameter setting

Dockerfile build를 위한 파라미터를 준비한다. 여기서는 `user` , `uid`, `gid`를 사용했다.

```docker
ARG user
ARG uid
ARG gid

# Add new user with our credentials
ENV USERNAME ${user}
```

<br/>

### Add user account

user account에서 `sudo`를 사용할 수 있게 설정하고 받은 파라미터를 이용해서 Ubuntu user account를 만들어준다. 

혹시 모르니 `root`의 비밀번호도 같이 설정준 후, 기본 계정과 홈 디렉토리를 설정해준다.

```docker
# Add sudoer s & check /etc/sudoers file
RUN echo "$USERNAME     ALL=NOPASSWD:       ALL" >> /etc/sudoers
RUN cat /etc/sudoers

# Add new user with our credentials
RUN useradd -m $USERNAME && \
        echo "$USERNAME:$passwd" | chpasswd && \
        usermod --shell /bin/bash $USERNAME && \
        usermod --uid ${uid} $USERNAME && \
        groupmod --gid ${gid} $USERNAME

RUN echo "root:$passwd" | chpasswd

USER ${user}
WORKDIR /home/${user}
```

<br/>

### Open Port & execute ssh

이제 마지막으로 22번 포트를 개방한 후에 ssh를 실행한다.

```docker
EXPOSE 22

CMD ["sudo", "/usr/sbin/sshd", "-D"]
```

<br/>

### Dockerfile

전체 Dockerfile은 다음과 같다.

```docker
FROM ubuntu:16.04

# install basic environment
RUN rm -rf /var/lib/apt/lists/*
RUN apt-get clean
RUN apt-get autoremove

# change mirrors in ubuntu server: us to korea
RUN sed -i 's/security.ubuntu.com/ftp.daum.net/g' /etc/apt/sources.list
RUN sed -i 's/us.archive.ubuntu.com/ftp.daum.net/g' /etc/apt/sources.list
RUN sed -i 's/archive.ubuntu.com/ftp.daum.net/g' /etc/apt/sources.list

RUN apt-get -y update && \
        apt-get install -y \
                sudo \
                vim \
                sudo \
                curl \
                wget \
                apt \
                base-files \
                libapt-pkg5.0 \
                libc-bin \
                libkmod2 \
                libudev1 \
                multiarch-support \
                systemd-sysv \
                baobab \
                lsb-release \ 
                command-not-found \
                python \ 
                python-pip \ 
                   python3 \ 
                python3-pip \
                openssh-server \
                iptables \
                    && rm -rf /var/lib/apt/lists/*

RUN mkdir /var/run/sshd

ARG user
ARG uid
ARG gid
ARG passwd

# Add new user with our credentials
ENV USERNAME ${user}

# Add sudoer s & check /etc/sudoers file
RUN echo "$USERNAME     ALL=NOPASSWD:       ALL" >> /etc/sudoers
RUN cat /etc/sudoers

# set up your account password here
ARG user
ARG uid
ARG gid

# Add new user with our credentials
ENV USERNAME ${user}

# Add new user with our credentials
RUN useradd -m $USERNAME && \
        echo "$USERNAME:$passwd" | chpasswd && \
        usermod --shell /bin/bash $USERNAME && \
        usermod --uid ${uid} $USERNAME && \
        groupmod --gid ${gid} $USERNAME

RUN echo "root:$passwd" | chpasswd

USER ${user}
WORKDIR /home/${user}

# print build parameter
RUN echo "user: $user"
RUN echo "uid : $uid"
RUN echo "gid : $gid"
RUN echo "USERNAME : $USERNAME"
RUN echo "passwd : $passwd"        

EXPOSE 22

CMD ["sudo", "/usr/sbin/sshd", "-D"]
```

<br/>

### Dockerfile build

만든 dockerfile 다음과 같은 명령어를 이용해서 build한다.

여기서 `--build-arg` 에 들어가는 인자들은 모두 현재 사용하고 있는 우분투 계정과 같은 이름으로 생성했다.

```bash
> sudo docker build --build-arg user=$USER --build-arg uid=$(id -u) --build-arg gid=$(id -g) --build-arg passwd="<YOUR PASSWORD>" --tag ssh:0.1.0 .

Sending build context to Docker daemon  8.704kB
Step 1/31 : FROM ubuntu:16.04
 ---> 9361ce633ff1
Step 2/31 : RUN rm -rf /var/lib/apt/lists/*
 ---> Using cache
 ---> 4bd11ad5cf79
Step 3/31 : RUN apt-get clean
 ---> Using cache
 ---> c7095e0402df
Step 4/31 : RUN apt-get autoremove

...

Step 30/31 : EXPOSE 22
 ---> Using cache
 ---> 88a856e4047b
Step 31/31 : CMD ["sudo", "/usr/sbin/sshd", "-D"]
 ---> Using cache
 ---> 0e07a5e278ab
Successfully built 0e07a5e278ab
Successfully tagged ssh:0.1.0
```

<br/>

### Check Docker images

build한 이미지를 아래와 같은 명령어로 확인해보자

```bash
> sudo docker images

REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
ssh                 0.1.0               0e07a5e278ab        5 minutes ago       936MB
```

<br/>

### Docker run

이제 마지막으로 `docker run`명령어를 이용해서 만든 docker image의 container를 생성해보자

```bash
> sudo docker run -i -t -d -P --name ssh ssh:0.1.0

6cd6101473c6f22f5d2009b631e158112bc4c1dfc719b731021c82f6759d21b8
```

<br/>

### Check Docker container

구동시킨 docker container가 정상적으로 작동하는지 아래와 같은 명령어로 확인한다.

```bash
> sudo docker ps 

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                   NAMES
6cd6101473c6        ssh:0.1.0           "sudo /usr/sbin/sshd…"   21 hours ago        Up 21 hours         0.0.0.0:32808->22/tcp   ssh
```

> 만약에 `sudo docekr ps`로 구동중인 container가 안보인다면, `sudo docker logs <container id or name>`으로 작동하지 않은 원인을 찾고 이를 수정해서 다시 run을 시키면 된다.
> 
> EX> `sudo docker logs 6cd6101473c6` `sudo docekr logs ssh`

<br/>

### Check docker container network information

이제 구동중인 Docker container에 ssh로 접속할 일만 남았다.

접속하기 전에 container의 ip와 port를 알아야하기 때문에 container의 network정보를 아래의 명령어로 확인하자

`--format` 의 parameters 부분은 마크다운에서 해당 표현을 하지 못해서 [링크](https://stackoverflow.com/questions/17157721/how-to-get-a-docker-containers-ip-address-from-the-host)를 확인하면 알 수 있다.



```bash
> sudo docker inspect --format='range .NetworkSettings.Networks .IPAddress end' <CONTAINER id or name>
> sudo docker port <CONTAINER id or name>

172.17.0.2
22/tcp -> 0.0.0.0:32808
```

<br/>

### Access to docker using ssh

이제 구동하는 docker container의 ip와 port번호를 확인했으니, ssh로 접속해보자.

```bash
> ssh <USER ACCOUNT>@<CONTAINER IP>

The authenticity of host '172.17.0.2 (172.17.0.2)' can't be established.
ECDSA key fingerprint is SHA256:zvI1yePMst+4hOFXOEpJphB6aLTSO+ASvJgIml3PTno.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '172.17.0.2' (ECDSA) to the list of known hosts.
<USER ACCOUNT>@172.17.0.2's password:
Welcome to Ubuntu 16.04.6 LTS (GNU/Linux 4.18.0-17-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

<USER ACCOUNT>@159f79dd0bd7:~$
```

위와같은 화면이 뜨면서 접속이 되면, ssh로 접속이 가능한 docker container를 만든 것이다.

다음에는 docker를 이용해서 gui화면을 띄우는 방법에 대해서 이야기하겠다.
