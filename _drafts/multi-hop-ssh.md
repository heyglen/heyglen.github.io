---
layout: post
title: Multi Hop SSH
---

# Problem


```shell
[host01]# ssh host02
[host02]# ssh host03
[host03]# ssh host04
[host04]# do_work

```

:smiley::gun:

# Solution

Edit your SSH Config

```shell
[host01]# vim $HOME/.ssh/config
```

 * Proxy to *host03* through *host02*

 * Proxy to *host04* through *host03*

```conf
host host03
	proxycommand ssh host02 nc %h %p
host host04
	proxycommand ssh host03 nc %h %p
```

  > :memo: You can also use [ncat](https://nmap.org/ncat/) if you are using windows

# Test

```shell
[host01]# ssh host04
[host04]# do_work
```

:sunglasses:
