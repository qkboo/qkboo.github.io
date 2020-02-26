---
title: Python - 과학계산을 위한 Jupyter(pip)
date: 2017-10-30 09:00:00 +0900
layout: post
tags: [python, virtualenv, virtualenvwrapper, jupyter, jupyter-notebook]
categories: [Linux, Programming]
---

과학계산을 위한 Python3 및 pip를 사용한 scipy, jupyter 설치 및 구성을  요약하고 있다.

https://packaging.python.org/guides/installing-scientific-packages/


## 과학계산을 위한 Python Jupyter

과학계산을 위한 Python 과 Jupyter Notebook 환경을 정리한다. 
 - 개발 프론트 엔드: Jupyter
 - 과학계산 모듈: numpy, scipy 등

Python과 Virtualenv 환경을 더 알고 싶으면 다음 두 링크에 자세한 설명이 있다.
 - [Python - Install virtualenv on Linux]({% post_url /python/2017-04-03-virtualenv-linux %})

### Setup

시스템에 Python2, Python3 가 설치되었는지 확인:
 - Ubuntu/Debian 계열: Python 2.7, Python 3.5 설치되어 있다.
 - openSUSE : Python2.7

#### Python 3 설치


#### 시스템 개발 패키지 설치

컴파일에 필요한 헤더 등이 포함된 개발자 라이브러리를 설치한다.

```
sudo apt-get install build-essential
sudo apt-get install python-dev python-distlib python3-dev python3-distlib
```

python-dev, python-distlib, apython-setuptools 은 파이썬 개발과 패키징을 지원한다.

apt-get install libfreetype6-dev
apt-get install pkg-config
apt-get install libpng-dev
apt-get install pkg-config


libzmq3-dev은 쥬피터 노트북에서 필요로 한다.

```
sudo apt-get install libzmq3-dev
```

- python-curses: Python이 (N)Curses Libr에 대한 인터페이스이다

Pandas 로 데이터 셋트를 다룰 예정, libgdal-dev 은 geopandas에서 geospatial analysis 에 필요하다.


```
sudo apt-get install libgdal-dev
```

For Debian Jessie and Stretch installing the following packages resolves the issue:

```terminal
sudo apt-get install libblas3 liblapack3 liblapack-dev libblas-dev
```

Your next issue is very likely going to be a missing Fortran compiler, resolve this by installing it like this:

```terminal
sudo apt-get install gfortran
```


#### pip 설치

```terminal
apt-get install python3-pip
```

python3 설치후 마지막으로 `update-alternatives` 를 이용해 `pip` 를 pip3 로 연결해 준다. 

다른 배포본에서 필요시 update-alternatives 사용:

```terminal
sudo update-alternatives --install /usr/bin/pip pip /usr/bin/pip3 30
```

시스템 `pip` 모듈을 최신 버전으로 업그레이드한다.

```terminal
sudo pip install -U pip
sudo pip install -U setuptools
```



### `pip`로 설치하기

Python2.6부터 User scheme 개념이 도입되며서 `pip`로 설치하는 패키지를 개별 사용자 환경 위치에 설치할 수 있다. 

install 명령에 `--user` 옵션을 주고 설치한다.[^2] 
 - 기본으로 리눅스는 *~/.local* 폴더이고, 
 - Mac OS X 는 *~/Library/Python/X.Y* 에 위치한다.[^3]
 - 그리고 `PATH` 에 *~/.local/bin* 을 추가한다.

#### Scientific stack

아래 같이 사용자 영역에 python3 기반으로 설치해보자, pip 를 파이썬 버전의 site-package 에 모듈을 설치하기 위해서 파이썬 `-m` 을 사용한다.

```terminal
python3 -m pip install --user numpy sympy nose
```

scipy는 설치 시스템에 따라 시간이 많이 걸린다. 또한 swap 영역을 사용하므로 스왑파일시스템을 활성화 하는 것을 권한다.

```terminal
python3 -m pip install --user scipy
```

##### error

스왑 파일시스템을 사용하지 않거나 모자라면 아래 같이 가상메모리 에러 혹은 컴파일 에러가 나는 것 같다.

```
     #warning "Using deprecated NumPy API, disable it by " \
      ^~~~~~~
    virtual memory exhausted: Cannot allocate memory
```


> 글쓴이는 OdroidC2, Armbian Stretch 에서 시도했다.

#### pandas 및 

```terminal
python3 -m pip install --user pandas
```

파이썬이 있고 pip를 사용하여 패키지를 설치하려면 다음 명령을 사용합니다.

matplotlib  pillow graphviz

scikit-learn 



#### conda를 사용한 패키지 설치

설치된 파이썬이 있다면 conda 패키지 매니저를 사용하여 다음 명령을 실행하면 필요한 패키지를 모두 얻을 수 있습니다.

conda install numpy scipy scikit-learn matplotlib pandas pillow graphviz python-graphviz
pip를 사용한 패키지 설치



#### 설치 확인

설치되고 사용이 가능한지 확인한다. 다음 두 모듈이 없으면 jupyter 설치가 제대로 안된다.

```terminal
python3 -c "import numpy;print(numpy.__version__)"
1.9.3
```

```terminal
python3 -c "import scipy;print(scipy.__version__)"
0.16.0
```

```terminal
python3 -c "import numpy;print(matplotlib.__version__)"
```

<br>
### Jupyter Notebook

Jupyter는 웹 브라우저를 통해서 IDE 환경을 제공하고, 다양한 언어/문법을 지원하는 Kernel 이라는 해석기를 통해 IDE에서 코딩한 결과를 확인할 수 있다. 

```sh
$ python3 -m pip install --user jupyter 
```

> 물론 가상환경이 아닌 시스템 패키지로 설치해도 된다.
> $ sudo pip install jupyter geopandas osmnx

`jupyter-notebook`을 실행해 보고, 서버 관련 구성 내용을 설정해서 사용해 보자.

##### Upgrade jupyter

여기서는 pip 가상머신을 이용하고 있어서 다음 같이 pip install 명령으로 업그레이드 할 수 있다.

```terminal
$ python3 -m pip install -U jupyter
```


python3 -m pip freeze —local > requirements.txt




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
$ jupyter notebook --config .jupyter/mybook_config.py [DIR]
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



## 참조

- [Pandas Install](http://pandas.pydata.org/pandas-docs/stable/install.html)
- [Scipy Install](https://www.scipy.org/install.html)
- [Scientific Python on Raspberry Pi](http://geoffboeing.com/2016/03/scientific-python-raspberry-pi/)

