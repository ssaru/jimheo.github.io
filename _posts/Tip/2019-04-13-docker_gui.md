---
layout: post
title: build ROS development environment with docker using ssh, GUI - (2)
comments: true
category: Tip
tags:
- docker
- ssh
- GUI
- ROS
---



이제 GUI를 지원하는 Docker container를 만들어보자.



## Make user account

기본적으로 Docker에서 GUI를 사용하려면 Xorg 즉, host의 X11을 docker의 X11과 연결시켜줘야한다. 앞에 글에서 약간의 힌트가 들어갔는데 host의 Xorg를 docker container와 연결하는 방법은 docker user account의 user id와 user group을 host account의 user id와 group id와 맞춰주는 것이다.



```docker
ARG user
ARG uid
ARG gid
ARG passwd

# Add new user with our credentials
ENV USERNAME ${user}

# Add sudoer s & check /etc/sudoers file
RUN echo "$USERNAME	ALL=NOPASSWD:	ALL" >> /etc/sudoers
RUN cat /etc/sudoers

# Add new user with our credentials
RUN useradd -m $USERNAME && \
	echo "$USERNAME:$passwd" | chpasswd && \
	usermod --shell /bin/bash $USERNAME && \
	usermod --uid ${uid} $USERNAME && \
	groupmod --gid ${gid} $USERNAME

USER ${user}
WORKDIR /home/${user}
```

<br/>

## Dockerfile

전체적인 dockerfile은 아래와 같다



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
		firefox \
		&& rm -rf /var/lib/apt/lists/*

ARG user
ARG uid
ARG gid
ARG passwd

# Add new user with our credentials
ENV USERNAME ${user}

# Add sudoer s & check /etc/sudoers file
RUN echo "$USERNAME	ALL=NOPASSWD:	ALL" >> /etc/sudoers
RUN cat /etc/sudoers

# Add new user with our credentials
RUN useradd -m $USERNAME && \
	echo "$USERNAME:$passwd" | chpasswd && \
	usermod --shell /bin/bash $USERNAME && \
	usermod --uid ${uid} $USERNAME && \
	groupmod --gid ${gid} $USERNAME

USER ${user}
WORKDIR /home/${user}

# print build parameter
RUN echo "user: $user"
RUN echo "uid : $uid"
RUN echo "gid : $gid"
RUN echo "USERNAME : $USERNAME"
RUN echo "passwd : $passwd"
```

<br/>

## Build Dockerfile

아래 명령어로 Dockerfile을 빌드하자.

빌드 시, host의 user id와 user group을 docker의 user id와 user group을 맞추기 위해서 host의 user account name, user id와 group id를 빌드 파라미터로 전달한다.

```bash
> sudo docker build --build-arg user=$USER --build-arg uid=$(id -u) --build-arg gid=$(id -g) --build-arg passwd="keti" --tag gui:0.1.0 .
```

<br/>

## Make Accessible xauth

이제 컨테이너를 생성하기 전에 아래의 명령어를 이용해서 액세스 권한이있는 xauth 파일을 만든다.

```bash
> xauth nlist $DISPLAY | sed -e 's/^..../ffff/' | xauth -f /tmp/.docker.xauth nmerge -
```

<br/>

## Docker run

이제 docker run 명령어를 이용해서 docker를 구동시키는데, 이 때 xauth파일을 전달한다.

```bash
> sudo docker run -i -t -d -P -e DISPLAY=unix$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix -v /tmp/.docker.xauth:/tmp/.docker.xauth:rw -e XAUTHORITY=/tmp/.docker.xauth --name gui gui:0.1.0
```

<br/>

## Docker attach

GUI를 위한 docker에서는 ssh환경을 만들지 않았기 때문에 attach를 이용해서 docker container에 접속하고, 접속된 container에서 gui작동을 확인한다.

```bash
> sudo docker attach <container id or container name>
> firefox
or
> baobab
```



위의 명령어를 실행했을 때, firefox화면이나, baobab gui 화면이뜨면 정상적으로 작동한 것이다.
