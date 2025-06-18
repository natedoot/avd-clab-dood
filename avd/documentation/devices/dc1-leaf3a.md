# dc1-leaf3a

## Table of Contents

- [Management](#management)
  - [Management Interfaces](#management-interfaces)
  - [Management API HTTP](#management-api-http)
- [Authentication](#authentication)
  - [Local Users](#local-users)
  - [Enable Password](#enable-password)
  - [AAA Authorization](#aaa-authorization)
- [MLAG](#mlag)
  - [MLAG Summary](#mlag-summary)
  - [MLAG Device Configuration](#mlag-device-configuration)
- [Spanning Tree](#spanning-tree)
  - [Spanning Tree Summary](#spanning-tree-summary)
  - [Spanning Tree Device Configuration](#spanning-tree-device-configuration)
- [Internal VLAN Allocation Policy](#internal-vlan-allocation-policy)
  - [Internal VLAN Allocation Policy Summary](#internal-vlan-allocation-policy-summary)
  - [Internal VLAN Allocation Policy Device Configuration](#internal-vlan-allocation-policy-device-configuration)
- [VLANs](#vlans)
  - [VLANs Summary](#vlans-summary)
  - [VLANs Device Configuration](#vlans-device-configuration)
- [Interfaces](#interfaces)
  - [Ethernet Interfaces](#ethernet-interfaces)
  - [Port-Channel Interfaces](#port-channel-interfaces)
  - [Loopback Interfaces](#loopback-interfaces)
  - [VLAN Interfaces](#vlan-interfaces)
  - [VXLAN Interface](#vxlan-interface)
- [Routing](#routing)
  - [Service Routing Protocols Model](#service-routing-protocols-model)
  - [Virtual Router MAC Address](#virtual-router-mac-address)
  - [IP Routing](#ip-routing)
  - [IPv6 Routing](#ipv6-routing)
  - [Static Routes](#static-routes)
  - [Router BGP](#router-bgp)
- [BFD](#bfd)
  - [Router BFD](#router-bfd)
- [Multicast](#multicast)
  - [IP IGMP Snooping](#ip-igmp-snooping)
- [Filters](#filters)
  - [Prefix-lists](#prefix-lists)
  - [Route-maps](#route-maps)
- [VRF Instances](#vrf-instances)
  - [VRF Instances Summary](#vrf-instances-summary)
  - [VRF Instances Device Configuration](#vrf-instances-device-configuration)
- [Virtual Source NAT](#virtual-source-nat)
  - [Virtual Source NAT Summary](#virtual-source-nat-summary)
  - [Virtual Source NAT Configuration](#virtual-source-nat-configuration)

## Management

### Management Interfaces

#### Management Interfaces Summary

##### IPv4

| Management Interface | Description | Type | VRF | IP Address | Gateway |
| -------------------- | ----------- | ---- | --- | ---------- | ------- |
| Management0 | OOB_MANAGEMENT | oob | default | 192.168.0.107/24 | 192.168.0.1 |

##### IPv6

| Management Interface | Description | Type | VRF | IPv6 Address | IPv6 Gateway |
| -------------------- | ----------- | ---- | --- | ------------ | ------------ |
| Management0 | OOB_MANAGEMENT | oob | default | - | - |

#### Management Interfaces Device Configuration

```eos
!
interface Management0
   description OOB_MANAGEMENT
   no shutdown
   ip address 192.168.0.107/24
```

### Management API HTTP

#### Management API HTTP Summary

| HTTP | HTTPS | UNIX-Socket | Default Services |
| ---- | ----- | ----------- | ---------------- |
| False | True | - | - |

#### Management API VRF Access

| VRF Name | IPv4 ACL | IPv6 ACL |
| -------- | -------- | -------- |
| default | - | - |

#### Management API HTTP Device Configuration

```eos
!
management api http-commands
   protocol https
   no shutdown
   !
   vrf default
      no shutdown
```

## Authentication

### Local Users

#### Local Users Summary

| User | Privilege | Role | Disabled | Shell |
| ---- | --------- | ---- | -------- | ----- |
| admin | 15 | network-admin | False | - |
| cvpadmin | 15 | network-admin | False | - |

#### Local Users Device Configuration

```eos
!
username admin privilege 15 role network-admin secret sha512 <removed>
username cvpadmin privilege 15 role network-admin secret sha512 <removed>
```

### Enable Password

Enable password has been disabled

### AAA Authorization

#### AAA Authorization Summary

| Type | User Stores |
| ---- | ----------- |
| Exec | local |

Authorization for configuration commands is disabled.

#### AAA Authorization Device Configuration

```eos
aaa authorization exec default local
!
```

## MLAG

### MLAG Summary

| Domain-id | Local-interface | Peer-address | Peer-link |
| --------- | --------------- | ------------ | --------- |
| DC1_L3_LEAF3 | Vlan4094 | 10.255.1.73 | Port-Channel3 |

Dual primary detection is disabled.

### MLAG Device Configuration

```eos
!
mlag configuration
   domain-id DC1_L3_LEAF3
   local-interface Vlan4094
   peer-address 10.255.1.73
   peer-link Port-Channel3
   reload-delay mlag 300
   reload-delay non-mlag 330
```

## Spanning Tree

### Spanning Tree Summary

STP mode: **mstp**

#### MSTP Instance and Priority

| Instance(s) | Priority |
| -------- | -------- |
| 0 | 4096 |

#### Global Spanning-Tree Settings

- Spanning Tree disabled for VLANs: **4093-4094**

### Spanning Tree Device Configuration

```eos
!
spanning-tree mode mstp
no spanning-tree vlan-id 4093-4094
spanning-tree mst 0 priority 4096
```

## Internal VLAN Allocation Policy

### Internal VLAN Allocation Policy Summary

| Policy Allocation | Range Beginning | Range Ending |
| ------------------| --------------- | ------------ |
| ascending | 1006 | 1199 |

### Internal VLAN Allocation Policy Device Configuration

```eos
!
vlan internal order ascending range 1006 1199
```

## VLANs

### VLANs Summary

| VLAN ID | Name | Trunk Groups |
| ------- | ---- | ------------ |
| 5 | 5 | - |
| 11 | VRF10_VLAN11 | - |
| 12 | VRF10_VLAN12 | - |
| 15 | VRF10_VLAN15 | - |
| 21 | VRF11_VLAN21 | - |
| 22 | VRF11_VLAN22 | - |
| 23 | VRF11_VLAN23 | - |
| 31 | VRF12_VLAN31 | - |
| 32 | VRF12_VLAN32 | - |
| 50 | hr-50 | - |
| 60 | VRF11_VLAN60 | - |
| 3009 | MLAG_L3_VRF_VRF10 | MLAG |
| 3010 | MLAG_L3_VRF_VRF11 | MLAG |
| 3011 | MLAG_L3_VRF_VRF12 | MLAG |
| 3012 | MLAG_L3_VRF_hrvrf | MLAG |
| 3401 | L2_VLAN3401 | - |
| 3402 | L2_VLAN3402 | - |
| 4093 | MLAG_L3 | MLAG |
| 4094 | MLAG | MLAG |

### VLANs Device Configuration

```eos
!
vlan 5
   name 5
!
vlan 11
   name VRF10_VLAN11
!
vlan 12
   name VRF10_VLAN12
!
vlan 15
   name VRF10_VLAN15
!
vlan 21
   name VRF11_VLAN21
!
vlan 22
   name VRF11_VLAN22
!
vlan 23
   name VRF11_VLAN23
!
vlan 31
   name VRF12_VLAN31
!
vlan 32
   name VRF12_VLAN32
!
vlan 50
   name hr-50
!
vlan 60
   name VRF11_VLAN60
!
vlan 3009
   name MLAG_L3_VRF_VRF10
   trunk group MLAG
!
vlan 3010
   name MLAG_L3_VRF_VRF11
   trunk group MLAG
!
vlan 3011
   name MLAG_L3_VRF_VRF12
   trunk group MLAG
!
vlan 3012
   name MLAG_L3_VRF_hrvrf
   trunk group MLAG
!
vlan 3401
   name L2_VLAN3401
!
vlan 3402
   name L2_VLAN3402
!
vlan 4093
   name MLAG_L3
   trunk group MLAG
!
vlan 4094
   name MLAG
   trunk group MLAG
```

## Interfaces

### Ethernet Interfaces

#### Ethernet Interfaces Summary

##### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | Channel-Group |
| --------- | ----------- | ---- | ----- | ----------- | ----------- | ------------- |
| Ethernet3 | MLAG_dc1-leaf3b_Ethernet3 | *trunk | *- | *- | *MLAG | 3 |
| Ethernet4 | MLAG_dc1-leaf3b_Ethernet4 | *trunk | *- | *- | *MLAG | 3 |

*Inherited from Port-Channel Interface

##### IPv4

| Interface | Description | Channel Group | IP Address | VRF |  MTU | Shutdown | ACL In | ACL Out |
| --------- | ----------- | ------------- | ---------- | ----| ---- | -------- | ------ | ------- |
| Ethernet1 | P2P_dc1-spine1_Ethernet5 | - | 10.255.255.17/31 | default | 1500 | False | - | - |
| Ethernet2 | P2P_dc1-spine2_Ethernet5 | - | 10.255.255.19/31 | default | 1500 | False | - | - |

#### Ethernet Interfaces Device Configuration

```eos
!
interface Ethernet1
   description P2P_dc1-spine1_Ethernet5
   no shutdown
   mtu 1500
   no switchport
   ip address 10.255.255.17/31
!
interface Ethernet2
   description P2P_dc1-spine2_Ethernet5
   no shutdown
   mtu 1500
   no switchport
   ip address 10.255.255.19/31
!
interface Ethernet3
   description MLAG_dc1-leaf3b_Ethernet3
   no shutdown
   channel-group 3 mode active
!
interface Ethernet4
   description MLAG_dc1-leaf3b_Ethernet4
   no shutdown
   channel-group 3 mode active
```

### Port-Channel Interfaces

#### Port-Channel Interfaces Summary

##### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | LACP Fallback Timeout | LACP Fallback Mode | MLAG ID | EVPN ESI |
| --------- | ----------- | ---- | ----- | ----------- | ------------| --------------------- | ------------------ | ------- | -------- |
| Port-Channel3 | MLAG_dc1-leaf3b_Port-Channel3 | trunk | - | - | MLAG | - | - | - | - |

#### Port-Channel Interfaces Device Configuration

```eos
!
interface Port-Channel3
   description MLAG_dc1-leaf3b_Port-Channel3
   no shutdown
   switchport mode trunk
   switchport trunk group MLAG
   switchport
```

### Loopback Interfaces

#### Loopback Interfaces Summary

##### IPv4

| Interface | Description | VRF | IP Address |
| --------- | ----------- | --- | ---------- |
| Loopback0 | ROUTER_ID | default | 10.255.0.7/32 |
| Loopback1 | VXLAN_TUNNEL_SOURCE | default | 10.255.1.7/32 |
| Loopback10 | DIAG_VRF_VRF10 | VRF10 | 10.255.10.7/32 |
| Loopback11 | DIAG_VRF_VRF11 | VRF11 | 10.255.11.7/32 |

##### IPv6

| Interface | Description | VRF | IPv6 Address |
| --------- | ----------- | --- | ------------ |
| Loopback0 | ROUTER_ID | default | - |
| Loopback1 | VXLAN_TUNNEL_SOURCE | default | - |
| Loopback10 | DIAG_VRF_VRF10 | VRF10 | - |
| Loopback11 | DIAG_VRF_VRF11 | VRF11 | - |

#### Loopback Interfaces Device Configuration

```eos
!
interface Loopback0
   description ROUTER_ID
   no shutdown
   ip address 10.255.0.7/32
!
interface Loopback1
   description VXLAN_TUNNEL_SOURCE
   no shutdown
   ip address 10.255.1.7/32
!
interface Loopback10
   description DIAG_VRF_VRF10
   no shutdown
   vrf VRF10
   ip address 10.255.10.7/32
!
interface Loopback11
   description DIAG_VRF_VRF11
   no shutdown
   vrf VRF11
   ip address 10.255.11.7/32
```

### VLAN Interfaces

#### VLAN Interfaces Summary

| Interface | Description | VRF |  MTU | Shutdown |
| --------- | ----------- | --- | ---- | -------- |
| Vlan5 | 5 | VRF10 | - | True |
| Vlan11 | VRF10_VLAN11 | VRF10 | - | False |
| Vlan12 | VRF10_VLAN12 | VRF10 | - | False |
| Vlan15 | VRF10_VLAN15 | VRF10 | - | False |
| Vlan21 | VRF11_VLAN21 | VRF11 | - | False |
| Vlan22 | VRF11_VLAN22 | VRF11 | - | False |
| Vlan23 | VRF11_VLAN23 | VRF11 | - | False |
| Vlan31 | VRF12_VLAN31 | VRF12 | - | False |
| Vlan32 | VRF12_VLAN32 | VRF12 | - | False |
| Vlan50 | hr-50 | hrvrf | - | False |
| Vlan60 | VRF11_VLAN60 | VRF11 | - | False |
| Vlan3009 | MLAG_L3_VRF_VRF10 | VRF10 | 1500 | False |
| Vlan3010 | MLAG_L3_VRF_VRF11 | VRF11 | 1500 | False |
| Vlan3011 | MLAG_L3_VRF_VRF12 | VRF12 | 1500 | False |
| Vlan3012 | MLAG_L3_VRF_hrvrf | hrvrf | 1500 | False |
| Vlan4093 | MLAG_L3 | default | 1500 | False |
| Vlan4094 | MLAG | default | 1500 | False |

##### IPv4

| Interface | VRF | IP Address | IP Address Virtual | IP Router Virtual Address | ACL In | ACL Out |
| --------- | --- | ---------- | ------------------ | ------------------------- | ------ | ------- |
| Vlan5 |  VRF10  |  10.10.5.1/24  |  -  |  -  |  -  |  -  |
| Vlan11 |  VRF10  |  -  |  10.10.11.1/24  |  -  |  -  |  -  |
| Vlan12 |  VRF10  |  -  |  10.10.12.1/24  |  -  |  -  |  -  |
| Vlan15 |  VRF10  |  10.10.15.1/24  |  -  |  -  |  -  |  -  |
| Vlan21 |  VRF11  |  -  |  10.10.21.1/24  |  -  |  -  |  -  |
| Vlan22 |  VRF11  |  -  |  10.10.22.1/24  |  -  |  -  |  -  |
| Vlan23 |  VRF11  |  10.10.23.1/24  |  -  |  -  |  -  |  -  |
| Vlan31 |  VRF12  |  10.10.31.1/24  |  -  |  -  |  -  |  -  |
| Vlan32 |  VRF12  |  10.10.32.1/24  |  -  |  -  |  -  |  -  |
| Vlan50 |  hrvrf  |  10.10.50.1/24  |  -  |  -  |  -  |  -  |
| Vlan60 |  VRF11  |  10.10.60.1/24  |  -  |  -  |  -  |  -  |
| Vlan3009 |  VRF10  |  10.255.1.104/31  |  -  |  -  |  -  |  -  |
| Vlan3010 |  VRF11  |  10.255.1.104/31  |  -  |  -  |  -  |  -  |
| Vlan3011 |  VRF12  |  10.255.1.104/31  |  -  |  -  |  -  |  -  |
| Vlan3012 |  hrvrf  |  10.255.1.104/31  |  -  |  -  |  -  |  -  |
| Vlan4093 |  default  |  10.255.1.104/31  |  -  |  -  |  -  |  -  |
| Vlan4094 |  default  |  10.255.1.72/31  |  -  |  -  |  -  |  -  |

#### VLAN Interfaces Device Configuration

```eos
!
interface Vlan5
   description 5
   shutdown
   vrf VRF10
   ip address 10.10.5.1/24
!
interface Vlan11
   description VRF10_VLAN11
   no shutdown
   vrf VRF10
   ip address virtual 10.10.11.1/24
!
interface Vlan12
   description VRF10_VLAN12
   no shutdown
   vrf VRF10
   ip address virtual 10.10.12.1/24
!
interface Vlan15
   description VRF10_VLAN15
   no shutdown
   vrf VRF10
   ip address 10.10.15.1/24
!
interface Vlan21
   description VRF11_VLAN21
   no shutdown
   vrf VRF11
   ip address virtual 10.10.21.1/24
!
interface Vlan22
   description VRF11_VLAN22
   no shutdown
   vrf VRF11
   ip address virtual 10.10.22.1/24
!
interface Vlan23
   description VRF11_VLAN23
   no shutdown
   vrf VRF11
   ip address 10.10.23.1/24
!
interface Vlan31
   description VRF12_VLAN31
   no shutdown
   vrf VRF12
   ip address 10.10.31.1/24
!
interface Vlan32
   description VRF12_VLAN32
   no shutdown
   vrf VRF12
   ip address 10.10.32.1/24
!
interface Vlan50
   description hr-50
   no shutdown
   vrf hrvrf
   ip address 10.10.50.1/24
!
interface Vlan60
   description VRF11_VLAN60
   no shutdown
   vrf VRF11
   ip address 10.10.60.1/24
!
interface Vlan3009
   description MLAG_L3_VRF_VRF10
   no shutdown
   mtu 1500
   vrf VRF10
   ip address 10.255.1.104/31
!
interface Vlan3010
   description MLAG_L3_VRF_VRF11
   no shutdown
   mtu 1500
   vrf VRF11
   ip address 10.255.1.104/31
!
interface Vlan3011
   description MLAG_L3_VRF_VRF12
   no shutdown
   mtu 1500
   vrf VRF12
   ip address 10.255.1.104/31
!
interface Vlan3012
   description MLAG_L3_VRF_hrvrf
   no shutdown
   mtu 1500
   vrf hrvrf
   ip address 10.255.1.104/31
!
interface Vlan4093
   description MLAG_L3
   no shutdown
   mtu 1500
   ip address 10.255.1.104/31
!
interface Vlan4094
   description MLAG
   no shutdown
   mtu 1500
   no autostate
   ip address 10.255.1.72/31
```

### VXLAN Interface

#### VXLAN Interface Summary

| Setting | Value |
| ------- | ----- |
| Source Interface | Loopback1 |
| UDP port | 4789 |
| EVPN MLAG Shared Router MAC | mlag-system-id |

##### VLAN to VNI, Flood List and Multicast Group Mappings

| VLAN | VNI | Flood List | Multicast Group |
| ---- | --- | ---------- | --------------- |
| 5 | 10005 | - | - |
| 11 | 10011 | - | - |
| 12 | 10012 | - | - |
| 15 | 10015 | - | - |
| 21 | 10021 | - | - |
| 22 | 10022 | - | - |
| 23 | 10023 | - | - |
| 31 | 10031 | - | - |
| 32 | 10032 | - | - |
| 50 | 10050 | - | - |
| 60 | 10060 | - | - |
| 3401 | 13401 | - | - |
| 3402 | 13402 | - | - |

##### VRF to VNI and Multicast Group Mappings

| VRF | VNI | Multicast Group |
| ---- | --- | --------------- |
| hrvrf | 13 | - |
| VRF10 | 10 | - |
| VRF11 | 11 | - |
| VRF12 | 12 | - |

#### VXLAN Interface Device Configuration

```eos
!
interface Vxlan1
   description dc1-leaf3a_VTEP
   vxlan source-interface Loopback1
   vxlan virtual-router encapsulation mac-address mlag-system-id
   vxlan udp-port 4789
   vxlan vlan 5 vni 10005
   vxlan vlan 11 vni 10011
   vxlan vlan 12 vni 10012
   vxlan vlan 15 vni 10015
   vxlan vlan 21 vni 10021
   vxlan vlan 22 vni 10022
   vxlan vlan 23 vni 10023
   vxlan vlan 31 vni 10031
   vxlan vlan 32 vni 10032
   vxlan vlan 50 vni 10050
   vxlan vlan 60 vni 10060
   vxlan vlan 3401 vni 13401
   vxlan vlan 3402 vni 13402
   vxlan vrf hrvrf vni 13
   vxlan vrf VRF10 vni 10
   vxlan vrf VRF11 vni 11
   vxlan vrf VRF12 vni 12
```

## Routing

### Service Routing Protocols Model

Multi agent routing protocol model enabled

```eos
!
service routing protocols model multi-agent
```

### Virtual Router MAC Address

#### Virtual Router MAC Address Summary

Virtual Router MAC Address: 00:1c:73:00:00:99

#### Virtual Router MAC Address Device Configuration

```eos
!
ip virtual-router mac-address 00:1c:73:00:00:99
```

### IP Routing

#### IP Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | True |
| hrvrf | True |
| VRF10 | True |
| VRF11 | True |
| VRF12 | True |

#### IP Routing Device Configuration

```eos
!
ip routing
ip routing vrf hrvrf
ip routing vrf VRF10
ip routing vrf VRF11
ip routing vrf VRF12
```

### IPv6 Routing

#### IPv6 Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | False |
| default | false |
| hrvrf | false |
| VRF10 | false |
| VRF11 | false |
| VRF12 | false |

### Static Routes

#### Static Routes Summary

| VRF | Destination Prefix | Next Hop IP | Exit interface | Administrative Distance | Tag | Route Name | Metric |
| --- | ------------------ | ----------- | -------------- | ----------------------- | --- | ---------- | ------ |
| default | 0.0.0.0/0 | 192.168.0.1 | - | 1 | - | - | - |

#### Static Routes Device Configuration

```eos
!
ip route 0.0.0.0/0 192.168.0.1
```

### Router BGP

ASN Notation: asplain

#### Router BGP Summary

| BGP AS | Router ID |
| ------ | --------- |
| 65103 | 10.255.0.7 |

| BGP Tuning |
| ---------- |
| no bgp default ipv4-unicast |
| maximum-paths 4 ecmp 4 |

#### Router BGP Peer Groups

##### EVPN-OVERLAY-PEERS

| Settings | Value |
| -------- | ----- |
| Address Family | evpn |
| Source | Loopback0 |
| BFD | True |
| Ebgp multihop | 3 |
| Send community | all |
| Maximum routes | 0 (no limit) |

##### IPv4-UNDERLAY-PEERS

| Settings | Value |
| -------- | ----- |
| Address Family | ipv4 |
| Send community | all |
| Maximum routes | 12000 |

##### MLAG-IPv4-UNDERLAY-PEER

| Settings | Value |
| -------- | ----- |
| Address Family | ipv4 |
| Remote AS | 65103 |
| Next-hop self | True |
| Send community | all |
| Maximum routes | 12000 |

#### BGP Neighbors

| Neighbor | Remote AS | VRF | Shutdown | Send-community | Maximum-routes | Allowas-in | BFD | RIB Pre-Policy Retain | Route-Reflector Client | Passive | TTL Max Hops |
| -------- | --------- | --- | -------- | -------------- | -------------- | ---------- | --- | --------------------- | ---------------------- | ------- | ------------ |
| 10.255.0.1 | 65100 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - | - |
| 10.255.0.2 | 65100 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - | - |
| 10.255.1.105 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | default | - | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | - | - | - | - | - | - |
| 10.255.255.16 | 65100 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - | - |
| 10.255.255.18 | 65100 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - | - |
| 10.255.1.105 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | hrvrf | - | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | - | - | - | - | - | - |
| 10.255.1.105 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | VRF10 | - | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | - | - | - | - | - | - |
| 10.255.1.105 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | VRF11 | - | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | - | - | - | - | - | - |
| 10.255.1.105 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | VRF12 | - | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | - | - | - | - | - | - |

#### Router BGP EVPN Address Family

##### EVPN Peer Groups

| Peer Group | Activate | Route-map In | Route-map Out | Encapsulation | Next-hop-self Source Interface |
| ---------- | -------- | ------------ | ------------- | ------------- | ------------------------------ |
| EVPN-OVERLAY-PEERS | True |  - | - | default | - |

#### Router BGP VLANs

| VLAN | Route-Distinguisher | Both Route-Target | Import Route Target | Export Route-Target | Redistribute |
| ---- | ------------------- | ----------------- | ------------------- | ------------------- | ------------ |
| 5 | 10.255.0.7:10005 | 10005:10005 | - | - | learned |
| 11 | 10.255.0.7:10011 | 10011:10011 | - | - | learned |
| 12 | 10.255.0.7:10012 | 10012:10012 | - | - | learned |
| 15 | 10.255.0.7:10015 | 10015:10015 | - | - | learned |
| 21 | 10.255.0.7:10021 | 10021:10021 | - | - | learned |
| 22 | 10.255.0.7:10022 | 10022:10022 | - | - | learned |
| 23 | 10.255.0.7:10023 | 10023:10023 | - | - | learned |
| 31 | 10.255.0.7:10031 | 10031:10031 | - | - | learned |
| 32 | 10.255.0.7:10032 | 10032:10032 | - | - | learned |
| 50 | 10.255.0.7:10050 | 10050:10050 | - | - | learned |
| 60 | 10.255.0.7:10060 | 10060:10060 | - | - | learned |
| 3401 | 10.255.0.7:13401 | 13401:13401 | - | - | learned |
| 3402 | 10.255.0.7:13402 | 13402:13402 | - | - | learned |

#### Router BGP VRFs

| VRF | Route-Distinguisher | Redistribute | Graceful Restart |
| --- | ------------------- | ------------ | ---------------- |
| hrvrf | 10.255.0.7:13 | connected | - |
| VRF10 | 10.255.0.7:10 | connected | - |
| VRF11 | 10.255.0.7:11 | connected | - |
| VRF12 | 10.255.0.7:12 | connected | - |

#### Router BGP Device Configuration

```eos
!
router bgp 65103
   router-id 10.255.0.7
   no bgp default ipv4-unicast
   maximum-paths 4 ecmp 4
   neighbor EVPN-OVERLAY-PEERS peer group
   neighbor EVPN-OVERLAY-PEERS update-source Loopback0
   neighbor EVPN-OVERLAY-PEERS bfd
   neighbor EVPN-OVERLAY-PEERS ebgp-multihop 3
   neighbor EVPN-OVERLAY-PEERS send-community
   neighbor EVPN-OVERLAY-PEERS maximum-routes 0
   neighbor IPv4-UNDERLAY-PEERS peer group
   neighbor IPv4-UNDERLAY-PEERS send-community
   neighbor IPv4-UNDERLAY-PEERS maximum-routes 12000
   neighbor MLAG-IPv4-UNDERLAY-PEER peer group
   neighbor MLAG-IPv4-UNDERLAY-PEER remote-as 65103
   neighbor MLAG-IPv4-UNDERLAY-PEER next-hop-self
   neighbor MLAG-IPv4-UNDERLAY-PEER description dc1-leaf3b
   neighbor MLAG-IPv4-UNDERLAY-PEER route-map RM-MLAG-PEER-IN in
   neighbor MLAG-IPv4-UNDERLAY-PEER send-community
   neighbor MLAG-IPv4-UNDERLAY-PEER maximum-routes 12000
   neighbor 10.255.0.1 peer group EVPN-OVERLAY-PEERS
   neighbor 10.255.0.1 remote-as 65100
   neighbor 10.255.0.1 description dc1-spine1_Loopback0
   neighbor 10.255.0.2 peer group EVPN-OVERLAY-PEERS
   neighbor 10.255.0.2 remote-as 65100
   neighbor 10.255.0.2 description dc1-spine2_Loopback0
   neighbor 10.255.1.105 peer group MLAG-IPv4-UNDERLAY-PEER
   neighbor 10.255.1.105 description dc1-leaf3b_Vlan4093
   neighbor 10.255.255.16 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.255.255.16 remote-as 65100
   neighbor 10.255.255.16 description dc1-spine1_Ethernet5
   neighbor 10.255.255.18 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.255.255.18 remote-as 65100
   neighbor 10.255.255.18 description dc1-spine2_Ethernet5
   redistribute connected route-map RM-CONN-2-BGP
   !
   vlan 5
      rd 10.255.0.7:10005
      route-target both 10005:10005
      redistribute learned
   !
   vlan 11
      rd 10.255.0.7:10011
      route-target both 10011:10011
      redistribute learned
   !
   vlan 12
      rd 10.255.0.7:10012
      route-target both 10012:10012
      redistribute learned
   !
   vlan 15
      rd 10.255.0.7:10015
      route-target both 10015:10015
      redistribute learned
   !
   vlan 21
      rd 10.255.0.7:10021
      route-target both 10021:10021
      redistribute learned
   !
   vlan 22
      rd 10.255.0.7:10022
      route-target both 10022:10022
      redistribute learned
   !
   vlan 23
      rd 10.255.0.7:10023
      route-target both 10023:10023
      redistribute learned
   !
   vlan 31
      rd 10.255.0.7:10031
      route-target both 10031:10031
      redistribute learned
   !
   vlan 32
      rd 10.255.0.7:10032
      route-target both 10032:10032
      redistribute learned
   !
   vlan 50
      rd 10.255.0.7:10050
      route-target both 10050:10050
      redistribute learned
   !
   vlan 60
      rd 10.255.0.7:10060
      route-target both 10060:10060
      redistribute learned
   !
   vlan 3401
      rd 10.255.0.7:13401
      route-target both 13401:13401
      redistribute learned
   !
   vlan 3402
      rd 10.255.0.7:13402
      route-target both 13402:13402
      redistribute learned
   !
   address-family evpn
      neighbor EVPN-OVERLAY-PEERS activate
   !
   address-family ipv4
      no neighbor EVPN-OVERLAY-PEERS activate
      neighbor IPv4-UNDERLAY-PEERS activate
      neighbor MLAG-IPv4-UNDERLAY-PEER activate
   !
   vrf hrvrf
      rd 10.255.0.7:13
      route-target import evpn 13:13
      route-target export evpn 13:13
      router-id 10.255.0.7
      neighbor 10.255.1.105 peer group MLAG-IPv4-UNDERLAY-PEER
      neighbor 10.255.1.105 description dc1-leaf3b_Vlan3012
      redistribute connected route-map RM-CONN-2-BGP-VRFS
   !
   vrf VRF10
      rd 10.255.0.7:10
      route-target import evpn 10:10
      route-target export evpn 10:10
      router-id 10.255.0.7
      neighbor 10.255.1.105 peer group MLAG-IPv4-UNDERLAY-PEER
      neighbor 10.255.1.105 description dc1-leaf3b_Vlan3009
      redistribute connected route-map RM-CONN-2-BGP-VRFS
   !
   vrf VRF11
      rd 10.255.0.7:11
      route-target import evpn 11:11
      route-target export evpn 11:11
      router-id 10.255.0.7
      neighbor 10.255.1.105 peer group MLAG-IPv4-UNDERLAY-PEER
      neighbor 10.255.1.105 description dc1-leaf3b_Vlan3010
      redistribute connected route-map RM-CONN-2-BGP-VRFS
   !
   vrf VRF12
      rd 10.255.0.7:12
      route-target import evpn 12:12
      route-target export evpn 12:12
      router-id 10.255.0.7
      neighbor 10.255.1.105 peer group MLAG-IPv4-UNDERLAY-PEER
      neighbor 10.255.1.105 description dc1-leaf3b_Vlan3011
      redistribute connected route-map RM-CONN-2-BGP-VRFS
```

## BFD

### Router BFD

#### Router BFD Multihop Summary

| Interval | Minimum RX | Multiplier |
| -------- | ---------- | ---------- |
| 300 | 300 | 3 |

#### Router BFD Device Configuration

```eos
!
router bfd
   multihop interval 300 min-rx 300 multiplier 3
```

## Multicast

### IP IGMP Snooping

#### IP IGMP Snooping Summary

| IGMP Snooping | Fast Leave | Interface Restart Query | Proxy | Restart Query Interval | Robustness Variable |
| ------------- | ---------- | ----------------------- | ----- | ---------------------- | ------------------- |
| Enabled | - | - | - | - | - |

#### IP IGMP Snooping Device Configuration

```eos
```

## Filters

### Prefix-lists

#### Prefix-lists Summary

##### PL-LOOPBACKS-EVPN-OVERLAY

| Sequence | Action |
| -------- | ------ |
| 10 | permit 10.255.0.0/27 eq 32 |
| 20 | permit 10.255.1.0/27 eq 32 |

##### PL-MLAG-PEER-VRFS

| Sequence | Action |
| -------- | ------ |
| 10 | permit 10.255.1.104/31 |

#### Prefix-lists Device Configuration

```eos
!
ip prefix-list PL-LOOPBACKS-EVPN-OVERLAY
   seq 10 permit 10.255.0.0/27 eq 32
   seq 20 permit 10.255.1.0/27 eq 32
!
ip prefix-list PL-MLAG-PEER-VRFS
   seq 10 permit 10.255.1.104/31
```

### Route-maps

#### Route-maps Summary

##### RM-CONN-2-BGP

| Sequence | Type | Match | Set | Sub-Route-Map | Continue |
| -------- | ---- | ----- | --- | ------------- | -------- |
| 10 | permit | ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY | - | - | - |

##### RM-CONN-2-BGP-VRFS

| Sequence | Type | Match | Set | Sub-Route-Map | Continue |
| -------- | ---- | ----- | --- | ------------- | -------- |
| 10 | deny | ip address prefix-list PL-MLAG-PEER-VRFS | - | - | - |
| 20 | permit | - | - | - | - |

##### RM-MLAG-PEER-IN

| Sequence | Type | Match | Set | Sub-Route-Map | Continue |
| -------- | ---- | ----- | --- | ------------- | -------- |
| 10 | permit | - | origin incomplete | - | - |

#### Route-maps Device Configuration

```eos
!
route-map RM-CONN-2-BGP permit 10
   match ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY
!
route-map RM-CONN-2-BGP-VRFS deny 10
   match ip address prefix-list PL-MLAG-PEER-VRFS
!
route-map RM-CONN-2-BGP-VRFS permit 20
!
route-map RM-MLAG-PEER-IN permit 10
   description Make routes learned over MLAG Peer-link less preferred on spines to ensure optimal routing
   set origin incomplete
```

## VRF Instances

### VRF Instances Summary

| VRF Name | IP Routing |
| -------- | ---------- |
| hrvrf | enabled |
| VRF10 | enabled |
| VRF11 | enabled |
| VRF12 | enabled |

### VRF Instances Device Configuration

```eos
!
vrf instance hrvrf
!
vrf instance VRF10
!
vrf instance VRF11
!
vrf instance VRF12
```

## Virtual Source NAT

### Virtual Source NAT Summary

| Source NAT VRF | Source NAT IPv4 Address | Source NAT IPv6 Address |
| -------------- | ----------------------- | ----------------------- |
| VRF10 | 10.255.10.7 | - |
| VRF11 | 10.255.11.7 | - |

### Virtual Source NAT Configuration

```eos
!
ip address virtual source-nat vrf VRF10 address 10.255.10.7
ip address virtual source-nat vrf VRF11 address 10.255.11.7
```
