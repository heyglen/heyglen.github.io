---
layout: post
title: Multi Hop SSH
---

I need to ssh through jumphosts `host02` and `host03` to get to `host04` .

![SSH Multihop](/public/img/multihop-ssh/hosts.jpg){:class="img-responsive"}


Edit your OpenSSH configuration file

```
[host01]# vim $HOME/.ssh/config
```

...with the following

```conf
host host03
    proxycommand ssh -q -W %h:%h host02
host host04
    proxycommand ssh -q -W %h:%h host03
```

Setup ssh keys and copy the public key to the servers for automatic login

```
[host01]# ls $HOME/.ssh/id_rsa 2> /dev/null || ssh-keygen -t rsa
[host01]# ssh-copy-id $USER@host02
[host01]# ssh-copy-id $USER@host03
[host01]# ssh-copy-id $USER@host04
```

Test

```
[host01]# ssh host04 hostname
host04
```

:sunglasses:
