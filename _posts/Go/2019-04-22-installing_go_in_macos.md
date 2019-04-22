---
layout: post
title: Installing Go in macos
comments: true
category: Go
tags:
- go
- macos
---



Kubernetes IN docker를 시도해보려고 했는데, MacOS에 Go가 없어서 간단히 설치함

레퍼런스랑 똑같으니, 레퍼런스만 봐도 된다. (해당 문서는 그냥 개인 공부차원에서...)



# Download Go for Mac

[Go 공식 홈페이지](https://golang.org/dl/)에서 MacOS를 위한 Go `pkg`파일을 다운로드 받는다.



![Download page](https://user-images.githubusercontent.com/13328380/56500378-7e711600-6545-11e9-95a5-0eeeee3c9efe.png)



# Installing

그냥 쭉쭉 설치해준다. (Keep continue)

![install-1](https://user-images.githubusercontent.com/13328380/56500415-aa8c9700-6545-11e9-96f3-7979157ece94.png)



# Verified installing

이제 터미널을 열어서 `go`, `go env`명령어를 입력해서 잘 설치되었는지 확인해보자.

> 만약 이전에 터미널이 열려있다면, 종류 후에 다시 시작하자.



```bash
> go version
go version go1.12.4 darwin/amd64

> go env
GOARCH="amd64"
GOBIN=""
GOCACHE="/Users/martin/Library/Caches/go-build"
GOEXE=""
GOFLAGS=""
GOHOSTARCH="amd64"
GOHOSTOS="darwin"
GOOS="darwin"
GOPATH="/Users/martin/go"
GOPROXY=""
GORACE=""
GOROOT="/usr/local/go"
GOTMPDIR=""
GOTOOLDIR="/usr/local/go/pkg/tool/darwin_amd64"
GCCGO="gccgo"
CC="clang"
CXX="clang++"
CGO_ENABLED="1"
GOMOD=""
CGO_CFLAGS="-g -O2"
CGO_CPPFLAGS=""
CGO_CXXFLAGS="-g -O2"
CGO_FFLAGS="-g -O2"
CGO_LDFLAGS="-g -O2"
PKG_CONFIG="pkg-config"
GOGCCFLAGS="-fPIC -m64 -pthread -fno-caret-diagnostics -Qunused-arguments -fmessage-length=0 -fdebug-prefix-map=/var/folders/y9/4rssv2y56yb395xt_yflbhqr0000gn/T/go-build144795127=/tmp/go-build -gno-record-gcc-switches -fno-common"
```



`GOPATH`를 변경하면, 원하는 Go 프로젝트 폴더를 변경해가면서 쓸 수 있는데, 필요성을 느껴서 더 이상 진행하지 않았다. 더 자세히 알고싶으면, 레퍼런스를 참조하면된다.



### REFERENCE

[1. Go 프로그래밍 - 맥(Mac) 기본 환경설정(1)]([https://niceman.tistory.com/128](https://niceman.tistory.com/128))
