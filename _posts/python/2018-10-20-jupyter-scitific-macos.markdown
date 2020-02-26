---
title: Python 과학계산을 위한 Jupyter Notebook - macOS
date: 2018-10-20 09:00:00 +0900
layout: post
tags: [python, 파이썬, jupyter, jupyter-notebook, opensuse, leap 42.3, leap 15, raspberry pi, 라즈베리파이, odroid c2]
categories: [Linux, Programming]
---

과학계산을 위한 Jupyter 설치 및 구성까지 요약하고 있다.

## 과학계산을 위한 Python Jupyter

Raspberry Pi 3 위에 설치한 openSUSE LEAP 42.3 과 15.0 에서 과학계산을 위한 Python 개발환경과 Jupyter Notebook 환경을 정리한다. 
 - 개발 프론트 엔드: Jupyter
 - 과학계산 모듈: numpy, scipy 등

Python과 Virtualenv 환경을 더 알고 싶으면 다음 두 링크에 자세한 설명이 있다.
 - [Python - Install virtualenv on Linux]({% post_url /python/2017-04-03-virtualenv-linux %})

### Setup

시스템에 Python2, Python3 가 설치되었는지 확인:
 - macOS : Python2.7

### python.org 설치

python.org 에서 package 를 다운받아 설치한다.

#### Uninstall package installer

https://stackoverflow.com/questions/3819449/how-to-uninstall-python-2-7-on-a-mac-os-x-10-6-4/3819829#3819829

#### brew에서 Python 3 설치

brew로 최신 3.7을 설치

```
brew install python3
```


You can install Python packages with
  pip3 install <package>

They will install into the site-package directory
  
/Users/qkboo/Library/Python/3.7/site-packages



### pip User Scheme

Python2.6부터 User scheme 개념이 도입되며서 `pip`로 설치하는 패키지를 개별 사용자 환경 위치에 설치할 수 있다. 

install 명령에 `--user` 옵션을 주고 설치한다.[^2] 
 - 기본으로 리눅스는 *~/.local* 폴더이고, 
 - Mac OS X 는 *~/Library/Python/X.Y* 에 위치한다.[^3]

그리고 `PATH` 에 *~/.local/bin* 을 추가한다.


#### virtualenv 와 virtualenvwrapper

pip 를 업그레이드하고, 가상 개발환경에서 쥬피터 관련 모듈을 설치하고 관리하기 위해 `pip`로 virtualenv, virtualenvwrapper 설치한다. 

시스템 `pip` 모듈을 최신 버전으로 업그레이드한다.

```terminal
pip install --upgrade pip
```

그리고 virtualenv, virtualenvwrapper 설치하는데, 사용자의 `.local` 폴더에 설치하도록 한다.

```terminal
pip install --user virtualenv virtualenvwrapper
```

자동으로 추가되지 않으면, 다음 스크립을 `.bashrc` 에 추가해 준다.

```sh
# set PATH for pip
if [ -d "$HOME/.local/bin" ] ; then
    PATH="$HOME/.local/bin:$PATH"
fi

VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3
export WORKON_HOME=$HOME/.virtualenvs
export PROJECT_HOME=$HOME/Devel
source ~/Library/Python/3.7/bin/virtualenvwrapper.sh
```

로그아웃했다 로그인하면 `mkvirtualenv`, `rmvirtualenv` 등의 명령어 스크립이 설치된다.



#### Scientific stack

과학계산을 지원하는 Python 패키지를 설치한다.

```
pip install --user numpy scipy matplotlib jupyter pandas sympy nose
```


<br>
### Jupyter Notebook

Jupyter는 웹 브라우저를 통해서 IDE 환경을 제공하고, 다양한 언어/문법을 지원하는 Kernel 이라는 해석기를 통해 IDE에서 코딩한 결과를 확인할 수 있다. 

여기서는 가상환경을 구성해 Jupyter 관련 패키지를 설치하고 사용하겠다.
 - 시스템에 설치된 Python3 와 pip 모듈




#### Jupyter 가상환경 사용시

다음은 `mkvirtualenv` 명령으로 *jupyter*라는 가상환경을 *python3*, 시스템 패키지 사용을 위해 *--system-site-packages* 옵션으로 생성한다.

```terminal
mkvirtualenv -p python3 --system-site-packages jupyter
Running virtualenv with interpreter /usr/local/bin/python3
Using base prefix '/usr/local/Cellar/python/3.7.1/Frameworks/Python.framework/Versions/3.7'
New python executable in /Users/qkboo/.virtualenvs/jupyter/bin/python3.7
Also creating executable in /Users/qkboo/.virtualenvs/jupyter/bin/python
Installing setuptools, pip, wheel...


(jupyter) $
(jupyter) $ python --version
Python 3.4.6
```

가상환경 *jupyter* 에서 필수 모듈 numpy, scipy가 사용 가능한지 확인한다.

```sh
(jupyter) $ python -c "import numpy;print(numpy.__version__)"
1.9.3
(jupyter) $ python -c "import scipy;print(scipy.__version__)"
0.16.0
```


그리고 pip로 Jupyter 가상환경에 Jupyter를 설치한다.

```sh
(jupyter)$ pip install jupyter --user
```

Python User scheme 에 설치시 macOS는 아래 위치에 설치되므로 경로에 추가한다.

```
/Users/qkboo/Library/Python/3.7/bin
```

`jupyter-notebook`을 실행해 보고, 서버 관련 구성 내용을 설정해서 사용해 보자.


현재까지 설치된 pip 모듈 목록을 저장하자.

```
pip freeze --local > jupyter-requirements.txt
```


#### 서버로 실행

`jupyter-notebook` 은 로컬 머신에서 브라우저를 실행한다. 여기서는 원격 사용을 위해 다음 같이 시작할 수 있다. `[DIR]` 에 jupyter-notebook에서 작성한 노트북 파일이 저장된다.

```
$ jupyter-notebook --no-browser --ip=* --port=8000 [DIR]
[I 02:51:04.797 NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
[C 02:51:04.800 NotebookApp]

    Copy/paste this URL into your browser when you connect for the first time,
    to login with a token:
        http://localhost:8585/?token=a747472bc9a7c6684829267d2ed0a4cd9a722e
```

시작후 `?token=a.....` 부분의 토큰을 복사해 브라우저에 입력후 쥬피터 노트북 사용을 시작한다. 브라우저로 노트북에 접속해서 복사한 토큰을 입력한 후 로그인한다.

![](/images/python/jupyter5-run.png){:width="650"}


<br>
### Jupyter 설정 이용

jupyter는 설치된 후에 jupyter 시스템 디렉토리와 사용자 **JUPYTER_DATA_DIR**에 필요한 내용을 저장한다.

 - **시스템 디렉토리**: /usr/local/share/jupyter/
 - **JUPYTER_DATA_DIR** 는 보통 ~/.jupyter

설정 파일을 이용해서 사용자 비밀번호를 이용해 노트북을 사용할 수 있다.

먼저 실행중인 쥬피터 노트북에서 `passwd()` 를 실행해 **패스워드 해시 값**을 얻는다.

```python
In [1]: from notebook.auth import passwd
In [2]: passwd()
Enter password:
Verify password:
Out[2]: 'sha1:67c9e60bb8b6:9ffede0825894254b2e043ea597d771089e11eed'
```

패스워드 해시 값을 데이터 디렉토리의 설정 파일에 입력해야 한다.

#### 데이터 설정 파일

먼저 다음 같이 설정 파일을 생성한다.

```sh
$ jupyter notebook --generate-config
Writing default config to: /home/foo/.jupyter/jupyter_notebook_config.py
$ cd .jupyter && mv jupyter_notebook_config.py mynotebook.py
```

*mynotebook.py*에 다음을 설정한다.

```
#c.NotebookApp.notebook_dir = '/path/to/notebook_directory'
c.NotebookApp.base_url = 'http://www.yourdomain.com/notebok'
c.NotebookApp.password = ''
c.NotebookApp.port = 8000
c.NotebookApp.port_retries = 50
```

복사한 패스워드 해시 값을 mynotebook.py 설정 파일 안의 `c.NotebookApp.password` 항목 주석을 풀고 해시 값을 입력한다.

```
c.NotebookApp.password = 'sha1:4ee6bb2da3d7:ed76216b87228540e5f5f20fcfa8069cf82686f0'
```

설정 파일을 이용해 jupyter-notebook을 실행한다.

```sh
$ jupyter-notebook --config .jupyter/mybook_config.py [DIR]
```

이제 jupyter에 접속하려면 다음 같이 패스워드를 묻고 입력한 후에 사용할 수 있다.

![](/images/python/jupyter-login.png){:width="650"}



#### Background 실행

Jupyter-notebook을 백그라운드로 실행하려면 `nohup` 을 사용할 수 있다.

```sh
$ nohup jupyter notebook --no-browser --ip=* --port=8000 [DIR] > log-jupyter.log &
```

nohup은 재시동시 다시 시작해야 하는 불편한 점이 있다.


#### crontab 으로 실행

nohup 과 결합해서 사용해도 좋은 방법으로 crontab 을 사용해 재시동 후 자동으로 시작해 줄 수 있다. 사용자 crontab 을 편집해 다음 같이 사용할 수 있다.


```
@reboot cd /home/foo; jnohup jupyter notebook --no-browser --ip=* --port=8000 [DIR] > log-jupyter.log &
```


이 방법도 사용자가 시작/상태확인/종료 같은 직접적인 방법을 사용하기는 좀 불편하다.

그래서 upstart의 init.d 를 사용하거나 systemd 의 unit으로 생성해서 사용하는 것이 좋다.


#### systemd 에서 실행

systemd unit으로 새로운 unit 파일을 생성해서 기존 `systemctl` 명령으로 시작/상태/종료/재시작 등의 작업을 할 수 있다. 물론 재시동 관련해서도 완벽히 동작한다.

systemd의 unit 위치는 OS 마다 조금 다른 것 같다. 여기서는 */etc/systemd/system* 밑에 *jupyter.service* 라는 유닉 파일로 직접 작업한다.

 - /etc/systemd/system/jupyter.service

가상환경을 사용하고 있으므로 systemd unit의 `ExecStart` 의 python도 가상환경 위치로 지정해 주어야 한다.


```
[Unit]
Description=My Jupyter-Notebook

[Service]
Type=simple
PIDFile=/run/jupyter-notebook.pid
ExecStart=/home/foo/.virtualenvs/jupyter/bin/jupyter-notebook --config=/home/foo/.jupyter/mynotebook.py
User=foo
Group=foo
WorkingDirectory=/home/foo/notebooks
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target
```

그리고 시스템 유닛을 시작해 준다.

```
systemctl enable jupyter.service
systemctl daemon-reload
systemctl restart jupyter.service
```



### Upgrade jupyter

여기서는 pip 가상머신을 이용하고 있고, virtualenv, virtualenvwrapper는 여기서 사용자 .local 환경에 설치했으므로 

```terminal
pip install -U --user virtualenv virtualenvwrapper
```

다음 같이 pip install 명령으로 업그레이드 할 수 있다.

```terminal
(jupyter)$ pip install -U jupyter
```

```terminal
pip freeze —local > requirements.txt
```


## 참조

- [Pandas Install](http://pandas.pydata.org/pandas-docs/stable/install.html)
- [Scipy Install](https://www.scipy.org/install.html)
- [Scientific Python on Raspberry Pi](http://geoffboeing.com/2016/03/scientific-python-raspberry-pi/)

