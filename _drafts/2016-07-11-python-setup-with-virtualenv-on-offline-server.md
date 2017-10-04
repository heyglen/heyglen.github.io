---
layout: post
title: Python setup with VirtualEnv on an offline server
---

# Test Setup

## Target computer without internet access
  * Redhat 6
  * Packages

```bash
yum install -y python-pip python-ipython.noarch python-distutils-extra.noarch python-devel.x86_64 openssl-devel.x86_64 python-virtualenvwrapper.noarch git.x86_64 sqlite-devel.x86_64 cdk.x86_64 ncurses-devel.x86_64 bzip2-devel.x86_64 readline-devel.x86_64
```
 > Assumes python packages will be stored `/usr/local/packages/python`. You can change this to whatever you have permissions to

## Computer with internet access
  * pip
  * wget (Can use a web browser instead)
  * scp

# Downlaod the Packages

On a machine with internet, download the packages and transfer them to `offline-server`

```bash
mkdir -p $HOME/Downloads/package
wget http://www.python.org/ftp/python/2.7.12/Python-2.7.12.tgz --directory-prefix=$HOME/Downloads/package
pip download --no-binary :all: -d $HOME/Downloads/package setuptools setuptools_scm pip
pip download --no-binary :all: -d $HOME/Downloads/package package
scp $HOME/Downloads/package/* user@offline-server
rm -fr $HOME/Downloads/package
```

On `offline-server` setup the [pip configuration file](https://pip.pypa.io/en/stable/user_guide/#config-file) on the offline server

File to edit

```bash
# If you have root access
vim /etc/pip.conf

# If not
vim $HOME/.config/pip/pip.conf
```

File Contents

```conf
[install]
no-index = true
wheel-dir = /usr/local/packages/python
find-links = /usr/local/packages/python
```

Setup [distutils configuration file](http://pydoc-zh.readthedocs.io/en/latest/install/index.html#distutils-configuration-files) on the offline server

This is used by pip when installing a package's dependencies

File to edit

```bash
# If you have root access
vim /usr/local/lib/python2.7/distutils/distutils.cfg

# If not
vim $HOME/.config/.pydistutils.cfg
```

File contents

```config
[easy_install]
allow_hosts = ''
find_links = /usr/local/packages/python
```

Setup Virtualenvwrapper

```bash
export WORKON_HOME=~/.virtual_environments
echo "export WORKON_HOME=~/.virtual_environments" >> $HOME/.bashrc
mkdir -p $WORKON_HOME
echo "source virtualenvwrapper.sh" >> $HOME/.bashrc
```

Configure virtualenvwrapper to build and install python every time a new environment is made

File to edit

```bash
vim $WORKON_HOME/postmkvirtualenv
```

File contents

 > Assumes install of Python 2.7.12

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
cp $py_packages_dir/setuptools-*.tar.gz $VIRTUAL_ENV/src
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

Setup the offine package folder

```bash
# Create the directory
mkdir -p /usr/local/packages/python

# Move all the package files to it
mv $HOME/*.tar.gz /usr/local/packages/python
```


Setup the environment

```bash
mkvirtualenv package
```

Install packages

```bash
pip install package
```
