# Ansible Role: eos_cli_config_gen

**Table of Contents:**

- [Ansible Role: eos_cli_config_gen](#ansible-role-eoscliconfiggen)
  - [Overview](#overview)
  - [Role Inputs and Outputs](#role-inputs-and-outputs)
  - [Requirements](#requirements)
  - [Input Variables](#input-variables)
    - [Terminal Settings](#terminal-settings)
    - [Aliases](#aliases)
    - [Hardware Counters](#hardware-counters)
    - [Daemon TerminAttr](#daemon-terminattr)
    - [Internal VLAN Allocation Policy](#internal-vlan-allocation-policy)
    - [IP IGMP Snooping](#ip_igmp_snooping#)
    - [Event Monitor](#event-monitor)
    - [Load Interval](#load-interval)
    - [Queue Monitor Length](#queue-monitor-length)
    - [Logging](#logging)
    - [Domain Lookup](#domain-lookup)
    - [Name Servers](#name-servers)
    - [DNS Domain](#dns-domain)
    - [NTP Servers](#ntp-servers)
    - [Router L2 VPN](#router-l2-vpn)
    - [Sflow](#sflow)
    - [Redundancy](#redundancy)
    - [SNMP Settings](#snmp-settings)
    - [Spanning Tree](#spanning-tree)
    - [Platform](#platform)
    - [Tacacs+ Servers](#tacacs-servers)
    - [AAA Server Groups](#aaa-server-groups)
    - [AAA Authentication](#aaa-authentication)
    - [AAA Authorization](#aaa-authorization)
    - [AAA Accounting](#aaa-accounting)
    - [Local Users](#local-users)
    - [Clock Timezone](#clock-timezone)
    - [VLANs](#vlans)
    - [VRF Instances](#vrf-instances)
    - [Bfd Multihop Interval](#bfd-multihop-interval)
    - [Port-Channel Interfaces](#port-channel-interfaces)
    - [Ethernet Interfaces](#ethernet-interfaces)
    - [Loopback Interfaces](#loopback-interfaces)
    - [Management Interfaces](#management-interfaces)
    - [VLAN Interfaces](#vlan-interfaces)
    - [VxLAN Interface](#vxlan-interface)
    - [Hardware TCAM Profiles](#hardware-tcam-profiles)
    - [MAC Address-table](#mac-address-table)
    - [Router Virtual MAC Address](#router-virtual-mac-address)
    - [Virtual Source NAT](#virtual-source-nat)
    - [IPv6 Extended ACL](#ipv6-access-lists)
    - [IPv6 Standard ACL](#ipv6-standard-access-lists)
    - [Standard ACL](#standard-access-lists)
    - [Static Routes](#static-routes)
    - [IPv6 Static Routes](#ipv6-static-routes)
    - [Prefix Lists](#prefix-lists)
    - [MLAG Configuration](#mlag-configuration)
    - [Route Maps](#route-maps)
    - [Peer Filters](#peer-filters)
    - [Router BGP Configuration](#router-bgp-configuration)
    - [Router OSPF Configuration](#router-ospf-configuration)
    - [Queue Monitor Streaming](#queue-monitor-streaming)
    - [IP TACACS+ Source Interfaces](#ip-tacacs-source-interfaces)
    - [VM Tracer Sessions](#vmtracer-sessions)
    - [Banners](#banners)
    - [HTTP Management API](#http-management-api)
    - [Management Console](#management-console)
    - [Management Security](#management-security)
    - [Management SSH](#management-ssh)
  - [License](#license)

## Overview

**eos_cli_config_gen**, is a role that generates eos cli syntax and device documentation.

The **eos_cli_config_gen** role:

- Designed to generate the intended configuration offline, without relying on switch current state information.
- Facilitates the evaluation of the configuration prior to deployment with tools like [Batfish](https://www.batfish.org/)

## Role Inputs and Outputs

Figure 1 below provides a visualization of the roles inputs, and outputs and tasks in order executed by the role.

![Figure 1: Ansible Role eos_cli_config_gen](media/figure-1-role-eos_cli_config_gen.gif)

**Inputs:**

- Structured EOS configuration file in yaml format.

**Outputs:**

- EOS configuration in CLI format.
- Device Documentation in Markdown format.

**Tasks:**

1. Include device structured configuration that was previously generated.
2. Generate EOS configuration in CLI format.
3. Generate Device Documentation in Markdown format.

## Requirements

Requirements are located here: [avd-requirements](../../README.md#Requirements)

## Input Variables

- The input variables are documented inline within yaml formated output with: "< >"
- Variables are organized in order of how they appear in the CLI syntax.
- Available features  and variables may vary by platforms, refer to documentation on arista.com for specifics.
- All values are optional.

### Terminal Settings

```yaml
terminal:
  length: < 0-32767 >
  width: < 0-32767 >
```

### Aliases

```yaml
aliases: |
< list of alias commands in EOS CLI syntax >
```

### Hardware Counters

```yaml
hardware_counters:
  features:
    - <feature_1>: < direction | in | out >
    - <feature_1>: < direction | in | out >
```

### Daemon TerminAttr

```yaml
daemon_terminattr:
  ingestgrpcurl:
    ips:
      - < IPv4_address >
      - < IPv4_address >
      - < IPv4_address >
    port: < port_id >
  ingestauth_key: < ingest_key >
  ingestvrf: < vrf_name >
  smashexcludes: "< list as string >"
  ingestexclude: "< list as string >"

```

### Internal VLAN Allocation Policy

```yaml
vlan_internal_allocation_policy:
  allocation: < ascending | descending >
  range:
    beginning: < vlan_id >
    ending: < vlan_id >
```

### IP IGMP Snooping

```yaml
ip_igmp_snooping:
  vlans:
    < vlan_id >:
      enabled: < true | false >
```

### Event Monitor

```yaml
event_monitor:
  enabled: < true | false >
```

### Load Interval

```yaml
load_interval:
  default: < seconds >

```

### Queue Monitor Length

```yaml
queue_monitor_length:
  log: < seconds >
  notifying: < true | false >
```

### Logging

```yaml
logging:
  console: < severity_level >
  monitor: < severity_level >
  buffered:
    size: < 10-2147483647 representing number of messages >
    level: < severity_level >
  trap: < severity_level >
  source_interface: < source_interface_name >
  vrfs:
    < vrf_name >:
      source_interface: < source_interface_name >
      hosts:
        - < syslog_server_1>
        - < syslog_server_2>
```

### Domain Lookup

```yaml
ip_domain_lookup:
  source_interfaces:
    < source_interface_1 >:
      vrf: < vrf_name >
```

### Name Servers

```yaml
name_server:
  source:
    vrf: < vrf_name >
  nodes:
    - < name_server_1 >
    - < name_server_2 >
```

### DNS Domain

```yaml
dns_domain: < domain_name >
```

### NTP Servers

```yaml
ntp_server:
  local_interface:
    vrf: < vrf_name >
    interface: < source_interface >
  nodes:
    - < ntp_server_1 >
    - < ntp_server_2 >
```

### Router L2 VPN

```yaml
router_l2_vpn:
  nd_rs_flooding_disabled: < true | false >
  virtual_router_nd_ra_flooding_disabled: < true | false >
  arp_selective_install: < true | false >
  arp_proxy:
    prefix_list: < prefix_list_name >
```

### Sflow

```yaml
sflow:
  destinations:
    < sflow_destination_ip_1 >:
    < sflow_destination_ip_2 >:
  source_interface: < source_interface_name >
  sample: < sample_rate >
  run: < true | false >
```

### Redundancy

```yaml
Redundancy:
  protocol: < redundancy_protocol >
```

### Snmp Settings

```yaml
snmp_server:
  contact: < contact_name >
  location: < location >
  local_interfaces:
    - name: < interface_name_1 >
      vrf: < vrf_name >
    - name: < interface_name_2 >
  views:
    - name: < view_name >
      MIB_family_name: < MIB_family_name >
      included: < true | false >
    - name: < view_name >
      MIB_family_name: < MIB_family_name >
      included: < true | false >
  groups:
    - name: < group_name >
      version: < v1 | v2c | v3 >
      authentication: < auth | noauth | priv >
      read: < read_view >
      write: < write_view >
      notify: < notify_view >
    - name: < group_name >
      version: < v1 | v2c | v3 >
      authentication: < auth | noauth | priv >
      read: < read_view >
  users:
    - name: < username >
      group: < group_name >
      version: < v1 | v2c | v3 >
      auth: < md5 | sha | sha224 | sha256 | sha 384 | sha512 >
      auth_passphrase: < encrypted_auth_passphrase >
      priv: < aes | eas192 | aes256 | des >
      priv_passphrase: < encrypted_priv_passphrase >
    - name: < username >
      group: < group_name >
      version: < v1 | v2c | v3 >
  hosts:
    - host: < host IP address or name >
      vrf: < vrf_name >
      users:
        - username: < username >
          authentication_level: < auth | noauth | priv >
          version: < 1 | 2c | 3 >
    - host: < host IP address or name >
      vrf: < vrf_name >
      users:
        - username: < username >
          authentication_level: < auth | noauth | priv >
          version: < 1 | 2c | 3 >
  traps:
    enable: < true | false >
  vrfs:
    - name: < vrf_name >
      enable: < true | false >
    - name: < vrf_name >
      enable: < true | false >
```

### Spanning Tree

```yaml
spanning_tree:
  edge_port:
    bpduguard_default: < true | false >
  mode: < spanning_tree_mode >
  priority: < priority_level >
  no_spanning_tree_vlan: < vlan_id >, < vlan_id >-< vlan_id >
```

### Platform

```yaml
platform:
  trident:
    forwarding_table_partition: < partition >
```

### Tacacs+ Servers

```yaml
tacacs_servers:
  hosts:
    - host: < host1_ip_address >
      vrf: < vrf_name >
      key: < encypted_key >
    - host: < host2_ip_address >
      key: < encypted_key >
```

### AAA Server Groups

```yaml
aaa_server_groups:
  - name: < server_group_name >
    type: < tacacs+ | radius | ladp >
    servers:
      - < server_1_ip_address >
      - < server_2_ip_address >
  - name: < server_group_name >
    type: < tacacs+ | radius | ladp >
    servers:
      - < server_1_ip_address >
```

### AAA Authentication

```yaml
aaa_authentication:
  login:
    default: < group | local | none >
    serial_console: < group | local | none >
```

### AAA Authorization

```yaml
aaa_authorization:
  exec_default: < group | local | none >
  config_commands: < true | false >
```

### AAA Accounting

```yaml
aaa_accounting:
  exec:
    default:
      type: < none | start-stop | stop-only >
      group: < group_name >
  commands:
    commands_default:
      - commands: < all | 0-15 >
        type: < none | start-stop | stop-only >
        group: < group_name >
        logging: < true | false >
      - commands: < all | 0-15 >
        type: < none | start-stop | stop-only >
        logging: < true | false >
```

### Local Users

```yaml
local_users:
  < user_1 >:
    privilege: < 1-15 >
    role: < role >
    sha512_password: "< sha_512_password >"
  < user_2 >:
    privilege: < 1-15 >
    role: < role >
    sha512_password: "< sha_512_password >"
```

### Clock Timezone

```yaml
clock:
  timezone: < timezone >
```

### VLANs

```yaml
vlans:
  < vlan_id >:
    name: < vlan_name >
    trunk_groups:
      - < trunk_group_name_1 >
      - < trunk_group_name_2 >
  < vlan_id >:
    name: < vlan_name >
```

### VRF Instances

```yaml
vrfs:
  < vrf_name >:
    description: < description>
    ip_routing: < true | false >
    ipv6_routing: < true | false >
  < vrf_name >:
    description: < description>
    ip_routing: < true | false >
    ipv6_routing: < true | false >
```

### Bfd Multihop Interval

```yaml
bfd_multihop:
  interval: < rate in milliseconds >
  min_rx: < rate in milliseconds >
  multiplier: < 3-50 >
```

### Port-Channel Interfaces

```yaml
port_channel_interfaces:
  < Port-Channel_interface_1 >:
    description: < description >
    shutdown: < true | false >
    vlans: "< list of vlans as string >"
    mode: < access | dot1q-tunnel | trunk >
    mlag: < mlag_id >
    trunk_groups:
      - < trunk_group_name_1 >
      - < trunk_group_name_2 >
    qos:
      trust: < cos | dscp >
  < Port-Channel_interface_1 >:
    description: < description >
    vlans: "< list of vlans as string >"
    mode: < access | dot1q-tunnel | trunk >
    spanning_tree_bpdufilter: < true | false >
    spanning_tree_portfast: < portfast_mode >
    vmtracer: < true | false >
  < Port-Channel_interface_3 >:
    description: < description >
    mtu: < mtu >
    type: < switched | routed >
    ipv6_enable: < true | false >
    ipv6_address: < IPv6_address/mask >
    ipv6_address_link_local: < link_local_IPv6_address/mask >
    ipv6_nd_ra_disabled: < true | false >
    ipv6_nd_managed_config_flag: < true | false >
    ipv6_nd_prefixes:
      < IPv6_address_1/Mask >:
        valid_lifetime: < infinite or lifetime in seconds >
        preferred_lifetime: < infinite or lifetime in seconds >
        no_autoconfig_flag: < true | false >
      < IPv6_address_2/Mask >:
    pim:
      ipv4:
        sparse_mode: < true | false >
```

### Ethernet Interfaces

```yaml
# Routed Interfaces
ethernet_interfaces:
  <Ethernet_interface_1 >:
    description: < description >
    shutdown: < true | false >
    speed: < interface_speed >
    mtu: < mtu >
    type: < routed | switched >
    vrf: < vrf_name >
    ip_address: < IPv4_address/Mask >
    ipv6_enable: < true | false >
    ipv6_address: < IPv6_address/Mask >
    ipv6_address_link_local: < link_local_IPv6_address/Mask >
    ipv6_nd_ra_disabled: < true | false >
    ipv6_nd_managed_config_flag: < true | false >
    ipv6_nd_prefixes:
      < IPv6_address_1/Mask >:
        valid_lifetime: < infinite or lifetime in seconds >
        preferred_lifetime: < infinite or lifetime in seconds >
        no_autoconfig_flag: < true | false >
      < IPv6_address_2/Mask >:
    ospf_network_point_to_point: < true | false >
    ospf_area: < ospf_area >
    pim:
      ipv4:
        sparse_mode: < true | false >

# Switched Interfaces
  <Ethernet_interface_2 >:
    description: < description >
    shutdown: < true | false >
    speed: < interface_speed >
    mtu: < mtu >
    vlans: "< list of vlans as string >"
    mode: < access | dot1q-tunnel | trunk >
    flowcontrol:
      received: < received | send | on >
    channel_group:
      id: < Port-Channel_id >
      mode: < on | active | passive >
    qos:
      trust: < cos | dscp >
    spanning_tree_bpdufilter: < true | false >
    spanning_tree_portfast: < portfast_mode >
    vmtracer: < true | false >
```

### Loopback Interfaces

```yaml
loopback_interfaces:
  < Loopback_interface_1 >:
    description: < description >
    shutdown: < true | false >
    vrf: < vrf_name >
    ip_address: < IPv4_address/Mask >
    ipv6_enable: < true | false >
    ipv6_address: < IPv6_address/Mask >
    ospf_area: < ospf_area >
  < Loopback_interface_2 >:
    description: < description >
    ip_address: < IPv4_address/Mask >
```

### Management Interfaces

```yaml
management_interfaces:
  < Management_interface_1 >:
    description: < description >
    vrf: < vrf_name >
    ip_address: < IPv4_address/Mask >
    ipv6_enable: < true | false >
    ipv6_address: < IPv6_address/Mask >
    gateway: <IPv4 address of gateway>
    ipv6_gateway: <IPv6 address of gateway>
```

### VLAN Interfaces

```yaml
vlan_interfaces:
  < Vlan_id_1 >:
    description: < description >
    shutdown: < true | false >
    vrf: < vrf_name >
    ip_address: < IPv4_address/Mask >
    ip_address_secondary: < IPv4_address/Mask >
    virtual: < true | false >
    mtu: < mtu >
    ip_helpers:
      < ip_helper_address_1 >:
        source_interface: < source_interface_name >
        vrf: < vrf_name >
      < ip_helper_address_2 >:
        source_interface: < source_interface_name >
    ipv6_enable: < true | false >
    ipv6_address: < IPv6_address/Mask >
    ipv6_address_link_local: < link_local_IPv6_address/Mask >
    ipv6_nd_ra_disabled: < true | false >
    ipv6_nd_managed_config_flag: < true | false >
    ipv6_nd_prefixes:
      < IPv6_address_1/Mask >:
        valid_lifetime: < infinite or lifetime in seconds >
        preferred_lifetime: < infinite or lifetime in seconds >
        no_autoconfig_flag: < true | false >
      < IPv6_address_2/Mask >:
    multicast:
      ipv4:
        source_route_export:
          enabled: < true | false >
          administrative_distance: < 1-255 >
    ospf_network_point_to_point: < true | false >
    ospf_area: < ospf_area >
    pim:
      ipv4:
        sparse_mode: < true | false >
        local_interface: < local_interface_name >
    ipv6_virtual_router_address: < IPv6_address >
  < Vlan_id_2 >:
    description: < description >
    ip_address: < IPv4_address/Mask >
```

### VxLAN Interface

```yaml
vxlan_tunnel_interface:
  Vxlan1:
    description: < description >
    source_interface: < source_interface_name >
    virtual_router:
      encapsulation_mac_address: < mlag-system-id | ethernet_address (H.H.H) >
    vxlan_udp_port: < udp_port >
    vxlan_vni_mappings:
      vlans:
        < vlan_id_1 >:
          vni: < vni_id_1 >
        < vlan_id_2 >:
          vni: < vni_id_2 >
      vrfs:
        < vrf_name >:
          vni: < vni_id_3 >
        < vrf_name >:
          vni: < vni_id_4 >
```

### Hardware TCAM Profiles

```yaml
tcam_profile:
  - < tcam_profile >
```

### MAC Address-table

```yaml
mac_address_table:
  aging_time: < aging_time_in_seconds >
```

### Router Virtual MAC Address

```yaml
ip_virtual_router_mac_address: < mac_address (hh:hh:hh:hh:hh:hh) >
```

### Virtual Source NAT

```yaml
virtual_source_nat_vrfs:
  < vrf_name_1 >:
    ip_address: < IPv4_address >
  < vrf_name_2 >:
    ip_address: < IPv4_address >
```

### IPv6 Extended Access-Lists

```yaml
ipv6_access_lists:
  < ipv6_access_list_name_1 >:
    sequence_numbers:
      < sequence_id_1 >:
        action: "< action as string >"
      < sequence_id_2 >:
        action: "< action as string >"
  < ipv6_access_list_name_2 >:
    sequence_numbers:
      < sequence_id_1 >:
        action: "< action as string >"
```

### IPv6 Standard Access-Lists

```yaml
ipv6_standard_access_lists:
  < ipv6_access_list_name_1 >:
    sequence_numbers:
      < sequence_id_1 >:
        action: "< action as string >"
      < sequence_id_2 >:
        action: "< action as string >"
  < ipv6_access_list_name_2 >:
    sequence_numbers:
      < sequence_id_1 >:
        action: "< action as string >"
```

### IP Standard Access-Lists

```yaml
standard_access_lists:
  < access_list_name_1 >:
    sequence_numbers:
      < sequence_id_1 >:
        action: "< action as string >"
      < sequence_id_2 >:
        action: "< action as string >"
  < access_list_name_2 >:
    sequence_numbers:
      < sequence_id_1 >:
        action: "< action as string >"
```

### Static Routes

```yaml
static_routes:
  - vrf: < vrf_name, if vrf_name = default the route will be placed in the GRT >
    destination_address_prefix: < IPv4_network/Mask >
    gateway: < IPv4_address >
    distance: < 1-255 >
    tag: < 0-4294967295 >
    name: < description >
  - destination_address_prefix: < IPv4_network/Mask >
    gateway: < IPv4_address >
```

### IPv6 Static Routes

```yaml
ipv6_static_routes:
  - vrf: < vrf_name, if vrf_name = default the route will be placed in the GRT >
    destination_address_prefix: < IPv6_network/Mask >
    gateway: < IPv6_address >
    distance: < 1-255 >
    tag: < 0-4294967295 >
    name: < description >
  - destination_address_prefix: < IPv6_network/Mask >
    gateway: < IPv6_address >
```

### Prefix Lists

```yaml
prefix_lists:
  < prefix_list_name_1 >:
    sequence_numbers:
      < sequence_id_1 >:
        action: "< action as string >"
      < sequence_id_2 >:
        action: "< action as string >"
  < prefix_list_name_2 >:
    sequence_numbers:
      < sequence_id_1 >:
        action: "< action as string >"
```

### IPv6 Prefix Lists

```yaml
ipv6_prefix_lists:
  < ipv6_prefix_list_name_1 >:
    sequence_numbers:
      < sequence_id_1 >:
        action: "< action as string >"
      < sequence_id_2 >:
        action: "< action as string >"
  < ipv6_prefix_list_name_2 >:
    sequence_numbers:
      < sequence_id_1 >:
        action: "< action as string >"
```

### MLAG Configuration

```yaml
mlag_configuration:
  domain_id: < domain_id_name >
  local_interface: < interface_name >
  peer_address: < IPv4_address >
  peer_address_heartbeat:
    peer_ip: < IPv4_address >
    vrf: < vrf_name >
  dual_primary_detection_delay: < seconds >
  peer_link: < Port-Channel_id >
  reload_delay_mlag: < seconds >
  reload_delay_non_mlag: < seconds >
```

### Route Maps

```yaml
route_maps:
  < route_map_name_1 >:
    sequence_numbers:
      < sequence_id_1 >:
        type: < permit | deny >
        description: < description >
        match: "< match as string >"
        set: "< set as string >"
      < sequence_id_2 >:
        type: < permit | deny >
        match: "< match as string >"
  < route_map_name_2 >:
    sequence_numbers:
      < sequence_id_1 >:
        type: < permit | deny >
        description: < description >
        set: "< set as string >"
```

### Peer Filters

```yaml
peer_filters:
  < peer_filter_name_1:
    sequence_numbers:
      < sequence_id_1 >:
        match: "< match as string >"
      < sequence_id_2 >:
        match: "< match as string >"
  < peer_filter_name_2:
    sequence_numbers:
      < sequence_id_1 >:
        match: "< match as string >"
```

### Router BGP Configuration

```yaml
router_bgp:
  as: < bgp_as >
  router_id: < IPv4_address >
  bgp_defaults:
    - "< bgp command as string >"
    - "< bgp command as string >"
  peer_groups:
    < peer_group_name_1>:
      type: < ipv4 | evpn >
      peer_filter: < peer_filter >
      next_hop_unchanged: < true | false >
      update_source: < interface >
      bfd: < true | false >
      ebgp_multihop: < integer >
      next_hop_self: < true | false >
      password: "< encrypted_password >"
      send_community: < true | false >
      maximum_routes: < integer >
    < peer_group_name_2 >:
      type: < ipv4 | evpn >
      bgp_listen_range_prefix: < IP prefix range >
      peer_filter: < peer_filter >
      password: "< encrypted_password >"
      maximum_routes: < integer >
  neighbors:
    < IPv4_address_1 >:
      peer_group: < peer_group_name >
      remote_as: < bgp_as >
    < IPv4_address_2 >:
      remote_as: < bgp_as >
      password: "< encrypted_password >"
  redistribute_routes:
    < route_type >:
      route_map: < route_map_name >
    < route_type >:
      route_map: < route_map_name >
  vlan_aware_bundles:
    < vlan_aware_bundle_name_1 >:
      rd: "< route distinguisher >"
      route_targets:
        both:
          - "< route_target >"
        import:
          - "< route_target >"
          - "< route_target >"
        export:
          - "< route_target >"
          - "< route_target >"
      redistribute_routes:
        - < learned >
      vlan: < vlan_range >
    < vlan_aware_bundle_name_2 >:
      rd: "< route distinguisher >"
      route_targets:
        both:
          - "< route_target >"
        import:
          - "< route_target >"
          - "< route_target >"
        export:
          - "< route_target >"
          - "< route_target >"
      redistribute_routes:
        - < connected >
        - < learned >
      vlan: < vlan_range >
  vlans:
    < vlan_id_1>:
      rd: "< route distinguisher >"
      route_targets:
        both:
          - "< route_target >"
      redistribute_routes:
        - < connected >
        - < learned >
    <vlan_id_2 >:
      rd: "< route distinguisher >"
      route_targets:
        import:
          - "< route_target >"
          - "< route_target >"
        export:
          - "< route_target >"
          - "< route_target >"
      redistribute_routes:
        - < connected >
        - < learned >
  address_family_evpn:
    peer_groups:
      < peer_group_name >:
        activate: < true | false >
  address_family_ipv4:
    peer_groups:
      < peer_group_name >:
        route_map_in: < route_map_name >
        route_map_out: < route_map_name >
        activate: < true | false >
      < peer_group_name >:
        activate: < true | false >
    neighbors:
      < neighbor_ip_address>:
        activate: < true | false >
      < neighbor_ip_address>:
        activate: < true | false >
  address_family_ipv4_multicast:
    peer_groups:
      < peer_group_name >:
        activate: < true | false >
      < peer_group_name >:
        activate: < true | false >
    neighbors:
      < neighbor_ip_address>:
    redistribute_routes:
      < route_type >:
  address_family_ipv6:
    peer_groups:
      < peer_group_name >:
        activate: < true | false >
        route_map_in: < route_map_name >
        route_map_out: < route_map_name >
      < peer_group_name >:
        activate: true
    neighbors:
      < neighbor_ip_address>:
        route_map_in: < route_map_name >
        route_map_out: < route_map_name >
        activate: < true | false >
    redistribute_routes:
      < route_type >:
        route_map: < route_map_name >
      < route_type >:
        route_map: < route_map_name >
  vrfs:
    < vrf_name_1 >:
      rd: "< route distinguisher >"
      route_targets:
        import:
          < address_family >:
            - "< route_target >"
            - "< route_target >"
          < address_family >:
            - "< route_target >"
            - "< route_target >"
        export:
          < address_family >:
            - "< route_target >"
            - "< route_target >"
        neighbors:
          < neighbor_ip_address >:
            remote_as: < asn >
          < neighbor_ip_address >:
            remote_as: < asn >
      redistribute_routes:
        < route_type >:
          route_map: < route_map_name >
        < route_type >:
          route_map: < route_map_name >
    < vrf_name_2 >:
      rd: "<route distinguisher >"
      route_targets:
        import:
          < address_family >:
            - "< route_target >"
            - "< route_target >"
          < address_family >:
            - "< route_target >"
            - "< route_target >"
        export:
          < address_family >:
            - "< route_target >"
            - "< route_target >"
      redistribute_routes:
        < route_type >:
          route_map: < route_map_name >
        < route_type >:
          route_map: < route_map_name >
```

### Routing - Multicast

```yaml
router_multicast:
  ipv4:
    routing: < true | false >
```

### Router OSPF Configuration

```yaml
router_ospf:
  process_ids:
    < process_id >:
      passive_interface_default: < true | false >
      router_id: < IPv4_address >
      no_passive_interfaces:
        - < interface_1 >
        - < interface_2 >
      max_lsa: < integer >
```

### Routing PIM Sparse Mode

```yaml
router_pim_sparse_mode:
  ipv4:
    rp_addresses:
      < rp_address_1 >:
        groups:
          < group_prefix_1/mask > :
          < group_prefix_2/mask > :
      < rp_address_2 >:
    anycast_rps:
      < anycast_rp_address_1 > :
        other_anycast_rp_addresses:
          < ip_address_other_anycast_rp_1 >:
            register_count: < register_count_nb >
```

### Queue Monitor Streaming

```yaml
queue_monitor_streaming:
  enable: < true | false >
```

### IP TACACS+ Source Interfaces

```yaml
ip_tacacs_source_interfaces:
    - name: <interface_name_1 >
      vrf: < vrf_name_1 >
    - name: <interface_name_2 >
```

### VM Tracer Sessions

```yaml
vmtracer_sessions:
  < vmtracer_session_name_1 >:
    url: < url >
    username: < username >
    password: < encrypted_password >
    autovlan_disable: < true | false >
    source_interface: < interface_name >
  < vmtracer_session_name_2 >:
    url: < url >
    username: < username >
    password: < encrypted_password >
```

### Banners

```yaml
banners:
  login: |
    < text ending with EOF >
  motd: |
    < text ending with EOF >
```

### HTTP Management API

```yaml
management_api_http:
  enable_http: < true | false >
  enable_https: < true | false >
  enable_vrfs:
    < vrf_name_1 >:
      access_group: < Standard IPv4 ACL name >
    < vrf_name_2 >:
```

### Management Console

```yaml
management_console:
  idle_timeout: < 0-86400 in minutes >
```

### Management Security

```yaml
management_security:
  password:
    encryption_key_common : < true | false >
```

### Management SSH

```yaml
management_ssh:
  access_groups:
    < standard_acl_name_1 >:
    < standard_acl_name_2 >:
      vrf: < vrf name >
  idle_timeout: < 0-86400 in minutes >
  enable: < true | false >
  vrfs:
    < vrf_name_1 >:
      enable: < true | false >
    < vrf_name_2 >:
      enable: < true | false >
```

## License

Project is published under [Apache 2.0 License](../../LICENSE)
