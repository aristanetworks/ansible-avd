# port-channel-interfaces
# Table of Contents
<!-- toc -->

- [Management](#management)
  - [Management Interfaces](#management-interfaces)
- [Authentication](#authentication)
- [Monitoring](#monitoring)
- [Internal VLAN Allocation Policy](#internal-vlan-allocation-policy)
  - [Internal VLAN Allocation Policy Summary](#internal-vlan-allocation-policy-summary)
- [Interfaces](#interfaces)
  - [Ethernet Interfaces](#ethernet-interfaces)
  - [Port-Channel Interfaces](#port-channel-interfaces)
- [Routing](#routing)
  - [IP Routing](#ip-routing)
  - [IPv6 Routing](#ipv6-routing)
  - [Router BFD](#router-bfd)
- [Multicast](#multicast)
- [Filters](#filters)
- [ACL](#acl)

<!-- toc -->
# Management

## Management Interfaces

### Management Interfaces Summary

#### IPv4

| Management Interface | description | Type | VRF | IP Address | Gateway |
| -------------------- | ----------- | ---- | --- | ---------- | ------- |
| Management1 | oob_management | oob | MGMT | 10.73.255.122/24 | 10.73.255.2 |

#### IPv6

| Management Interface | description | Type | VRF | IPv6 Address | IPv6 Gateway |
| -------------------- | ----------- | ---- | --- | ------------ | ------------ |
| Management1 | oob_management | oob | MGMT | -  | - |

### Management Interfaces Device Configuration

```eos
!
interface Management1
   description oob_management
   vrf MGMT
   ip address 10.73.255.122/24
```

# Authentication

# Monitoring

# Internal VLAN Allocation Policy

## Internal VLAN Allocation Policy Summary

**Default Allocation Policy**

| Policy Allocation | Range Beginning | Range Ending |
| ------------------| --------------- | ------------ |
| ascending | 1006 | 4094 |

# Interfaces

## Ethernet Interfaces

### Ethernet Interfaces Summary

#### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | Channel-Group |
| --------- | ----------- | ---- | ----- | ----------- | ----------- | ------------- |
| Ethernet3 | MLAG_PEER_DC1-LEAF1B_Ethernet3 | *trunk | *2-4094 | *- | *['LEAF_PEER_L3', 'MLAG'] | 3 |
| Ethernet4 | MLAG_PEER_DC1-LEAF1B_Ethernet4 | *trunk | *2-4094 | *- | *['LEAF_PEER_L3', 'MLAG'] | 3 |
| Ethernet5 | DC1-AGG01_Ethernet1 | *trunk | *110,201 | *- | *- | 5 |
| Ethernet50 | SRV-POD03_Eth1 | *trunk | *110,201 | *- | *- | 5 |

*Inherited from Port-Channel Interface

### Ethernet Interfaces Device Configuration

```eos
!
interface Ethernet3
   description MLAG_PEER_DC1-LEAF1B_Ethernet3
   channel-group 3 mode active
!
interface Ethernet4
   description MLAG_PEER_DC1-LEAF1B_Ethernet4
   channel-group 3 mode active
!
interface Ethernet5
   description DC1-AGG01_Ethernet1
   channel-group 5 mode active
!
interface Ethernet50
   description SRV-POD03_Eth1
   channel-group 5 mode active
```

## Port-Channel Interfaces

### Port-Channel Interfaces Summary

#### L2

| Interface | Description | Type | Mode | VLANs | Native VLAN | Trunk Group | LACP Fallback Timeout | LACP Fallback Mode | MLAG ID | EVPN ESI |
| --------- | ----------- | ---- | ---- | ----- | ----------- | ------------| --------------------- | ------------------ | ------- | -------- |
| Port-Channel3 | MLAG_PEER_DC1-LEAF1B_Po3 | switched | trunk | 2-4094 | - | ['LEAF_PEER_L3', 'MLAG'] | - | - | - | - |
| Port-Channel5 | DC1_L2LEAF1_Po1 | switched | trunk | 110,201 | - | - | - | - | 5 | - |
| Port-Channel50 | SRV-POD03_PortChanne1 | switched | trunk | 1-4000 | - | - | - | - | - | 0000:0000:0303:0202:0101 |
| Port-Channel51 | ipv6_prefix | switched | trunk | 1-500 | - | - | - | - | - | - |
| Port-Channel100.101 | IFL for TENANT01 | switched | access | - | - | - | - | - | - | - |
| Port-Channel100.102 | IFL for TENANT02 | switched | access | - | - | - | - | - | - | - |

### Port-Channel Interfaces Device Configuration

```eos
!
interface Port-Channel3
   description MLAG_PEER_DC1-LEAF1B_Po3
   switchport
   switchport trunk allowed vlan 2-4094
   switchport mode trunk
   switchport trunk group LEAF_PEER_L3
   switchport trunk group MLAG
!
interface Port-Channel5
   description DC1_L2LEAF1_Po1
   switchport
   switchport trunk allowed vlan 110,201
   switchport mode trunk
   mlag 5
   storm-control broadcast level 1
   storm-control multicast level 1
   storm-control unknown-unicast level 1
!
interface Port-Channel50
   description SRV-POD03_PortChanne1
   switchport
   switchport trunk allowed vlan 1-4000
   switchport mode trunk
   !
    evpn ethernet-segment
       identifier 0000:0000:0303:0202:0101
       route-target import 03:03:02:02:01:01
   !
   lacp system-id 0303.0202.0101
!
interface Port-Channel51
   description ipv6_prefix
   switchport
   switchport trunk allowed vlan 1-500
   switchport mode trunk
   ipv6 nd prefix a1::/64 infinite infinite no-autoconfig
!
interface Port-Channel100
   logging event link-status
   no switchport
!
interface Port-Channel100.101
   description IFL for TENANT01
   logging event link-status
   mtu 1500
   switchport
   ip address 10.1.1.3/31
!
interface Port-Channel100.102
   description IFL for TENANT02
   mtu 1500
   switchport
   vrf C2
   ip address 10.1.2.3/31
```

# Routing

## IP Routing

### IP Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | false|
### IP Routing Device Configuration

```eos
```
## IPv6 Routing

### IPv6 Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | false |

## Router BFD

### Router BFD Multihop Summary

| Interval | Minimum RX | Multiplier |
| -------- | ---------- | ---------- |
| 300 | 300 | 3 |

*No device configuration required - default values

# Multicast

# Filters

# ACL
