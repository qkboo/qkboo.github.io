---
title: openSUSE - 과학계산을 위한 Python, Jupyter Notebook
date: 2019-09-05 20:00:00 +0900
layout: post
tags:
  [
    python,
    파이썬,
    Scientific,
    Numpy,
    jupyter-notebook,
    opensuse leap 15,
    raspberry pi,
    라즈베리파이,
    odroid c2,
  ]
categories: [Linux, Programming]
---

이 글은 OpenSuse LEAP 15 에서 과학계산을 위한 파이썬 환경을 위한 Jupyter Notebook을 설치하는 과정을 요약하고 있다.

이 글에서는 시스템 패키지로 Python3, jupyter-notebook 을 설치하고, 파이썬 가상환경에서 과학계산 파이썬 모듈을 설치한 후에 Jupyter notebook 을 운영하도록 한다.

**과학계산을 위한 _Python Jupyter_ 환경**

아래 같은 플랫폼에서 시스템 패키지 소프트웨어와 파이썬 모듈을 설치하는 과정이다.

- Raspberry Pi 3
- openSUSE LEAP 42.3, 15.0, 15.1
- Jupyter Notebook
- Python 그래프 matplotlib 및 pillow 이미지 라이브러리
- 과학계산 모듈: numpy, scipy 등
- 파이썬 가상환경
  - pandas

Python과 Virtualenv 환경을 더 알고 싶으면 다음 두 링크에 자세한 설명이 있다.

- [Python - Install virtualenv on Linux]({% post_url /python/2017-04-03-virtualenv-linux %})

<br>

## 설치

시스템에 Python2, Python3 가 설치되었는지 확인:

#### Python 환경

openSUSE 15.0에서 파이썬은 python2, python3.6 시스템 패키지로 설치되어 있다.

pip를 설치해 준다.

```sh
sudo zypper in python3-pip
```

pip 를 여러 버전 사용중이라면,

> 만약 python2-pip 도 사용중이고 pip를 pip3로 사용하려면 `update-alternatives` 를 이용해 `pip` 를 pip3 로 연결해 준다. 다른 배포본에서 필요시 update-alternatives 사용:
>
> ```
> sudo update-alternatives --install /usr/bin/pip pip /usr/bin/pip3 30
> ```

#### 시스템 개발 패키지 설치

컴파일에 필요한 헤더 등이 포함된 개발자 라이브러리를 설치한다. `pattern devel` 로 검색해 보면,,,

```terminal
sudo zypper install -t pattern devel

S | Name                 | Summary                             | Type
--+----------------------+-------------------------------------+--------
  | devel_C_C++          | C/C++ Development                   | pattern
i | devel_basis          | Base Development                    | pattern
  | devel_gnome          | GNOME Development                   | pattern
  | devel_ide            | Integrated Development Environments | pattern
  | devel_java           | Java Development                    | pattern
  | devel_kde            | KDE Development                     | pattern
  | devel_kde_frameworks | KDE Frameworks Development          | pattern
  | devel_kernel         | Linux Kernel Development            | pattern
  | devel_mono           | .NET Development                    | pattern
  | devel_perl           | Perl Development                    | pattern
  | devel_python         | Python Development                  | pattern
  | devel_python3        | Python 3 Development                | pattern
  | devel_qt4            | Qt 4 Development                    | pattern
  | devel_qt5            | Qt 5 Development                    | pattern
  | devel_rpm_build      | RPM Build Environment               | pattern
  | devel_ruby           | Ruby Development                    | pattern
  | devel_tcl            | Tcl/Tk Development                  | pattern
  | devel_web            | Web Development                     | pattern
  | devel_yast           | YaST Development                    | pattern

```

이 중에서 devel_basis, python 관련 개발용 모듈을 설치한다.

```terminal
sudo zypper install -t pattern devel_basis
```

파이썬 개발 모듈을 설치한다.

```terminal
sudo zypper install -t pattern devel_python devel_python3
sudo zypper install python-distutils-extra
```

curses 관련 파이썬 모듈을 설치해야 한다.

- python-curses: Python이 (N)Curses Libr에 대한 인터페이스이다

```terminal
sudo zypper in python-curses python3-curses
```

#### matplotlib

matplot 을 설치하면 numpy, scipy, pillow python3-tk 등이 함께 설치된다.

```
sudo zypper in python-matplotlib python3-matplotlib
...

다음 새 패키지가 설치됩니다.
  cups cups-client cups-filters ghostscript ghostscript-x11 libICE6 libSM6 libXt6 libavahi-glib1
  libcupscgi1 libcupsimage2 libcupsmime1 libcupsppdc1 libopenjp2-7 libpoppler73 libqhull7-7_2_0 libqpdf21
  libwebp6 libwebpdemux2 libwebpmux2 libxml2-tools parallel-printer-support poppler-data poppler-tools
  python-functools32 python-subprocess32 python-tk python2-Cycler python2-Pillow python2-matplotlib
  python2-matplotlib-tk python2-numpy python2-olefile python2-pyparsing python2-python-dateutil
  python2-pytz python2-six python3-Cycler python3-Pillow python3-matplotlib python3-matplotlib-tk
  python3-olefile python3-tk

다음 권장 패키지가 자동으로 선택되었습니다.
  cups-filters ghostscript ghostscript-x11 libxml2-tools poppler-data poppler-tools python2-Pillow
  python2-matplotlib-tk python3-Pi
```

numpy 개발자 패키지를 설치한다.

```
sudo zypper in python2-numpy-devel python3-numpy-devel
```

<br>

## Jupyter Notebook

Jupyter는 웹 브라우저를 통해서 IDE 환경을 제공하고, 다양한 언어/문법을 지원하는 Kernel 이라는 해석기를 통해 IDE에서 코딩한 결과를 확인할 수 있다.

여기서는 가상환경을 구성해 Jupyter 관련 패키지를 설치하고 사용하겠다.

- 시스템에 설치된 Python3 와 pip 모듈
- 가상환경으로 venv 사용

### 설치

Zeromq-devel 설치한다.

- libczmq5은 쥬피터 노트북에서 필요로 한다.
- LEAP 14는 libzmq3 다.

```terminal
sudo zypper install zeromq-devel libzmq5
```

jupyter notebook 을 시스템 패키지로 설치한다.

```terminal
sudo zypper in python3-jupyter_notebook
...
다음 새 패키지가 설치됩니다.
  ethtool libcmark0_28_3 libgfortran4 libibverbs libibverbs1 liblua5_1-5 libmlx4-1 libmlx5-1 libnuma1
  libopenblas_openmp0 librdmacm1 libucm0 libucp0 libucs0 libuct0 libxslt1 mpi-selector openmpi
  openmpi-config openmpi-libs pandoc python-sip-common python3-Automat python3-Babel python3-Genshi
  python3-Jinja2 python3-MarkupSafe python3-PyNaCl python3-Pygments python3-Send2Trash python3-Twisted
  python3-asn1crypto python3-attrs python3-backcall python3-bcrypt python3-bleach python3-certifi
  python3-cffi python3-constantly python3-cryptography python3-cssselect python3-defusedxml
  python3-entrypoints python3-gevent python3-greenlet python3-html5lib python3-hyperlink python3-idna
  python3-incremental python3-ipython_genutils python3-jedi python3-jsonschema python3-jupyter_client
  python3-jupyter_core python3-jupyter_ipykernel python3-jupyter_ipyparallel python3-jupyter_ipython
  python3-jupyter_ipywidgets python3-jupyter_nbconvert python3-jupyter_nbformat python3-jupyter_notebook
  python3-jupyter_qtconsole python3-jupyter_widgetsnbextension python3-lxml python3-mistune python3-mpi4py
  python3-numpy python3-pandocfilters python3-paramiko python3-parso python3-pexpect python3-pickleshare
  python3-prometheus_client python3-prompt_toolkit python3-psutil python3-ptyprocess python3-py
  python3-pyOpenSSL python3-pyasn1 python3-pyasn1-modules python3-pycparser python3-pycrypto
  python3-pycurl python3-pymongo python3-pyserial python3-python-dateutil python3-pytz python3-pyzmq
  python3-service_identity python3-simplegeneric python3-simplejson python3-sip python3-terminado
  python3-testpath python3-tornado python3-traitlets python3-wcwidth python3-webencodings
  python3-zope.interface rdma-core

다음 권장 패키지가 자동으로 선택되었습니다.
  ethtool openmpi-config pandoc python3-Genshi python3-Pygments python3-Twisted python3-cffi
  python3-gevent python3-jupyter_client python3-jupyter_ipykernel python3-jupyter_ipyparallel
  python3-jupyter_ipython python3-jupyter_ipywidgets python3-jupyter_nbconvert python3-jupyter_nbformat
  python3-jupyter_notebook python3-jupyter_qtconsole python3-lxml python3-mpi4py python3-numpy
  python3-paramiko python3-pexpect python3-psutil python3-py python3-pycurl python3-pymongo
  python3-service_identity python3-simplejson python3-tornado

다음 패키지가 제안되지만 설치되지 않습니다.
  python3-jupyter_notebook-latex
```

<br>

### 가상환경 구성

jupyter notebook 을 실행하고 사용할 파이썬 과학계산 모듈을 위한 가상환경을 구성한다.

시스템 `pip` 모듈을 최신 버전으로 업그레이드한다.

```sh
sudo pip install --upgrade pip
```

가상환경은 사용자 홈디렉토리의 _.venv_ 폴더로 가정한다. _python3_, 파이썬 시스템 라이브러리를 함께 사용을 위해 _--system-site-packages_ 옵션과 함께 *jupyterscfy*라는 가상환경을 생성한다.

```sh
~> python3 -m venv --system-site-packages .venv/jupyterscfy
~> source .venv/jupyterscfy/bin/activate
(jupyterscfy) ~>
(jupyterscfy) ~> python --version
Python 3.6
```

가상환경의 pip 모듈을 최신버전으로 유지한다.

```sh
(jupyterscfy) ~> pip install --upgrade pip
```

<br>

### 실행

`jupyter-notebook`을 실행해 보고, 서버 관련 구성 내용을 설정해서 사용해 보자.

```sh
(jupyterscfy) ~> jupyter notebook
[I 01:15:02.799 NotebookApp] Writing notebook server cookie secret to /run/user/1000/jupyter/notebook_cookie_secret
[I 01:15:21.630 NotebookApp] Loading IPython parallel extension
[I 01:15:21.646 NotebookApp] Serving notebooks from local directory: /home/qkboo
[I 01:15:21.646 NotebookApp] The Jupyter Notebook is running at:
[I 01:15:21.647 NotebookApp] http://localhost:8888/?token=689ae860106f8a7274b50f68f69895420eab19064ebb8e69
[I 01:15:21.648 NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
[W 01:15:21.708 NotebookApp] No web browser found: could not locate runnable browser.
[C 01:15:21.710 NotebookApp]

    To access the notebook, open this file in a browser:
        file:///run/user/1000/jupyter/nbserver-8251-open.html
    Or copy and paste one of these URLs:
        http://localhost:8888/?token=689ae860106f8a7274b50f68f69895420eab19064ebb8e69

```

jupyter notebook 을 종료하려면 Ctrl+C 로 인터럽트를 걸어준다.

```sh
...

^C[I 01:16:41.847 NotebookApp] interrupted
Serving notebooks from local directory: /home/qkboo
0 active kernels
The Jupyter Notebook is running at:
http://localhost:8888/?token=689ae860106f8a7274b50f68f69895420eab19064ebb8e69
Shutdown this notebook server (y/[n])? y
[C 01:16:44.757 NotebookApp] Shutdown confirmed
[I 01:16:44.760 NotebookApp] Shutting down 0 kernels
(jupyterscfy) ~>
```

<br>

### Scientific packages

numpy
scipy
matplotlib
pillow

- matplotlib는 외부 이미지를 png 형식만 지원한다. jpeg를 사용하려면 pillow 모듈을 설치한다.

pillow 여기까지 설치하면 의존성에 관련한 아래 같은 여러 패키지가 함께 설치된다.

- requests : Python package provides a graceful interface for making HTTP requests,
- pil : provides Python imaging capabilities,
- scrapy : is a web scraping framework,
- geopy provides geocoding and geodesic distance functions,
- shapely provides 2D geometry manipulation,
- pyproj: provides cartographic transformations. In the second command, jupyter provides interactive coding notebooks,
- geopandas spatializes pandas,
- OSMnx lets you work with OpenStreetMap street networks.

이 패키지들은 별도로 설치를 하고자 하면 `pip`로 설치하거나 시스템 패키지로 다음 같이 설치 할 수 있다.

Python2

```sh
sudo apt-get install python-requests python-pil python-scrapy python-geopy python-shapely python-pyproj
```

Python3

```sh
sudo apt-get install python3-requests python3-pil python3-scrapy python3-geopy python3-shapely python3-pyproj
```

#### symbolic math

symbolic mathematics 관련 패키지도 설치한다.

```terminal
sudo zypper install python-sympy python-nose
sudo zypper install python3-sympy python3-nose
```

#### Pandas 설치

pandas 설치시 여러 번들이 필요해서 시스템 패키지가 아닌 conda 혹은 pip 로 설치한다.

```sh
$ time pip install pandas
...

real 118m4.118s
user 114m47.176s
sys 1m27.483s
```

> pandas를 pip로 설치시 Raspberry Pi 3의 OpenSuse 15.2 에서 실제 2시간 정도 걸린다.

Pandas 로 지오 데이터 셋트를 다룰 예정이라면

#### 백업

현재까지 설치된 pip 모듈 목록을 저장하자.

```terminal
~> pip freeze --local > jupyter-requirements.txt
```

<br>

## 서버로 실행

`jupyter-notebook` 은 로컬 머신에서 브라우저를 실행한다. 여기서는 원격 사용을 위해 다음 같이 시작할 수 있다. `[DIR]` 에 jupyter-notebook에서 작성한 노트북 파일이 저장된다.

```
~> jupyter-notebook --no-browser --ip=* --port=8000 [DIR]
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
~> jupyter notebook --generate-config
Writing default config to: /home/foo/.jupyter/jupyter_notebook_config.py
~> cd .jupyter && mv jupyter_notebook_config.py mynotebook.py
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
~> jupyter-notebook --config .jupyter/mybook_config.py [DIR]
```

이제 jupyter에 접속하려면 다음 같이 패스워드를 묻고 입력한 후에 사용할 수 있다.

![](/images/python/jupyter-login.png){:width="650"}

#### Background 실행

Jupyter-notebook을 백그라운드로 실행하려면 `nohup` 을 사용할 수 있다.

```sh
~> nohup jupyter notebook --no-browser --ip=* --port=8000 [DIR] > log-jupyter.log &
```

nohup은 재시동시 다시 시작해야 하는 불편한 점이 있다.

#### crontab 으로 실행

nohup 과 결합해서 사용해도 좋은 방법으로 crontab 을 사용해 재시동 후 자동으로 시작해 줄 수 있다. 사용자 crontab 을 편집해 다음 같이 사용할 수 있다.

```
@reboot cd /home/foo; jnohup jupyter notebook --no-browser --ip=* --port=8000 [DIR] > log-jupyter.log &
```

이 방법도 사용자가 시작/상태확인/종료 같은 직접적인 방법을 사용하기는 좀 불편하다.

그래서 upstart의 init.d 를 사용하거나 systemd 의 unit으로 생성해서 사용하는 것이 좋다.

### systemd 에서 실행

systemd unit으로 새로운 unit 파일을 생성해서 기존 `systemctl` 명령으로 시작/상태/종료/재시작 등의 작업을 할 수 있다. 물론 재시동 관련해서도 완벽히 동작한다.

systemd의 unit 위치는 OS 마다 조금 다른 것 같다. 여기서는 _/etc/systemd/system_ 밑에 _jupyter.service_ 라는 유닉 파일로 직접 작업한다.

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

```terminal
systemctl enable jupyter.service
systemctl daemon-reload
systemctl restart jupyter.service
```

### Upgrade jupyter

다음 같이 install 명령으로 업그레이드 할 수 있다.

```terminal
(jupyter)~> pip install -U jupyter
```

## 참조

- [Pandas Install](http://pandas.pydata.org/pandas-docs/stable/install.html)
- [Scipy Install](https://www.scipy.org/install.html)
- [Scientific Python on Raspberry Pi](http://geoffboeing.com/2016/03/scientific-python-raspberry-pi/)
