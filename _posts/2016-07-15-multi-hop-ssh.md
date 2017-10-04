---
layout: post
title: Multi Hop SSH
---

You need to ssh through jumphosts `host02` and `host03` to get to `host04` .

![SSH Multihop](/public/img/multihop-ssh/hosts.jpg){:class="img-responsive"}


Edit your OpenSSH configuration file

```
[host01]# vim $HOME/.ssh/config
```

...with the following

```conf
host host03
    proxycommand ssh -q -AW %h:%p host02
host host04
    proxycommand ssh -q -AW %h:%p host03
```

Setup ssh keys

```
[host01]# ls $HOME/.ssh/id_rsa 2> /dev/null || ssh-keygen -t rsa
```

Copy the public key to the servers for automatic login

```
[host01]# ssh-copy-id $USER@host02
[host01]# ssh-copy-id $USER@host03
[host01]# ssh-copy-id $USER@host04
```

Test

```
[host01]# ssh host04
[host04]#
```

:sunglasses:
