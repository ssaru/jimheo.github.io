---
layout: post
title: Using OpenCV in virtualenv
comments: true
category: Tip
tags:
- virtualenv
- opencv
---

# Virtualenv에서 Global OpenCV사용

Python에서 OpenCV를 사용할 때, OpenCV를 컴파일해서 사용한다. virtualenv를 사용하게되면 global로 설치된 opencv를 사용할 수 없는데, opencv를 virtualenv를 통해서 사용하는 방법을 알아보자.

여기서는 Ubuntu 18.04에서 python3.6과 OpenCV-3.4.0을 이용해 테스트했으며, OpenCV는 Python2.x, Python3.x 대상으로 컴파일이 되서 Global환경에서는 사용할 수 있음을 가정한다.

​      

    

## python3

다음과 같이 Python3 인터프리터를 통해서 python3 패키지가 설치되는 경로를 확인할 수 있다.

```python
import site
site.getsitepackages()
>> ['/usr/local/lib/python3.6/dist-packages', '/usr/lib/python3/dist-packages', '/usr/lib/python3.6/dist-packages']
```

Python3 패키지가 설치되는 경로를 확인했다면, 해당 경로에 하나하나 들어가서 `cv` 로 시작되는 파일이 있는지 찾아보자.

여기서는 `/usr/local/lib/python3.6/dist-packages`에서 `cv2.cpython-36m-x86_64-linux-gnu.so`라는 파일을 발견할 수 있다.

<br/>

## OpenCV for virtualenv

이제 Global Python에서 사용되는  OpenCV so파일의 위치를 알았으니, 이를 복사해서 virtualenv 패키지 폴더에 복사하면 된다.

먼저 테스트하기 위해서 virtualenv 환경을 하나 만들어주고, `numpy` 를 설치해주자! (OpenCV는 numpy에 의존성을 갖는다.)

```bash
> virtualenv env
Using base prefix '/usr'
New python executable in /home/keti-1080ti/Desktop/test/env/bin/python3
Also creating executable in /home/keti-1080ti/Desktop/test/env/bin/python
Installing setuptools, pip, wheel...
done.

> source env/bin/activate
> pip3 install numpy

> ls
env

> cd env
> ls
bin  include  lib

> cd lib
> ls
python3.6

> cd python3.6
> ls
LICENSE.txt          config-3.6m-x86_64-linux-gnu  imp.py                       posixpath.py      struct.py
__future__.py        copy.py                       importlib                    random.py         tarfile.py
__pycache__          copyreg.py                    io.py                        re.py             tempfile.py
_bootlocale.py       distutils                     keyword.py                   reprlib.py        token.py
_collections_abc.py  encodings                     lib-dynload                  rlcompleter.py    tokenize.py
_dummy_thread.py     enum.py                       linecache.py                 shutil.py         types.py
_weakrefset.py       fnmatch.py                    locale.py                    site-packages     warnings.py
abc.py               functools.py                  no-global-site-packages.txt  site.py           weakref.py
base64.py            genericpath.py                ntpath.py                    sre_compile.py
bisect.py            hashlib.py                    operator.py                  sre_constants.py
codecs.py            heapq.py                      orig-prefix.txt              sre_parse.py
collections          hmac.py                       os.py                        stat.py
```

위에서 보이듯이 virtualenv의 패키지파일은 `env/lib/python3.6` 에 있음을 확인할 수 있고, 여기에 `cv2.cpython-36m-x86_64-linux-gnu.so` 파일을 복사하면 된다.

```bash
> cp /usr/local/lib/python3.6/dist-packages/cv* <YOUR VIRTUALENV DIRECTORY>/env/lib/python3.6/
```

<br/>    

## Test

이제 OpenCV가 Virtualenv 환경에서 잘 작동하는지 확인해보자.

```bash
> virtualenv env
> source env/bin/activate
> python3
Python 3.6.7 (default, Oct 22 2018, 11:32:17)
[GCC 8.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import cv2
>>>
```

다음과 같이 결과가 나온다면 정상적으로 virtualenv에서 OpenCV환경을 사용할 수 있게 된 것이다.
