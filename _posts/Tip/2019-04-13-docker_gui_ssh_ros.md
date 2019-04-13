---
layout: post
title: build ROS development environment with docker using ssh, GUI - (3)
comments: true
category: Tip
tags:
- docker
- ssh
- GUI
- ROS
---

이제 마지막으로 앞글에서 만들었던 ssh docker container와 GUI docker container를 병합하고 ROS개발환경까지 지원하는 docker container를 만들어보도록하겠다.

여기서는 ssh와 gui를 동시에 지원하고 ROS개발환경까지 지원하는데 필요한 추가적인 내용을 이야기하도록 하겠다.



혹시 해당 글을 읽기 귀찮고 바로 스크립트만 실행해서 바로 사용하고싶은 사람들은 아래 Repository를 이용하면 바로 작동여부를 확인할 수있다.

[https://github.com/ssaru/ssh-gui-ros-docker](https://github.com/ssaru/ssh-gui-ros-docker)



<br/>

## Install ROS

ROS는 ROS1 kinetic version이며, 해당 버전은 [ROS강의](https://www.youtube.com/watch?v=ot_D9N-H4lQ&list=PLRG6WP3c31_VIFtFAxSke2NG_DumVZPgw)에서 제공하는 script를 이용해서 설치했다.

```docker
# Install ROS
RUN wget https://raw.githubusercontent.com/ROBOTIS-GIT/robotis_tools/master/install_ros_kinetic.sh && sudo chmod 755 ./install_ros_kinetic.sh && bash ./install_ros_kinetic.sh

RUN rosdep update
```

<br/>

## Forward X11

ssh docker container를 만들고, ssh를 이용해서 GUI를 활용할 때는 ssh 접속시 `-X`옵션을 주게된다. 이때 `-X`옵션은 ssh server에서 `Forwrad X11`이 `yes`로 설정되어있어야한다. 따라서 주석을 해제하고 `yes`로 변경해준다.

```docker
# X11 Forward setting
RUN sudo sed -i 's/#   ForwardX11 no/    ForwardX11 yes/g' /etc/ssh/ssh_config
RUN sudo cat /etc/ssh/ssh_config
```

<br/>

## DISPLAY option set up as OS environment variable

앞서 GUI docker container를 생성할 때, host의 DISPLAY옵션을 `docker run`시 전달했었다. 전달된 host의 DISPLAY옵션은 docker container에서는 전달되지만 ssh 접속시에는 나타나지 않는다. 따라서 별도로 docker container의 os 환경변수에 이를 등록해두는 과정을 거쳤다.

```docker
# DISPLAY option setting
ENV DISPLAY ${display}
RUN echo "export DISPLAY=unix${display}" >> ~/.bashrc
RUN echo "display parameter : ${display}"
```

<br/>

## Dockerfile

전체 Dockerfile은 아래와 같다.

```docker
# base image
FROM ubuntu:16.04

# install basic environment
RUN rm -rf /var/lib/apt/lists/*
RUN apt-get clean
RUN apt-get autoremove

# change mirrors in ubuntu server: us to korea
RUN sed -i 's/security.ubuntu.com/ftp.daum.net/g' /etc/apt/sources.list
RUN sed -i 's/us.archive.ubuntu.com/ftp.daum.net/g' /etc/apt/sources.list
RUN sed -i 's/archive.ubuntu.com/ftp.daum.net/g' /etc/apt/sources.list

# install pre-required
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
                baobab \
                firefox \
                    && rm -rf /var/lib/apt/lists/*

# set up your account password here
ARG user
ARG uid
ARG gid
ARG passwd
ARG display

# Add new user with our credentials
ENV USERNAME ${user}

# Add sudoer s & check /etc/sudoers file
RUN echo "$USERNAME     ALL=NOPASSWD:       ALL" >> /etc/sudoers
RUN cat /etc/sudoers

# Add new user with our credentials
RUN useradd -m $USERNAME && \
        echo "$USERNAME:$passwd" | chpasswd && \
        usermod --shell /bin/bash $USERNAME && \
        usermod --uid ${uid} $USERNAME && \
        groupmod --gid ${gid} $USERNAME

# set up root passwd
RUN echo "root:$passwd" | chpasswd

# set up default user & home directory
USER ${user}
WORKDIR /home/${user}

# print build parameter
RUN echo "user: $user"
RUN echo "uid : $uid"
RUN echo "gid : $gid"
RUN echo "USERNAME : $USERNAME"
RUN echo "passwd : $passwd"

# ssh setting
RUN mkdir /home/${user}/.ssh
RUN sudo mkdir /var/run/sshd

# open port
EXPOSE 22

# Install ROS
RUN wget https://raw.githubusercontent.com/ROBOTIS-GIT/robotis_tools/master/install_ros_kinetic.sh && sudo chmod 755 ./install_ros_kinetic.sh && bash ./install_ros_kinetic.sh

RUN rosdep update

# X11 Forward setting
RUN sudo sed -i 's/#   ForwardX11 no/    ForwardX11 yes/g' /etc/ssh/ssh_config
RUN sudo cat /etc/ssh/ssh_config

# DISPLAY option setting
ENV DISPLAY ${display}
RUN echo "export DISPLAY=unix${display}" >> ~/.bashrc
RUN echo "display parameter : ${display}"

# attach local storate
RUN mkdir /home/${user}/Documents

# start ssh
CMD ["sudo", "/usr/sbin/sshd", "-D"]
```

<br/>

## Build dockerfile

아래 명령어를 이용해서 dockerfile을 빌드한다.

```bash
> sudo docker build --build-arg user=$USER --build-arg uid=$(id -u) --build-arg gid=$(id -g) --build-arg passwd="<YOUR PASSWD>" --build-arg display=$DISPLAY --tag ros:0.1.0 .
```

<br/>

## Make Accessible xauth

아래의 명령어를 이용해서 액세스 권한이있는 xauth 파일을 만든다.

```bash
> xauth nlist $DISPLAY | sed -e 's/^..../ffff/' | xauth -f /tmp/.docker.xauth nmerge -
```

<br/>

## Run docker container

아래의 명령어를 이용해서 docker container를 구동한다.

```bash
sudo docker run -i -t -d -P \
    -e DISPLAY=unix$DISPLAY \
    -v /tmp/.X11-unix:/tmp/.X11-unix \
    -v /tmp/.docker.xauth:/tmp/.docker.xauth:rw \
    -e XAUTHORITY=/tmp/.docker.xauth \
    --name ros ros:0.1.0
```

<br/>

## Inspection container network information

접속하기 전에 container의 ip와 port를 알아야하기 때문에 container의 network정보를 아래의 명령어로 확인하자

`--format`의 parameters 부분은 마크다운에서 해당 표현을 하지 못해서[링크](https://stackoverflow.com/questions/17157721/how-to-get-a-docker-containers-ip-address-from-the-host)를 확인하면 알 수 있다.

```bash
> sudo docker inspect --format='range .NetworkSettings.Networks .IPAddress end' <CONTAINER id or name>
> sudo docker port <CONTAINER id or name>

172.17.0.2
22/tcp -> 0.0.0.0:32808
```

<br/>

## Access to docker using ssh

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

<br/>

## Check ROS

터미널을 여러개 띄워서 해당 container에 ssh로 접속하고 아래의 명령어를 이용해서 ROS 개발환경이 잘 구축되었는지 확인해보자.

아래와 같은 turtlesim이 GUI화면으로 나오고, 화살표키를 이용해서 움직일 수 있다면 ROS개발환경까지 잘 구축이 된 것이다.

```bash
> roscore
> rosrun turtlesim turtlesim_node
> rosrun turtlesim turtle_teleop_key
> rosrun rqt_graph rqt_graph
```

![ROS](https://user-images.githubusercontent.com/13328380/56078750-aca87480-5e26-11e9-9748-4f89b4a3a6e8.png)

<br/>

### REFERENCE

---

[1. Linux 컨테이너에서 GUI 어플리케이션 실행하기](https://riptutorial.com/ko/docker/example/21831/linux-%EC%BB%A8%ED%85%8C%EC%9D%B4%EB%84%88%EC%97%90%EC%84%9C-gui-%EC%95%A0%ED%94%8C%EB%A6%AC%EC%BC%80%EC%9D%B4%EC%85%98-%EC%8B%A4%ED%96%89%ED%95%98%EA%B8%B0)

[2. Dockerize as SSH service](https://docs.docker.com/engine/examples/running_ssh_service/)
