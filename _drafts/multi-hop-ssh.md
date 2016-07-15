---
layout: post
title: Multi Hop OpenSSH
---

![SSH Multihop](/public/img/multihop-ssh/hosts.jpg){:class="img-responsive"}


How do I ssh or scp to host04 without first logging into hosts02-03?

Add host entires to your OpenSSH configuration file that use proxycommand with netcat (nc) to:

 * Proxy to `host03` via `host02` 
 * Proxy to `host04` via `host03` 


```
[host01]# vim $HOME/.ssh/config
```


```conf
host host03
    proxycommand ssh host02 nc %h %p
host host04
    proxycommand ssh host03 nc %h %p
```

Setup ssh keys for automatic login.

```
[host01]# ls $HOME/.ssh/id_rsa 2> /dev/null || ssh-keygen -t rsa
[host01]# ssh-copy-id $USER@host02
[host01]# ssh-copy-id $USER@host03
[host01]# ssh-copy-id $USER@host04
```

# Test

```shell
[host01]# ssh host04
[host04]#
```

:sunglasses:
