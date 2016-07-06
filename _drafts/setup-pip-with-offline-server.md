---
layout: post
title: Setup PIP installation on an offline server
---

# Test Setup

## Internet Connected Computer
 * python
 * pip
 * scp
 * wget

## Offline Computer
 * python
 * pip

# Downlaod the Packages

On a machine with internet, download the packages and transfer them to `offline-server`

```bash
mkdir -p $USER/Downloads/package
wget http://www.python.org/ftp/python/2.7.9/Python-2.7.9.tgz --directory-prefix=$USER/Downloads/package
pip download --no-binary :all: -d $USER/Downloads/package package
scp $USER/Downloads/package/* user@offline-server
rm -fr $USER/Downloads/package
```

On `offline-server`

```bash
vim /etc/pip.conf
```

```conf
[install]
no-index = true
wheel-dir = /usr/local/packages/python
find-links = /usr/local/packages/python
```


```bash
pip install virtualenvwrapper

export WORKON_HOME=~/.virtual_environments
echo "export WORKON_HOME=~/.virtual_environments" >> $HOME/.bashrc
mkdir -p $WORKON_HOME
echo "source virtualenvwrapper.sh" >> $HOME/.bashrc

vim $WORKON_HOME/postmkvirtualenv
```

Customize environment creation
 * Install Python 2.7.12

```bash
pyver=Python-2.7.12
pybinary=$pyver.tgz
py_packages_dir=/usr/local/packages/python/


# Python
mkdir -p $VIRTUAL_ENV/src
tar zxf $py_packages_dir$pybinary -C $VIRTUAL_ENV/src
rm $py_packages_dir$pybinary -C $VIRTUAL_ENV/src
cd $VIRTUAL_ENV/src/$pyver
./configure --prefix=$VIRTUAL_ENV
make
make install

# SetupTools
cd $VIRTUAL_ENV/src
cp $py_packages_dir/setuptools*.tar.gz .
tar zxf setuptools-*
rm setuptools-*.gz
cd setuptools-*
python setup.py install


# Pip
mkdir -p $HOME/.pip
cp /etc/pip.conf $HOME/.pip/pip.conf

cd $VIRTUAL_ENV/src
cp $py_packages_dir/pip*.tar.gz .
easy_install pip*

cd $VIRTUAL_ENV
rm -fr src
```

Build the environment, install the requirements

```bash
mkvirtualenv package
pip install package
```
