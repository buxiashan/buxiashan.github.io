---
layout: post
title: Cisco Nexus VXLAN/EVPN F5+ASA综合实验
category: Labs
tags: VXLAN,EVPN,Nexus,F5,ASA,综合实验
description: 综合Lab
#published: true    #如果不想发布某一篇文章，你可以设置此项为false.
---
# EVE-NG版本的拓扑图
   备注：本博文只关注VXLAN,EVPN,F5部分实验，lab较大，其余部分暂时不在该博文内描述  
   ![EVE-topo](/assets/img/vxlan,evpn_lab_EVEtopo.png)

   [EVE-NG .unl拓扑文件备份](/assets\attached_files/DEMO.unl,"EVE-NG .unl文件")

# VISIO版本的拓扑图(VXLAN,EVPN,F5部分)
![visio-topo](/assets/img/vxlan,evpn_lab_topo.jpg)
##

#### 本lab用到的EVE-NG images：

DevieType       | iamgeName
--------        | ---
NEXUS image     |  nxosv9k-7.0.3.I7.3
F5 iamge        |  bigip-12.12
IOL L2 iamge    |  i86bi_linux_l2-ipbasek9-ms.high_iron_aug9_2017b.bin
IOL L3 iamge    |  L3-ADVENTERPRISEK9-M-15.4-2T.bin
ASA iamge       |  asav-952-204


# VXLAN部分配置

1. 启用Feature  

```  
feature nv overlay
feature vn-segment-vlan-based
feature bgp
nv overlay evpn
feature ospf
feature lldp
feature pim
feature interface-vlan
feature lldp
```  

备注： spine设备leaf上都配置


2. 初始化接口配置和底层OSPF配置

**DMDC-N9K-Leaf1:**
```
router ospf UNDERLAY
  router-id 10.30.13.249
exit

interface Ethernet1/4
  no switchport
  ip address 10.30.13.157/30
  ip router ospf UNDERLAY area 0.0.0.0
  description "Conn TO Spine1"
  no shutdown
interface Ethernet1/5
  no switchport
  ip address 10.30.13.161/30
  ip router  ospf UNDERLAY area 0.0.0.0
   description "Conn TO Spine2"
  no shutdown
interface loopback0
  ip address 10.30.13.247/32
  ip router  ospf UNDERLAY area 0.0.0.0
  no shutdown
exit

```
**DMDC-N9K-Leaf2:**
```
router ospf UNDERLAY
  router-id 10.30.13.250
exit

interface Ethernet1/4
  no switchport
  ip address 10.30.13.165/30
  ip router ospf UNDERLAY area 0.0.0.0
  description "Conn TO Spine1"
  no shutdown
interface Ethernet1/5
  no switchport
  ip address 10.30.13.169/30
  ip router  ospf UNDERLAY area 0.0.0.0
   description "Conn TO Spine2"
  no shutdown
interface loopback0
  ip address 10.30.13.248/32
  ip router  ospf UNDERLAY area 0.0.0.0
  no shutdown
exit

```

**DMDC-N9K-Leaf3:**
```
router ospf UNDERLAY
  router-id 10.30.13.251
exit

interface Ethernet1/4
  no switchport
  ip address 10.30.13.141/30
  ip router ospf UNDERLAY area 0.0.0.0
  description "Conn TO Spine1"
  no shutdown
interface Ethernet1/5
  no switchport
  ip address 10.30.13.145/30
  ip router  ospf UNDERLAY area 0.0.0.0
   description "Conn TO Spine2"
  no shutdown
interface loopback0
  ip address 10.30.13.249/32
  ip router  ospf UNDERLAY area 0.0.0.0
  no shutdown
exit

```

**DMDC-N9K-Leaf4:**
```
router ospf UNDERLAY
  router-id 10.30.13.252
exit

interface Ethernet1/4
  no switchport
  ip address 10.30.13.149/30
  ip router ospf UNDERLAY area 0.0.0.0
  description "Conn TO Spine1"
  no shutdown
interface Ethernet1/5
  no switchport
  ip address 10.30.13.153/30
  ip router  ospf UNDERLAY area 0.0.0.0
   description "Conn TO Spine2"
  no shutdown
interface loopback0
  ip address 10.30.13.250/32
  ip router  ospf UNDERLAY area 0.0.0.0
  no shutdown
exit

```

**DMDC-N9K-Spine1:**
```
router ospf UNDERLAY
  router-id 10.30.13.253
exit

interface Ethernet1/3
  no switchport
  ip address 10.30.13.142/30
  ip router ospf UNDERLAY area 0.0.0.0
  description "Conn TO Leaf3"
  no shutdown
interface Ethernet1/4
  no switchport
  ip address 10.30.13.150/30
  ip router  ospf UNDERLAY area 0.0.0.0
   description "Conn TO Leaf4"
  no shutdown
interface Ethernet1/5
  no switchport
  ip address 10.30.13.158/30
  ip router ospf UNDERLAY area 0.0.0.0
  description "Conn TO Leaf1"
  no shutdown
interface Ethernet1/6
  no switchport
  ip address 10.30.13.166/30
  ip router  ospf UNDERLAY area 0.0.0.0
   description "Conn TO Leaf2"
  no shutdown
interface loopback0
  ip address 10.30.13.251/32
  ip router  ospf UNDERLAY area 0.0.0.0
  no shutdown
interface loopback1
  description "Rendezvous Point"
  ip address 10.30.13.252/32
  ip router  ospf UNDERLAY area 0.0.0.0
  no shutdown
exit

```

**DMDC-N9K-Spine2:**
```
router ospf UNDERLAY
  router-id 10.30.13.254
exit

interface Ethernet1/3
  no switchport
  ip address 10.30.13.146/30
  ip router ospf UNDERLAY area 0.0.0.0
  description "Conn TO Leaf3"
  no shutdown
interface Ethernet1/4
  no switchport
  ip address 10.30.13.154/30
  ip router  ospf UNDERLAY area 0.0.0.0
   description "Conn TO Leaf4"
  no shutdown
interface Ethernet1/5
  no switchport
  ip address 10.30.13.162/30
  ip router ospf UNDERLAY area 0.0.0.0
  description "Conn TO Leaf1"
  no shutdown
interface Ethernet1/6
  no switchport
  ip address 10.30.13.170/30
  ip router  ospf UNDERLAY area 0.0.0.0
   description "Conn TO Leaf2"
  no shutdown
interface loopback0
  ip address 10.30.13.253/32
  ip router  ospf UNDERLAY area 0.0.0.0
  no shutdown
interface loopback1
  description "Rendezvous Point"
  ip address 10.30.13.254/32
  ip router  ospf UNDERLAY area 0.0.0.0
  no shutdown
exit

```


3. 配置组播

**DMDC-N9K-Leaf1:**

```
ip pim rp-address 10.30.13.252 group-list 224.0.0.0/4
ip pim ssm range 232.0.0.0/8

interface Ethernet1/4
 ip pim sparse-mode
interface Ethernet1/5
 ip pim sparse-mode
interface loopback0
 ip pim sparse-mode
exit


```
**DMDC-N9K-Leaf2:**
```
ip pim rp-address 10.30.13.252 group-list 224.0.0.0/4
ip pim ssm range 232.0.0.0/8

interface Ethernet1/4
 ip pim sparse-mode
interface Ethernet1/5
 ip pim sparse-mode
interface loopback0
 ip pim sparse-mode
exit


```
**DMDC-N9K-Leaf3:**
```
ip pim rp-address 10.30.13.252 group-list 224.0.0.0/4
ip pim ssm range 232.0.0.0/8

interface Ethernet1/4
 ip pim sparse-mode
interface Ethernet1/5
 ip pim sparse-mode
interface loopback0
 ip pim sparse-mode
exit


```
**DMDC-N9K-Leaf4:**
```
ip pim rp-address 10.30.13.252 group-list 224.0.0.0/4
ip pim ssm range 232.0.0.0/8

interface Ethernet1/4
 ip pim sparse-mode
interface Ethernet1/5
 ip pim sparse-mode
interface loopback0
 ip pim sparse-mode
exit


```
**DMDC-N9K-Spine1:**
```
ip pim rp-address 10.30.13.252 group-list 224.0.0.0/4
ip pim ssm range 232.0.0.0/8
interface Ethernet1/3
 ip pim sparse-mode
interface Ethernet1/4
 ip pim sparse-mode
interface Ethernet1/5
 ip pim sparse-mode
interface Ethernet1/6
 ip pim sparse-mode
interface loopback0
 ip pim sparse-mode
interface loopback1
 ip pim sparse-mode
exit

```
**DMDC-N9K-Spine2:**
```
ip pim ssm range 232.0.0.0/8
interface Ethernet1/3
 ip pim sparse-mode
interface Ethernet1/4
 ip pim sparse-mode
interface Ethernet1/5
 ip pim sparse-mode
interface Ethernet1/6
 ip pim sparse-mode
interface loopback0
 ip pim sparse-mode
interface loopback1
 ip pim sparse-mode
exit

```
4. 配置MP-BGP

**DMDC-N9K-Leaf1:**
```
router bgp 65535
  router-id 10.30.13.247
  neighbor 10.30.13.251
    remote-as 65535
    update-source lo 0
    address-family l2vpn evpn
        send-community both
  neighbor 10.30.13.253
    remote-as 65535
    update-source lo 0
    address-family l2vpn evpn
        send-community both
    exit
  exit
exit


```
**DMDC-N9K-Leaf2:**
```
router bgp 65535
  router-id 10.30.13.248
  neighbor 10.30.13.251
    remote-as 65535
    update-source lo 0
    address-family l2vpn evpn
        send-community both
  neighbor 10.30.13.253
    remote-as 65535
    update-source lo 0
    address-family l2vpn evpn
        send-community both
    exit
  exit
exit

```
**DMDC-N9K-Leaf3:**
```
router bgp 65535
  router-id 10.30.13.249
  neighbor 10.30.13.251
    remote-as 65535
    update-source lo 0
    address-family l2vpn evpn
        send-community both
  neighbor 10.30.13.253
    remote-as 65535
    update-source lo 0
    address-family l2vpn evpn
        send-community both
    exit
  exit
exit



```
**DMDC-N9K-Leaf4:**
```
router bgp 65535
  router-id 10.30.13.250
  neighbor 10.30.13.251
    remote-as 65535
    update-source lo 0
    address-family l2vpn evpn
        send-community both
  neighbor 10.30.13.253
    remote-as 65535
    update-source lo 0
    address-family l2vpn evpn
        send-community both
    exit
  exit
exit



```
**DMDC-N9K-Spine1:**
```
router bgp 65535
  router-id 10.30.13.251
 neighbor  10.30.13.247
     remote-as 65535
     update-source loopback0
     address-family l2vpn evpn
        send-community both
        route-reflector-client
 neighbor  10.30.13.248
     remote-as 65535
     update-source loopback0
     address-family l2vpn evpn
        send-community both
        route-reflector-client
 neighbor  10.30.13.249
     remote-as 65535
     update-source loopback0
     address-family l2vpn evpn
        send-community both
        route-reflector-client
 neighbor  10.30.13.250
     remote-as 65535
     update-source loopback0
     address-family l2vpn evpn
        send-community both
        route-reflector-client
    exit
  exit
exit


```

**DMDC-N9K-Spine2:**


```
router bgp 65535
  router-id 10.30.13.251
 neighbor  10.30.13.247
     remote-as 65535
     update-source loopback0
     address-family l2vpn evpn
        send-community both
        route-reflector-client
 neighbor  10.30.13.248
     remote-as 65535
     update-source loopback0
     address-family l2vpn evpn
        send-community both
        route-reflector-client
 neighbor  10.30.13.249
     remote-as 65535
     update-source loopback0
     address-family l2vpn evpn
        send-community both
        route-reflector-client
 neighbor  10.30.13.250
     remote-as 65535
     update-source loopback0
     address-family l2vpn evpn
        send-community both
        route-reflector-client
    exit
  exit
exit

```
5. 配置VXLAN EVPN

**DMDC-N9K-Leaf1/2/3/4:**
```
fabric forwarding anycast-gateway-mac 0000.2222.3333

vlan 101
 name L3-VNI-VLAN-101
 vn-segment 900001
vlan 1001
 vn-segment 2001001
vlan 1002
 vn-segment 2001002
vlan 102
 name L3-VNI-VLAN-102
 vn-segment 900002
vlan 1003
 vn-segment 2001003
vlan 103
 name L3-VNI-VLAN-103
 vn-segment 900003
vlan 1004
 vn-segment 2001004
exit

vrf context Tenant-1
 vni 900001
 rd auto
 address-family ipv4 unicast
   route-target both auto
   route-target both auto evpn
 address-family ipv6 unicast
   route-target both auto
   route-target both auto evpn

vrf context Tenant-2
 vni 900002
 rd auto
 address-family ipv4 unicast
   route-target both auto
   route-target both auto evpn
#Tenant-2不支持IPv6

vrf context Tenant-3
 vni 900003
 rd auto
 address-family ipv4 unicast
   route-target both auto
   route-target both auto evpn
#Tenant-2不支持IPv6

int vlan 101
 no shut
 vrf member Tenant-1
 ip forward
 no ip redirects
 ipv6 address use-link-local-only

int vlan 1001
 no shut
 vrf member Tenant-1
 descrip Conn-To-Servers&&F5
 ip address 10.30.15.28/28
 ipv6 address 1001::/64
 fabric forwarding mode anycast-gateway
int vlan 1002
 no shut
 descrip Reserved_For_Servers
 ip address 10.30.15.44/28
 vrf member Tenant-1
 ipv6 address 1002::/64
 fabric forwarding mode anycast-gateway


int vlan 102
 no shut
 vrf member Tenant-2
 ip forward
 no ip redirects

int vlan 1003
 no shut
 vrf member Tenant-2
 ip address 10.30.15.13/28
 fabric forwarding mode anycast-gateway
 description "FW-TrustZone-To-F5"
 exit
int vlan 103
 no shut
 vrf member Tenant-3
 ip forward
 no ip redirects
int vlan 1004
 no shut
 vrf member Tenant-3
 ip address 10.30.13.190/28
 fabric forwarding mode anycast-gateway
 description "Conn To FW-UntrustZone"
 exit
```

  备注：四台leaf设备DMDC-N9K-Leaf1/2/3/4配置相同

6. 接下来配置Nve接口

**DMDC-N9K-Leaf1/2/3/4:**
```
int nve1
 no shut
 host-reachability protocol bgp
 source-interface loopback0
 member vni 900001 associate-vrf
 member vni 900002 associate-vrf
 member vni 900003 associate-vrf

 member vni 2001001
 suppress-arp
 mcast-group 225.4.0.1
 member vni 2001002
 suppress-arp 
 mcast-group 225.4.0.100
 member vni 2001003
 suppress-arp
 mcast-group 225.4.0.2
 member vni 2001004
 suppress-arp 
 mcast-group 225.4.0.200
    exit
  exit

```
  备注：四台leaf设备DMDC-N9K-Leaf1/2/3/4配置相同

7. 下面配置把路由通告出去

```
router bgp 65535
 vrf Tenant-1
   address-family ipv4 unicast
     advertise l2vpn evpn
   address-family ipv6 unicast
     advertise l2vpn evpn
 evpn
   vni 2001001 l2
      rd auto
      route-target import auto
      route-target export auto
   vni 2001002 l2
      rd auto
      route-target import auto
      route-target export auto
    exit
  exit


router bgp 65535
 vrf Tenant-2
   address-family ipv4 unicast
     advertise l2vpn evpn
 evpn
   vni 2001003 l2
      rd auto
      route-target import auto
      route-target export auto

router bgp 65535
 vrf Tenant-3
   address-family ipv4 unicast
     advertise l2vpn evpn
 evpn
   vni 2001004 l2
      rd auto
      route-target import auto
      route-target export auto
    exit
  exit

```
  备注：四台leaf设备DMDC-N9K-Leaf1/2/3/4配置相同

###  配置L3外部路由：
    本实验里通过leaf1/leaf2分别连接两台iol设备，通过在leaf1/2与两台iol设备之间建立bgp来相互交换路由信息。

    **DMDC-N9K-Leaf1:**
    ```
    vlan 3000
      name To-External-Network

    int e1/6
     sw acc vlan 3000
     no shut
      exit


    interface Vlan3000
      description "To-External Network-Bgp-SVI"
      no shutdown
      vrf member Tenant-3
      ip address 10.30.13.34/29
      exit
    router bgp 65535
      vrf Tenant-3
        address-family ipv4 unicast
          network 10.30.13.32/30
          network 10.30.13.184/29
          network 10.30.15.0/27
          advertise l2vpn evpn
        neighbor 10.30.13.33
          remote-as 100
          update-source Vlan3000
          address-family ipv4 unicast
          exit



    ```
    **DMDC-N9K-Leaf2:**
    ```
    vlan 3001
      name To-External-Network

    int e1/6
     sw acc vlan 3001
     no shut
      exit


    interface Vlan3001
      description "To-External Network-Bgp-SVI"
      no shutdown
      vrf member Tenant-3
      ip address 10.30.13.38/29
      exit
    router bgp 65535
      vrf Tenant-3
        address-family ipv4 unicast
          network 10.30.13.36/30
          network 10.30.13.184/29
          network 10.30.15.0/27
          advertise l2vpn evpn
        neighbor 10.30.13.37
          remote-as 100
          update-source Vlan3001
          address-family ipv4 unicast
          exit

    ```
**DMHQ-6509-1#**

```
int e1/3
 no sw
 ip add 10.30.13.33 255.255.255.252
 Des Link-To-VXLAN-Leaf1
 no shut
 exit
 no router bgp 100
router bgp 100
bgp router-id 9.9.9.9
 neighbor 10.30.13.34 remote-as 65535

 address-family ipv4 unicast
   network 9.9.9.9 mask 255.255.255.255
   network 10.30.13.32 mask 255.255.255.252
   neighbor 10.30.13.34 activate
   redistribute ospf 100 match internal external
   exit-address-family


router ospf 100
  redistribute bgp 100 subnet metric 100
  exit

do wr


```
**DMHQ-6509-2#**
```
int e1/3
 no sw
 ip add 10.30.13.37 255.255.255.252
 Des Link-To-VXLAN-Leaf2
 no shut
 exit
 no router bgp 100
router bgp 100
bgp router-id 9.9.9.10
 neighbor 10.30.13.38 remote-as 65535

 address-family ipv4 unicast
   network 9.9.9.10 mask 255.255.255.255
   network 10.30.13.36 mask 255.255.255.252
   neighbor 10.30.13.38 activate
   redistribute ospf 100 match internal external
   exit-address-family


router ospf 100
  redistribute bgp 100 subnet metric 200
  exit

do wr


```

###  配置VXLAN外部设备学习F5 VS网段路由：

       完成上面的配置DMDQ-6509-1/2已经可以学习到ASA outside网段路由，但是外部设备iol还不能学习到F5 VS网段的路由，
为了让外部设备DMDQ-6509-1/2学习到F5 VS网段的路由，这里的配置思路是让leaf3/4跟ASA之间跑BGP，然后ASA上宣告inside网段即ASA宣告直连F5 VS的网段，这样VS网段路由会传递给leaf1/2，然后通过MPBGP传递给外部设备，从而实现了外部设备穿过VXLAN访问F5的目的：


**DMHQ-6509-3/4#**

```
router bgp 65535
  vrf Tenant-3
    address-family ipv4 unicast
      network 10.30.13.184/29   #宣告互联ASA网段
      advertise l2vpn evpn
    neighbor 10.30.13.185   #跟ASA建EBGP邻居
      remote-as 200
      update-source Vlan1004
      address-family ipv4 unicast

```
  备注：DMHQ-6509-3/4#配置相同

**DMHQ-ASA-1/2#配置**
```
DMDC-ASA-1# show run router
router bgp 200
 bgp log-neighbor-changes
 bgp router-id 10.30.13.185
 address-family ipv4 unicast
  neighbor 10.30.13.190 remote-as 65535
  neighbor 10.30.13.190 activate
  network 10.30.15.0 mask 255.255.255.240  #宣告ASA互联F5 external网段
  no auto-summary
  no synchronization
 exit-address-family
```

#  查看相关信息

1. 外部设备检查到F5 VS网段路由和连通性

```
DMHQ-6509-1#show ip route B
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override, p - overrides from PfR

Gateway of last resort is not set

      10.0.0.0/8 is variably subnetted, 46 subnets, 4 masks
B        10.30.13.185/32 [20/0] via 10.30.13.34, 2d21h
B        10.30.13.186/32 [20/0] via 10.30.13.34, 2d21h
B        10.30.15.0/28 [20/0] via 10.30.13.34, 2d21h
DMHQ-6509-1#
DMHQ-6509-1#ping 10.30.15.1
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.30.15.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 29/39/49 ms
DMHQ-6509-1#ping 10.30.15.2
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.30.15.2, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 36/45/50 ms
DMHQ-6509-1#ping 10.30.15.3
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.30.15.3, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 40/45/50 ms
DMHQ-6509-1#
```

2. 查看VXLAN相关状态

```
DMDC-N9K-Leaf3# show int nve 1
nve1 is up
admin state is up,  Hardware: NVE
  MTU 9216 bytes
  Encapsulation VXLAN
  Auto-mdix is turned off
  RX
    ucast: 0 pkts, 0 bytes - mcast: 0 pkts, 0 bytes
  TX
    ucast: 0 pkts, 0 bytes - mcast: 0 pkts, 0 bytes

DMDC-N9K-Leaf3#
DMDC-N9K-Leaf3# show mac address-table
Legend:
        * - primary entry, G - Gateway MAC, (R) - Routed MAC, O - Overlay MAC
        age - seconds since last seen,+ - primary entry using vPC Peer-Link,
        (T) - True, (F) - False, C - ControlPlane MAC, ~ - vsan
   VLAN     MAC Address      Type      age     Secure NTFY Ports
---------+-----------------+--------+---------+------+----+------------------
*  101     5000.000a.0007   static   -         F      F    Vlan101
*  102     5000.000a.0007   static   -         F      F    Vlan102
*  103     5000.000a.0007   static   -         F      F    Vlan103
G    -     0000.2222.3333   static   -         F      F    sup-eth1(R)
G    -     5000.000a.0007   static   -         F      F     (R)
G  101     5000.000a.0007   static   -         F      F    sup-eth1(R)
G  102     5000.000a.0007   static   -         F      F    sup-eth1(R)
G  103     5000.000a.0007   static   -         F      F    sup-eth1(R)
G 1001     5000.000a.0007   static   -         F      F    sup-eth1(R)
G 1002     5000.000a.0007   static   -         F      F    sup-eth1(R)
G 1003     5000.000a.0007   static   -         F      F    sup-eth1(R)
G 1004     5000.000a.0007   static   -         F      F    sup-eth1(R)
G    -     aabb.cc95.0123   static   -         F      F    sup-eth1(R) (Eth1/12)
DMDC-N9K-Leaf3#
DMDC-N9K-Leaf3# show nve vni
Codes: CP - Control Plane        DP - Data Plane          
       UC - Unconfigured         SA - Suppress ARP        
       SU - Suppress Unknown Unicast

Interface VNI      Multicast-group   State Mode Type [BD/VRF]      Flags
--------- -------- ----------------- ----- ---- ------------------ -----
nve1      900001   n/a               Up    CP   L3 [Tenant-1]             
nve1      900002   n/a               Up    CP   L3 [Tenant-2]             
nve1      900003   n/a               Up    CP   L3 [Tenant-3]             
nve1      2001001  225.4.0.1         Up    CP   L2 [1001]          SA      
nve1      2001002  225.4.0.100       Up    CP   L2 [1002]          SA      
nve1      2001003  225.4.0.2         Up    CP   L2 [1003]          SA      
nve1      2001004  225.4.0.200       Up    CP   L2 [1004]          SA      

DMDC-N9K-Leaf3#
DMDC-N9K-Leaf3# show nve peers
Interface Peer-IP          State LearnType Uptime   Router-Mac       
--------- ---------------  ----- --------- -------- -----------------
nve1      10.30.13.247     Up    CP        2d21h    5000.004c.0007   
nve1      10.30.13.248     Up    CP        2d21h    5000.004d.0007   
nve1      10.30.13.250     Up    CP        2d21h    5000.0018.0007   

DMDC-N9K-Leaf3#
DMDC-N9K-Leaf3# show bgp l2vpn evpn summary
BGP summary information for VRF default, address family L2VPN EVPN
BGP router identifier 10.30.13.249, local AS number 65535
BGP table version is 7791, L2VPN EVPN config peers 2, capable peers 2
162 network entries and 287 paths using 42060 bytes of memory
BGP attribute entries [53/8480], BGP AS path entries [2/12]
BGP community entries [0/0], BGP clusterlist entries [3/12]

Neighbor        V    AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
10.30.13.251    4 65535    5898    4461     7791    0    0    2d21h 89        
10.30.13.253    4 65535    5940    4467     7791    0    0    1d02h 89        
DMDC-N9K-Leaf3#
DMDC-N9K-Leaf3# show nve vni ingress-replication
DMDC-N9K-Leaf3# show nve peers contr
control-plane       control-plane-vni   
DMDC-N9K-Leaf3# show nve peers control-plane-vni
Peer                 VNI     Learn-Source Gateway-MAC        Peer-type
-------------------- ------- ---------- ------------------ ----------
10.30.13.247         900001  BGP-RNH    5000.004c.0007     FAB       
 10.30.13.247         900003  BGP-RNH    5000.004c.0007     FAB       
 10.30.13.247         2001001 BGP-RNH    0000.0000.0000     FAB       
 10.30.13.248         900003  BGP-RNH    5000.004d.0007     FAB       
 10.30.13.250         900001  BGP-RNH    5000.0018.0007     FAB       
 10.30.13.250         900002  BGP-RNH    5000.0018.0007     FAB       
 10.30.13.250         900003  BGP-RNH    5000.0018.0007     FAB       
 10.30.13.250         2001001 BGP-RNH    0000.0000.0000     FAB       
 10.30.13.250         2001003 BGP-RNH    0000.0000.0000     FAB       
 10.30.13.250         2001004 BGP-RNH    0000.0000.0000     FAB       


DMDC-N9K-Leaf3#
DMDC-N9K-Leaf3# show l2route evpn mac all

Flags -(Rmac):Router MAC (Stt):Static (L):Local (R):Remote (V):vPC link
(Dup):Duplicate (Spl):Split (Rcv):Recv (AD):Auto-Delete (D):Del Pending
(S):Stale (C):Clear, (Ps):Peer Sync (O):Re-Originated (Nho):NH-Override
(Pf):Permanently-Frozen

Topology    Mac Address    Prod   Flags         Seq No     Next-Hops      
----------- -------------- ------ ------------- ---------- ----------------
101         5000.0018.0007 VXLAN  Rmac          0          10.30.13.250   
101         5000.004c.0007 VXLAN  Rmac          0          10.30.13.247   
102         5000.0018.0007 VXLAN  Rmac          0          10.30.13.250   
103         5000.0018.0007 VXLAN  Rmac          0          10.30.13.250   
103         5000.004c.0007 VXLAN  Rmac          0          10.30.13.247   
103         5000.004d.0007 VXLAN  Rmac          0          10.30.13.248   
1001        0050.0100.0901 Local  L,            43         Eth1/6         
1001        0050.0100.1601 BGP    SplRcv        42         10.30.13.250   
1001        aabb.cc00.3f11 Local  L,            0          Eth1/3         
1001        aabb.cc00.4511 Local  L,            0          Eth1/3         
1001        aabb.cc00.4a11 BGP    SplRcv        0          10.30.13.247   
1001        aabb.cc00.4e11 BGP    SplRcv        0          10.30.13.247   
1003        0050.0100.0902 Local  L,            0          Eth1/7         
1003        0050.0100.1602 BGP    SplRcv        0          10.30.13.250   
1003        5000.0058.0001 Local  L,            0          Eth1/10        
1003        5000.0059.0001 BGP    SplRcv        0          10.30.13.250   
1004        5000.0058.0002 Local  L,            0          Eth1/11        
1004        5000.0059.0002 BGP    SplRcv        0          10.30.13.250   
DMDC-N9K-Leaf3#
DMDC-N9K-Leaf3# show forwarding nve l2 ingress-replication-peers

slot  1
=======

DMDC-N9K-Leaf3#

```

```
DMDC-N9K-Leaf1# show int nve1
nve1 is up
admin state is up,  Hardware: NVE
  MTU 9216 bytes
  Encapsulation VXLAN
  Auto-mdix is turned off
  RX
    ucast: 0 pkts, 0 bytes - mcast: 0 pkts, 0 bytes
  TX
    ucast: 0 pkts, 0 bytes - mcast: 0 pkts, 0 bytes

DMDC-N9K-Leaf1# show bgp session
Total peers 4, established peers 3
ASN 65535
VRF default, local ASN 65535
peers 3, established peers 2, local router-id 10.30.13.247
State: I-Idle, A-Active, O-Open, E-Established, C-Closing, S-Shutdown

Neighbor        ASN    Flaps LastUpDn|LastRead|LastWrit St Port(L/R)  Notif(S/R)
10.30.13.34       100 0     2d21h   |never   |never    I   0/0          0/0
10.30.13.251    65535 0     2d21h   |00:00:01|00:00:15 E   179/34334      0/0
10.30.13.253    65535 1     1d02h   |00:00:01|00:00:15 E   179/38929      1/0
DMDC-N9K-Leaf1# show int nve1
nve1 is up
admin state is up,  Hardware: NVE
  MTU 9216 bytes
  Encapsulation VXLAN
  Auto-mdix is turned off
  RX
    ucast: 0 pkts, 0 bytes - mcast: 0 pkts, 0 bytes
  TX
    ucast: 0 pkts, 0 bytes - mcast: 0 pkts, 0 bytes

DMDC-N9K-Leaf1# show mac address-table
Legend:
        * - primary entry, G - Gateway MAC, (R) - Routed MAC, O - Overlay MAC
        age - seconds since last seen,+ - primary entry using vPC Peer-Link,
        (T) - True, (F) - False, C - ControlPlane MAC, ~ - vsan
   VLAN     MAC Address      Type      age     Secure NTFY Ports
---------+-----------------+--------+---------+------+----+------------------
*  101     5000.004c.0007   static   -         F      F    Vlan101
*  102     5000.004c.0007   static   -         F      F    Vlan102
*  103     5000.004c.0007   static   -         F      F    Vlan103
G    -     0000.2222.3333   static   -         F      F    sup-eth1(R)
G    -     5000.004c.0007   static   -         F      F     (R)
G  101     5000.004c.0007   static   -         F      F    sup-eth1(R)
G  102     5000.004c.0007   static   -         F      F    sup-eth1(R)
G  103     5000.004c.0007   static   -         F      F    sup-eth1(R)
G 3000     5000.004c.0007   static   -         F      F    sup-eth1(R)
G 1001     5000.004c.0007   static   -         F      F    sup-eth1(R)
G 1002     5000.004c.0007   static   -         F      F    sup-eth1(R)
G 1003     5000.004c.0007   static   -         F      F    sup-eth1(R)
G 1004     5000.004c.0007   static   -         F      F    sup-eth1(R)
G    -     aabb.cc95.0121   static   -         F      F    sup-eth1(R) (Eth1/12)
DMDC-N9K-Leaf1#
DMDC-N9K-Leaf1# show nve vni
Codes: CP - Control Plane        DP - Data Plane          
       UC - Unconfigured         SA - Suppress ARP        
       SU - Suppress Unknown Unicast

Interface VNI      Multicast-group   State Mode Type [BD/VRF]      Flags
--------- -------- ----------------- ----- ---- ------------------ -----
nve1      900001   n/a               Up    CP   L3 [Tenant-1]             
nve1      900002   n/a               Up    CP   L3 [Tenant-2]             
nve1      900003   n/a               Up    CP   L3 [Tenant-3]             
nve1      2001001  225.4.0.1         Up    CP   L2 [1001]          SA      
nve1      2001002  225.4.0.100       Up    CP   L2 [1002]          SA      
nve1      2001003  225.4.0.2         Up    CP   L2 [1003]          SA      
nve1      2001004  225.4.0.200       Up    CP   L2 [1004]          SA      

DMDC-N9K-Leaf1#
DMDC-N9K-Leaf1# show vxlan
Vlan            VN-Segment
====            ==========
101             900001
102             900002
103             900003
1001            2001001
1002            2001002
1003            2001003
1004            2001004
DMDC-N9K-Leaf1#
DMDC-N9K-Leaf1# show nve peers
Interface Peer-IP          State LearnType Uptime   Router-Mac       
--------- ---------------  ----- --------- -------- -----------------
nve1      10.30.13.248     Up    CP        2d21h    5000.004d.0007   
nve1      10.30.13.249     Up    CP        2d21h    5000.000a.0007   
nve1      10.30.13.250     Up    CP        2d21h    5000.0018.0007   

DMDC-N9K-Leaf1#
DMDC-N9K-Leaf1# show bgp l2vpn evpn summary
BGP summary information for VRF default, address family L2VPN EVPN
BGP router identifier 10.30.13.247, local AS number 65535
BGP table version is 9098, L2VPN EVPN config peers 2, capable peers 2
137 network entries and 233 paths using 35632 bytes of memory
BGP attribute entries [49/7840], BGP AS path entries [2/12]
BGP community entries [0/0], BGP clusterlist entries [3/12]

Neighbor        V    AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
10.30.13.251    4 65535    6542    4171     9098    0    0    2d21h 60        
10.30.13.253    4 65535    6592    4173     9098    0    0    1d02h 60        
DMDC-N9K-Leaf1#
DMDC-N9K-Leaf1# show nve vni ingress-replication
DMDC-N9K-Leaf1# show nve peers  con
control-plane       control-plane-vni   
DMDC-N9K-Leaf1# show nve peers  control-plane-vni
Peer                 VNI     Learn-Source Gateway-MAC        Peer-type
-------------------- ------- ---------- ------------------ ----------
10.30.13.248         900003  BGP-RNH    5000.004d.0007     FAB       
 10.30.13.249         900001  BGP-RNH    5000.000a.0007     FAB       
 10.30.13.249         900002  BGP-RNH    5000.000a.0007     FAB       
 10.30.13.249         900003  BGP-RNH    5000.000a.0007     FAB       
 10.30.13.249         2001001 BGP-RNH    0000.0000.0000     FAB       
 10.30.13.249         2001003 BGP-RNH    0000.0000.0000     FAB       
 10.30.13.249         2001004 BGP-RNH    0000.0000.0000     FAB       
 10.30.13.250         900001  BGP-RNH    5000.0018.0007     FAB       
 10.30.13.250         900002  BGP-RNH    5000.0018.0007     FAB       
 10.30.13.250         900003  BGP-RNH    5000.0018.0007     FAB       
 10.30.13.250         2001001 BGP-RNH    0000.0000.0000     FAB       
 10.30.13.250         2001003 BGP-RNH    0000.0000.0000     FAB       
 10.30.13.250         2001004 BGP-RNH    0000.0000.0000     FAB       


DMDC-N9K-Leaf1#
DMDC-N9K-Leaf1# show l2route evpn mac all

Flags -(Rmac):Router MAC (Stt):Static (L):Local (R):Remote (V):vPC link
(Dup):Duplicate (Spl):Split (Rcv):Recv (AD):Auto-Delete (D):Del Pending
(S):Stale (C):Clear, (Ps):Peer Sync (O):Re-Originated (Nho):NH-Override
(Pf):Permanently-Frozen

Topology    Mac Address    Prod   Flags         Seq No     Next-Hops      
----------- -------------- ------ ------------- ---------- ----------------
101         5000.000a.0007 VXLAN  Rmac          0          10.30.13.249   
101         5000.0018.0007 VXLAN  Rmac          0          10.30.13.250   
102         5000.000a.0007 VXLAN  Rmac          0          10.30.13.249   
102         5000.0018.0007 VXLAN  Rmac          0          10.30.13.250   
103         5000.000a.0007 VXLAN  Rmac          0          10.30.13.249   
103         5000.0018.0007 VXLAN  Rmac          0          10.30.13.250   
103         5000.004d.0007 VXLAN  Rmac          0          10.30.13.248   
1001        0050.0100.0901 BGP    SplRcv        43         10.30.13.249   
1001        0050.0100.1601 BGP    SplRcv        0          10.30.13.250   
1001        aabb.cc00.3f11 BGP    SplRcv        0          10.30.13.249   
1001        aabb.cc00.4511 BGP    SplRcv        0          10.30.13.249   
1001        aabb.cc00.4a11 Local  L,            0          Eth1/3         
1001        aabb.cc00.4e11 Local  L,            0          Eth1/3         
1003        0050.0100.0902 BGP    SplRcv        0          10.30.13.249   
1003        0050.0100.1602 BGP    SplRcv        0          10.30.13.250   
1003        5000.0058.0001 BGP    SplRcv        0          10.30.13.249   
1003        5000.0059.0001 BGP    SplRcv        0          10.30.13.250   
1004        5000.0058.0002 BGP    SplRcv        0          10.30.13.249   
1004        5000.0059.0002 BGP    SplRcv        0          10.30.13.250   
DMDC-N9K-Leaf1#   
DMDC-N9K-Leaf1#
DMDC-N9K-Leaf1# show forwarding nve l2 ingress-replication-peers

slot  1
=======

DMDC-N9K-Leaf1#
```



3.

2.

```Markdown

NXB\Router/3>show ip ospf neighbor

Neighbor ID     Pri   State           Dead Time   Address         Interface
172.24.30.6       1   FULL/  -        00:00:35    172.24.32.81    Serial0

```

```java
def xxx:



```


# ASA部分配置



# F5部分配置


#
