---
layout: post
title: Cisco FabricPath Path Tracing
---

![Fabric Path Logical Network Diagram](/public/img/fabric-path-logical.jpg){:class="img-responsive"}

What is the egress physical interface for 3.3.3.3?

# Layer 3

3.3.3.3 next hop?

```
n6k01# show ip route 3.3.3.3
...
3.3.3.3/32, ubest/mbest: 1/0, attached
    *via 3.3.3.3, Vlan123, [1/0], 1w2d, am
```

✔ SVI Vlan123

# Layer 2

3.3.3.3 mac address?

```
n6k01# show ip arp | i 3.3.3.3
3.3.3.3  00:01:23  3333.abcd.3333  Vlan123
```

✔ 3333.abcd.3333

3333.abcd.3333 egress physical interface?

```
n6k01# show mac address-table address 3333.abcd.3333 vlan 123
...
   VLAN     MAC Address      Type      age     Secure NTFY   Ports/SWID.SSID.LID
---------+-----------------+--------+---------+------+----+------------------
* 123       3333.abcd.3333    static    0          F    F  456.0.0
```

:memo: 456 is the FabricPath switch id

✔ 456.0.0


# FarbicPath: Layer 2 via a layer 3 routing protocol

Egress physical interface to switch id 456?

```
n6k01# show fabricpath route switchid 456
...
1/456/0, number of next-hops: 1
        via Eth1/2, [115/15], 42 day/s 01:00:00, isis_fabricpath-default
```

✔ Eth1/2

Neighboring switch on interface Eth1/2?

```
n6k01# show cdp ne interface Eth1/2 detail
----------------------------------------
Device ID:n6k02(FOC1456A12B)
System Name:n6k02
...
Mgmt address(es):
    IPv4 Address: 2.2.2.2

```

✔ n6k02 

Connect to n6k02 on 2.2.2.2

:checkered_flag:
