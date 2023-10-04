# Network Bondig & VLAN Tagging

| interface name | Bond|
| ----------- | ----------- |
| eno5| bond0 |
| eno6 | bond1|
| eno7 | bond1 |
| eno8 | bond0 |

| VLAN  | Bond|
| ----------- | ----------- |
| bond0.2026| bond0 |
| bond0.2028 | bond0|
| bond1.2027 | bond1 |

## Bonding and tagging Information

```
eno5 + eno8 --> bond0
eno6 + eno7 --> bond1

bond0 --> bond0.2026
bond0 --> bond0.2028
bond1 --> bond1.2027
```

> **Here I am  going to configure Network Bonding with 2 Inerfaces.**
> **In my lab-server I have total 4 Interfaces and will create 2 Bonding.(bond0, bond1)**
> **And again will do 3 vlan tagging to the bonds which I have created.**

## Follow the steps

1. Create 2 Bonds

```
nmcli connection add type bond ifname bond0 con-name bond0 bond.options "mode=active-backup"
nmcli connection add type bond ifname bond1 con-name bond1 bond.options "mode=active-backup"

nmcli connection modify bond0 ipv4.method disabled ipv6.method ignore
nmcli connection modify bond1 ipv4.method disabled ipv6.method ignore
```

*If You are doing only bonding, not need to disable and ignore ip method.*

2. Add slave to the bonds

```
nmcli connection add type bond-slave ifname eno5 con-name en05 master bond0
nmcli connection add type bond-slave ifname eno8 con-name eno8 master bond0

nmcli connection add type bond-slave ifname eno7 con-name eno7 master bond1
nmcli connection add type bond-slave ifname eno7 con-name eno7 master bond1

nmcli connection up bond0
nmcli connection up bond1
```

3. Create Vlan on top of bonds and assign the IPs.

```
nmcli connection add type vlan ifname bond0.2026 con-name bond0.2026 id 10 dev bond0 ip4 10.20.30.10/24 gw4 10.20.30.1 ipv4.method manual

nmcli connection add type vlan ifname bond0.2028 con-name bond0.2028 id 10 dev bond0 ip4 10.20.30.20/24 gw4 10.20.30.1 ipv4.method manual

nmcli connection add type vlan ifname bond1.2027 con-name bond1.2027 id 10 dev bond1 ip4 10.20.30.30/24 gw4 10.20.30.1 ipv4.method manual

nmcli connection modify bond0.10 ipv4.dns 8.8.8.8 (optional)
```

*2 vlans might not be created uing nmcli on the same bond. So you can use nmtui if the 2nd vlan didn't created. Here we are creating bond0.2026 and bond0.2028 on bond0. I have used nmtui for creting the bond0.2028*

4. Restart the NetworkManager Service.

```
systemctl restart NetworkManager
```
