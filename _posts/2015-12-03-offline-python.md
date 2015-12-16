---
layout: post
title: Install Python: no sudo, no internet
---


# Install Python no sudo no internet

## Versions

This was tested using the following versions

 * Red Hat Enterprise Linux Server release 7.1
 * Python 2.7.10
 * zlib 1.2.8
 * openssl 1.0.0s
 * setuptools 18.6.1
 * pip 7.1.2

## Downlaod Packages

 * [Python](https://www.python.org/downloads/source/)
 * [zlib](http://www.zlib.net/)
 * [openssl](https://www.openssl.org/source/)
 * [setuptools](https://pypi.python.org/pypi/setuptools#code-of-conduct)
 * [pip](https://pypi.python.org/pypi/pip#downloads)

Transfer the software to the offline server

```bash
cd Downloads
scp * user@offline-server
```

## Setup the offline server

 * Update the path
 * Create the directory structure

```bash
echo "export PATH=$HOME/.local/bin:\$PATH" >> $HOME/.bashrc
export PATH=$HOME/.local/bin:\$PATH

mkdir $HOME/.local
mkdir -p $HOME/packages/python
mkdir -p $HOME/.config/pip/
```

## Compile software

### openssl

```bash
cd
tar zxf openssl-1.0.0s.tar.gz
cd openssl-1.0.0s
./config --prefix=$HOME/.local --openssldir=$HOME/.local/openssl shared && make && make install

echo "export LD_LIBRARY_PATH=$HOME/.local/lib:\$LD_LIBRARY_PATH" >> $HOME/.bashrc
export LD_LIBRARY_PATH=$HOME/.local/lib:$LD_LIBRARY_PATH

```

### zlib

```bash
cd
tar zxf zlib-1.2.8.tar.gz
cd zlib-1.2.8
./configure --prefix=$HOME/.local && make && make install
```

### Python

```bash
cd
tar zxf Python-2.7.10.tgz
cd Python-2.7.10
./configure --prefix=$HOME/.local && make && make install
```

### Python setuptools


```bash
cd
tar zxf setuptools-18.6.1.tar.gz
cd setuptools-18.6.1
python setup.py install
```

### Python pip

```bash
easy_install pip-7.1.2.tar.gz
```

## Pip Setup

### Distutils

This is used so pip can resolve and install dependencies when a server does not have internet access

```bash
vim $HOME/.config/.pydistutils.cfg
```

```cfg
[easy_install]
allow_hosts = ''
find_links = ~/packages/python
```

### Pip

```bash
vim $HOME/.config/pip/pip.conf
```

```cfg
[install]
no-index = true
wheel-dir = ~/packages/python
find-links = ~/packages/python
```

## Aquire Packages

On a PC with internet access and a working python + pip, download the packages and their dependencies

```bash
pip install -d . package
```

Transfer to server

```bash
scp package username@offline-server:/home/username/packages/python
```

You can now install the packages by name

```bash
ssh username@offline-server
pip install package
```
