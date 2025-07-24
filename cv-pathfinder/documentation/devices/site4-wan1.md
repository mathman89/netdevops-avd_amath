# site4-wan1

## Table of Contents

- [Management](#management)
  - [Agents](#agents)
  - [Management Interfaces](#management-interfaces)
  - [DNS Domain](#dns-domain)
  - [IP Name Servers](#ip-name-servers)
  - [Domain Lookup](#domain-lookup)
  - [NTP](#ntp)
  - [Management API HTTP](#management-api-http)
- [Authentication](#authentication)
  - [Local Users](#local-users)
  - [Enable Password](#enable-password)
  - [AAA Authorization](#aaa-authorization)
- [Management Security](#management-security)
  - [Management Security Summary](#management-security-summary)
  - [Management Security SSL Profiles](#management-security-ssl-profiles)
  - [SSL profile STUN-DTLS Certificates Summary](#ssl-profile-stun-dtls-certificates-summary)
  - [Management Security Device Configuration](#management-security-device-configuration)
- [Monitoring](#monitoring)
  - [TerminAttr Daemon](#terminattr-daemon)
  - [Flow Tracking](#flow-tracking)
- [Spanning Tree](#spanning-tree)
  - [Spanning Tree Summary](#spanning-tree-summary)
  - [Spanning Tree Device Configuration](#spanning-tree-device-configuration)
- [IP Security](#ip-security)
  - [IKE policies](#ike-policies)
  - [Security Association policies](#security-association-policies)
  - [IPSec profiles](#ipsec-profiles)
  - [Key controller](#key-controller)
  - [IP Security Device Configuration](#ip-security-device-configuration)
- [Interfaces](#interfaces)
  - [DPS Interfaces](#dps-interfaces)
  - [Ethernet Interfaces](#ethernet-interfaces)
  - [Loopback Interfaces](#loopback-interfaces)
  - [VXLAN Interface](#vxlan-interface)
- [Routing](#routing)
  - [Service Routing Protocols Model](#service-routing-protocols-model)
  - [IP Routing](#ip-routing)
  - [IPv6 Routing](#ipv6-routing)
  - [Static Routes](#static-routes)
  - [Router Adaptive Virtual Topology](#router-adaptive-virtual-topology)
  - [Router Traffic-Engineering](#router-traffic-engineering)
  - [Router BGP](#router-bgp)
- [BFD](#bfd)
  - [Router BFD](#router-bfd)
- [Filters](#filters)
  - [Prefix-lists](#prefix-lists)
  - [Route-maps](#route-maps)
  - [IP Extended Community Lists](#ip-extended-community-lists)
- [ACL](#acl)
  - [IP Access-lists](#ip-access-lists)
- [VRF Instances](#vrf-instances)
  - [VRF Instances Summary](#vrf-instances-summary)
  - [VRF Instances Device Configuration](#vrf-instances-device-configuration)
- [Application Traffic Recognition](#application-traffic-recognition)
  - [Applications](#applications)
  - [Application Profiles](#application-profiles)
  - [Field Sets](#field-sets)
  - [Router Application-Traffic-Recognition Device Configuration](#router-application-traffic-recognition-device-configuration)
  - [Router Path-selection](#router-path-selection)
- [STUN](#stun)
  - [STUN Client](#stun-client)
  - [STUN Device Configuration](#stun-device-configuration)

## Management

### Agents

#### Agent KernelFib

##### Environment Variables

| Name | Value |
| ---- | ----- |
| KERNELFIB_PROGRAM_ALL_ECMP | 1 |

#### Agents Device Configuration

```eos
!
agent KernelFib environment KERNELFIB_PROGRAM_ALL_ECMP=1
```

### Management Interfaces

#### Management Interfaces Summary

##### IPv4

| Management Interface | Description | Type | VRF | IP Address | Gateway |
| -------------------- | ----------- | ---- | --- | ---------- | ------- |
| Management1 | OOB_MANAGEMENT | oob | MGMT | 192.168.17.22/24 | 192.168.17.1 |

##### IPv6

| Management Interface | Description | Type | VRF | IPv6 Address | IPv6 Gateway |
| -------------------- | ----------- | ---- | --- | ------------ | ------------ |
| Management1 | OOB_MANAGEMENT | oob | MGMT | - | - |

#### Management Interfaces Device Configuration

```eos
!
interface Management1
   description OOB_MANAGEMENT
   no shutdown
   vrf MGMT
   ip address 192.168.17.22/24
   no lldp transmit
   no lldp receive
```

### DNS Domain

DNS domain: wan.example.local

#### DNS Domain Device Configuration

```eos
dns domain wan.example.local
!
```

### IP Name Servers

#### IP Name Servers Summary

| Name Server | VRF | Priority |
| ----------- | --- | -------- |
| 192.168.17.1 | MGMT | - |

#### IP Name Servers Device Configuration

```eos
ip name-server vrf MGMT 192.168.17.1
```

### Domain Lookup

#### DNS Domain Lookup Summary

| Source interface | vrf |
| ---------------- | --- |
| Management1 | MGMT |

#### DNS Domain Lookup Device Configuration

```eos
ip domain lookup vrf MGMT source-interface Management1
```

### NTP

#### NTP Summary

##### NTP Local Interface

| Interface | VRF |
| --------- | --- |
| Management1 | MGMT |

##### NTP Servers

| Server | VRF | Preferred | Burst | iBurst | Version | Min Poll | Max Poll | Local-interface | Key |
| ------ | --- | --------- | ----- | ------ | ------- | -------- | -------- | --------------- | --- |
| 0.pool.ntp.org | MGMT | True | - | - | - | - | - | - | - |

#### NTP Device Configuration

```eos
!
ntp local-interface vrf MGMT Management1
ntp server vrf MGMT 0.pool.ntp.org prefer
```

### Management API HTTP

#### Management API HTTP Summary

| HTTP | HTTPS | UNIX-Socket | Default Services |
| ---- | ----- | ----------- | ---------------- |
| False | True | - | - |

#### Management API VRF Access

| VRF Name | IPv4 ACL | IPv6 ACL |
| -------- | -------- | -------- |
| MGMT | - | - |

#### Management API HTTP Device Configuration

```eos
!
management api http-commands
   protocol https
   no shutdown
   !
   vrf MGMT
      no shutdown
```

## Authentication

### Local Users

#### Local Users Summary

| User | Privilege | Role | Disabled | Shell |
| ---- | --------- | ---- | -------- | ----- |
| admin | 15 | network-admin | False | - |
| arista | 15 | network-admin | False | - |
| cvpadmin | 15 | network-admin | False | - |

#### Local Users Device Configuration

```eos
!
username admin privilege 15 role network-admin nopassword
username arista privilege 15 role network-admin secret sha512 <removed>
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

## Management Security

### Management Security Summary

| Settings | Value |
| -------- | ----- |

### Management Security SSL Profiles

| SSL Profile Name | TLS protocol accepted | Certificate filename | Key filename | Ciphers | CRLs | FIPS restrictions enabled |
| ---------------- | --------------------- | -------------------- | ------------ | ------- | ---- | ------------------------- |
| STUN-DTLS | 1.2 | STUN-DTLS.crt | STUN-DTLS.key | - | - | - |

### SSL profile STUN-DTLS Certificates Summary

| Trust Certificates | Requirement | Policy | System |
| ------------------ | ----------- | ------ | ------ |
| aristaDeviceCertProvisionerDefaultRootCA.crt | - | - | - |

### Management Security Device Configuration

```eos
!
management security
   !
   ssl profile STUN-DTLS
      tls versions 1.2
      trust certificate aristaDeviceCertProvisionerDefaultRootCA.crt
      certificate STUN-DTLS.crt key STUN-DTLS.key
```

## Monitoring

### TerminAttr Daemon

#### TerminAttr Daemon Summary

| CV Compression | CloudVision Servers | VRF | Authentication | Smash Excludes | Ingest Exclude | Bypass AAA |
| -------------- | ------------------- | --- | -------------- | -------------- | -------------- | ---------- |
| gzip | apiserver.arista.io:443 | MGMT | token-secure,/tmp/cv-onboarding-token | ale,flexCounter,hardware,kni,pulse,strata | - | False |

#### TerminAttr Daemon Device Configuration

```eos
!
daemon TerminAttr
   exec /usr/bin/TerminAttr -cvaddr=apiserver.arista.io:443 -cvauth=token-secure,/tmp/cv-onboarding-token -cvvrf=MGMT -smashexcludes=ale,flexCounter,hardware,kni,pulse,strata -taillogs -cvsourceintf=Management1
   no shutdown
```

### Flow Tracking

#### Flow Tracking Hardware

##### Trackers Summary

| Tracker Name | Record Export On Inactive Timeout | Record Export On Interval | Number of Exporters | Applied On |
| ------------ | --------------------------------- | ------------------------- | ------------------- | ---------- |
| FLOW-TRACKER | 70000 | 5000 | 1 | Dps1<br>Ethernet1<br>Ethernet2<br>Ethernet3<br>Ethernet4 |

##### Exporters Summary

| Tracker Name | Exporter Name | Collector IP/Host | Collector Port | Local Interface |
| ------------ | ------------- | ----------------- | -------------- | --------------- |
| FLOW-TRACKER | CV-TELEMETRY | - | - | Loopback0 |

#### Flow Tracking Device Configuration

```eos
!
flow tracking hardware
   tracker FLOW-TRACKER
      record export on inactive timeout 70000
      record export on interval 5000
      exporter CV-TELEMETRY
         collector 127.0.0.1
         local interface Loopback0
   no shutdown
```

## Spanning Tree

### Spanning Tree Summary

STP mode: **none**

### Spanning Tree Device Configuration

```eos
!
spanning-tree mode none
```

## IP Security

### IKE policies

| Policy name | IKE lifetime | Encryption | DH group | Local ID | Integrity |
| ----------- | ------------ | ---------- | -------- | -------- | --------- |
| CP-IKE-POLICY | - | - | - | 192.168.42.15 | - |

### Security Association policies

| Policy name | ESP Integrity | ESP Encryption | Lifetime | PFS DH Group |
| ----------- | ------------- | -------------- | -------- | ------------ |
| CP-SA-POLICY | - | aes256gcm128 | - | 14 |
| DP-SA-POLICY | - | aes256gcm128 | - | 14 |

### IPSec profiles

| Profile name | IKE policy | SA policy | Connection | DPD Interval | DPD Time | DPD action | Mode | Flow Parallelization |
| ------------ | ---------- | ----------| ---------- | ------------ | -------- | ---------- | ---- | -------------------- |
| CP-PROFILE | CP-IKE-POLICY | CP-SA-POLICY | start | - | - | - | transport | - |
| DP-PROFILE | - | DP-SA-POLICY | start | - | - | - | transport | - |

### Key controller

| Profile name |
| ------------ |
| DP-PROFILE |

### IP Security Device Configuration

```eos
!
ip security
   ike policy CP-IKE-POLICY
      local-id 192.168.42.15
   !
   sa policy CP-SA-POLICY
      esp encryption aes256gcm128
      pfs dh-group 14
   !
   sa policy DP-SA-POLICY
      esp encryption aes256gcm128
      pfs dh-group 14
   !
   profile CP-PROFILE
      ike-policy CP-IKE-POLICY
      sa-policy CP-SA-POLICY
      connection start
      shared-key 7 <removed>
      dpd 10 50 clear
      mode transport
   !
   profile DP-PROFILE
      sa-policy DP-SA-POLICY
      connection start
      shared-key 7 <removed>
      dpd 10 50 clear
      mode transport
   !
   key controller
      profile DP-PROFILE
```

## Interfaces

### DPS Interfaces

#### DPS Interfaces Summary

| Interface | IP address | Shutdown | MTU | Flow tracker(s) | TCP MSS Ceiling |
| --------- | ---------- | -------- | --- | --------------- | --------------- |
| Dps1 | 192.168.42.15/32 | - | 9194 | Hardware: FLOW-TRACKER |  |

#### DPS Interfaces Device Configuration

```eos
!
interface Dps1
   description DPS Interface
   mtu 9194
   flow tracker hardware FLOW-TRACKER
   ip address 192.168.42.15/32
```

### Ethernet Interfaces

#### Ethernet Interfaces Summary

##### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | Channel-Group |
| --------- | ----------- | ---- | ----- | ----------- | ----------- | ------------- |

*Inherited from Port-Channel Interface

##### IPv4

| Interface | Description | Channel Group | IP Address | VRF |  MTU | Shutdown | ACL In | ACL Out |
| --------- | ----------- | ------------- | ---------- | ----| ---- | -------- | ------ | ------- |
| Ethernet1 | P2P_site4-border1_Ethernet3 | - | 10.0.4.57/31 | default | 9194 | False | - | - |
| Ethernet2 | P2P_site4-border2_Ethernet3 | - | 10.0.4.59/31 | default | 9194 | False | - | - |
| Ethernet3 | ACME-MPLS-INC_mpls-site4-wan1_mpls-cloud_Ethernet8 | - | 172.18.40.2/24 | default | - | False | - | - |
| Ethernet4 | REGION1-INTERNET-CORP_inet-site4-wan1_inet-cloud_Ethernet10 | - | 100.64.40.2/24 | default | - | False | ACL-INTERNET-IN_Ethernet4 | - |

#### Ethernet Interfaces Device Configuration

```eos
!
interface Ethernet1
   description P2P_site4-border1_Ethernet3
   no shutdown
   mtu 9194
   no switchport
   flow tracker hardware FLOW-TRACKER
   ip address 10.0.4.57/31
!
interface Ethernet2
   description P2P_site4-border2_Ethernet3
   no shutdown
   mtu 9194
   no switchport
   flow tracker hardware FLOW-TRACKER
   ip address 10.0.4.59/31
!
interface Ethernet3
   description ACME-MPLS-INC_mpls-site4-wan1_mpls-cloud_Ethernet8
   no shutdown
   no switchport
   flow tracker hardware FLOW-TRACKER
   ip address 172.18.40.2/24
!
interface Ethernet4
   description REGION1-INTERNET-CORP_inet-site4-wan1_inet-cloud_Ethernet10
   no shutdown
   no switchport
   flow tracker hardware FLOW-TRACKER
   ip address 100.64.40.2/24
   ip access-group ACL-INTERNET-IN_Ethernet4 in
```

### Loopback Interfaces

#### Loopback Interfaces Summary

##### IPv4

| Interface | Description | VRF | IP Address |
| --------- | ----------- | --- | ---------- |
| Loopback0 | ROUTER_ID | default | 192.168.255.15/32 |

##### IPv6

| Interface | Description | VRF | IPv6 Address |
| --------- | ----------- | --- | ------------ |
| Loopback0 | ROUTER_ID | default | - |

#### Loopback Interfaces Device Configuration

```eos
!
interface Loopback0
   description ROUTER_ID
   no shutdown
   ip address 192.168.255.15/32
```

### VXLAN Interface

#### VXLAN Interface Summary

| Setting | Value |
| ------- | ----- |
| Source Interface | Dps1 |
| UDP port | 4789 |

##### VRF to VNI and Multicast Group Mappings

| VRF | VNI | Multicast Group |
| ---- | --- | --------------- |
| BLUE | 100 | - |
| default | 1 | - |
| RED | 101 | - |

#### VXLAN Interface Device Configuration

```eos
!
interface Vxlan1
   description site4-wan1_VTEP
   vxlan source-interface Dps1
   vxlan udp-port 4789
   vxlan vrf BLUE vni 100
   vxlan vrf default vni 1
   vxlan vrf RED vni 101
```

## Routing

### Service Routing Protocols Model

Multi agent routing protocol model enabled

```eos
!
service routing protocols model multi-agent
```

### IP Routing

#### IP Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | True |
| BLUE | True |
| MGMT | False |
| RED | True |

#### IP Routing Device Configuration

```eos
!
ip routing
ip routing vrf BLUE
no ip routing vrf MGMT
ip routing vrf RED
```

### IPv6 Routing

#### IPv6 Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | False |
| BLUE | false |
| MGMT | false |
| RED | false |

### Static Routes

#### Static Routes Summary

| VRF | Destination Prefix | Next Hop IP | Exit interface | Administrative Distance | Tag | Route Name | Metric |
| --- | ------------------ | ----------- | -------------- | ----------------------- | --- | ---------- | ------ |
| MGMT | 0.0.0.0/0 | 192.168.17.1 | - | 1 | - | - | - |
| default | 172.18.0.0/16 | 172.18.40.1 | - | 1 | - | - | - |
| default | 100.64.0.0/16 | 100.64.40.1 | - | 1 | - | - | - |

#### Static Routes Device Configuration

```eos
!
ip route 100.64.0.0/16 100.64.40.1
ip route 172.18.0.0/16 172.18.40.1
ip route vrf MGMT 0.0.0.0/0 192.168.17.1
```

### Router Adaptive Virtual Topology

#### Router Adaptive Virtual Topology Summary

Topology role: transit region

VXLAN gateway: Enabled

| Hierarchy | Name | ID |
| --------- | ---- | -- |
| Region | REGION1 | 1 |
| Zone | REGION1-ZONE | 1 |
| Site | SITE4 | 102 |

#### AVT Profiles

| Profile name | Load balance policy | Internet exit policy | Metric Order | Jitter Threshold (ms) | Latency Threshold (ms) | Load (%) | Loss Rate (%) |
| ------------ | ------------------- | -------------------- | ------------ | --------------------- | ---------------------- | -------- | ------------- |
| BLUE-POLICY-DEFAULT | LB-BLUE-POLICY-DEFAULT | - | - | - | - | - | - |
| BLUE-POLICY-VIDEO | LB-BLUE-POLICY-VIDEO | - | - | - | - | - | - |
| BLUE-POLICY-VOICE | LB-BLUE-POLICY-VOICE | - | - | - | - | - | - |
| DEFAULT-POLICY-CONTROL-PLANE | LB-DEFAULT-POLICY-CONTROL-PLANE | - | - | - | - | - | - |
| DEFAULT-POLICY-DEFAULT | LB-DEFAULT-POLICY-DEFAULT | - | - | - | - | - | - |
| RED-POLICY-CRITICAL-SECRET-DATA | LB-RED-POLICY-CRITICAL-SECRET-DATA | - | - | - | - | - | - |
| RED-POLICY-NORMAL-DATA | LB-RED-POLICY-NORMAL-DATA | - | - | - | - | - | - |
| RED-POLICY-NOT-SO-IMPORTANT-DATA | LB-RED-POLICY-NOT-SO-IMPORTANT-DATA | - | - | - | - | - | - |

#### AVT Policies

##### AVT policy BLUE-POLICY

| Application profile | AVT Profile | Traffic Class | DSCP |
| ------------------- | ----------- | ------------- | ---- |
| VIDEO | BLUE-POLICY-VIDEO | - | - |
| VOICE | BLUE-POLICY-VOICE | - | 46 |
| default | BLUE-POLICY-DEFAULT | - | - |

##### AVT policy DEFAULT-POLICY-WITH-CP

| Application profile | AVT Profile | Traffic Class | DSCP |
| ------------------- | ----------- | ------------- | ---- |
| APP-PROFILE-CONTROL-PLANE | DEFAULT-POLICY-CONTROL-PLANE | - | - |
| default | DEFAULT-POLICY-DEFAULT | - | - |

##### AVT policy RED-POLICY

| Application profile | AVT Profile | Traffic Class | DSCP |
| ------------------- | ----------- | ------------- | ---- |
| CRITICAL-SECRET-DATA | RED-POLICY-CRITICAL-SECRET-DATA | - | - |
| NORMAL-DATA | RED-POLICY-NORMAL-DATA | - | - |
| NOT-SO-IMPORTANT-DATA | RED-POLICY-NOT-SO-IMPORTANT-DATA | - | - |

#### VRFs configuration

##### VRF BLUE

| AVT policy |
| ---------- |
| BLUE-POLICY |

| AVT Profile | AVT ID |
| ----------- | ------ |
| BLUE-POLICY-DEFAULT | 1 |
| BLUE-POLICY-VIDEO | 2 |
| BLUE-POLICY-VOICE | 3 |

##### VRF default

| AVT policy |
| ---------- |
| DEFAULT-POLICY-WITH-CP |

| AVT Profile | AVT ID |
| ----------- | ------ |
| DEFAULT-POLICY-DEFAULT | 1 |
| DEFAULT-POLICY-CONTROL-PLANE | 254 |

##### VRF RED

| AVT policy |
| ---------- |
| RED-POLICY |

| AVT Profile | AVT ID |
| ----------- | ------ |
| RED-POLICY-CRITICAL-SECRET-DATA | 2 |
| RED-POLICY-NORMAL-DATA | 3 |
| RED-POLICY-NOT-SO-IMPORTANT-DATA | 4 |

#### Router Adaptive Virtual Topology Configuration

```eos
!
router adaptive-virtual-topology
   topology role transit region gateway vxlan
   region REGION1 id 1
   zone REGION1-ZONE id 1
   site SITE4 id 102
   !
   policy BLUE-POLICY
      !
      match application-profile VIDEO
         avt profile BLUE-POLICY-VIDEO
      !
      match application-profile VOICE
         avt profile BLUE-POLICY-VOICE
         dscp 46
      !
      match application-profile default
         avt profile BLUE-POLICY-DEFAULT
   !
   policy DEFAULT-POLICY-WITH-CP
      !
      match application-profile APP-PROFILE-CONTROL-PLANE
         avt profile DEFAULT-POLICY-CONTROL-PLANE
      !
      match application-profile default
         avt profile DEFAULT-POLICY-DEFAULT
   !
   policy RED-POLICY
      !
      match application-profile CRITICAL-SECRET-DATA
         avt profile RED-POLICY-CRITICAL-SECRET-DATA
      !
      match application-profile NORMAL-DATA
         avt profile RED-POLICY-NORMAL-DATA
      !
      match application-profile NOT-SO-IMPORTANT-DATA
         avt profile RED-POLICY-NOT-SO-IMPORTANT-DATA
   !
   profile BLUE-POLICY-DEFAULT
      path-selection load-balance LB-BLUE-POLICY-DEFAULT
   !
   profile BLUE-POLICY-VIDEO
      path-selection load-balance LB-BLUE-POLICY-VIDEO
   !
   profile BLUE-POLICY-VOICE
      path-selection load-balance LB-BLUE-POLICY-VOICE
   !
   profile DEFAULT-POLICY-CONTROL-PLANE
      path-selection load-balance LB-DEFAULT-POLICY-CONTROL-PLANE
   !
   profile DEFAULT-POLICY-DEFAULT
      path-selection load-balance LB-DEFAULT-POLICY-DEFAULT
   !
   profile RED-POLICY-CRITICAL-SECRET-DATA
      path-selection load-balance LB-RED-POLICY-CRITICAL-SECRET-DATA
   !
   profile RED-POLICY-NORMAL-DATA
      path-selection load-balance LB-RED-POLICY-NORMAL-DATA
   !
   profile RED-POLICY-NOT-SO-IMPORTANT-DATA
      path-selection load-balance LB-RED-POLICY-NOT-SO-IMPORTANT-DATA
   !
   vrf BLUE
      avt policy BLUE-POLICY
      avt profile BLUE-POLICY-DEFAULT id 1
      avt profile BLUE-POLICY-VIDEO id 2
      avt profile BLUE-POLICY-VOICE id 3
   !
   vrf default
      avt policy DEFAULT-POLICY-WITH-CP
      avt profile DEFAULT-POLICY-DEFAULT id 1
      avt profile DEFAULT-POLICY-CONTROL-PLANE id 254
   !
   vrf RED
      avt policy RED-POLICY
      avt profile RED-POLICY-CRITICAL-SECRET-DATA id 2
      avt profile RED-POLICY-NORMAL-DATA id 3
      avt profile RED-POLICY-NOT-SO-IMPORTANT-DATA id 4
```

### Router Traffic-Engineering

- Traffic Engineering is enabled.

#### Router Traffic Engineering Device Configuration

```eos
!
router traffic-engineering
```

### Router BGP

ASN Notation: asplain

#### Router BGP Summary

| BGP AS | Router ID |
| ------ | --------- |
| 65000 | 192.168.255.15 |

| BGP Tuning |
| ---------- |
| no bgp default ipv4-unicast |
| maximum-paths 16 |

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

##### WAN-OVERLAY-PEERS

| Settings | Value |
| -------- | ----- |
| Address Family | wan |
| Remote AS | 65000 |
| Source | Dps1 |
| BFD | True |
| BFD Timers | interval: 1000, min_rx: 1000, multiplier: 10 |
| TTL Max Hops | 1 |
| Send community | all |
| Maximum routes | 0 (no limit) |

#### BGP Neighbors

| Neighbor | Remote AS | VRF | Shutdown | Send-community | Maximum-routes | Allowas-in | BFD | RIB Pre-Policy Retain | Route-Reflector Client | Passive | TTL Max Hops |
| -------- | --------- | --- | -------- | -------------- | -------------- | ---------- | --- | --------------------- | ---------------------- | ------- | ------------ |
| 10.0.4.56 | 65104 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - | - |
| 10.0.4.58 | 65104 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - | - |
| 192.168.42.1 | Inherited from peer group WAN-OVERLAY-PEERS | default | - | Inherited from peer group WAN-OVERLAY-PEERS | Inherited from peer group WAN-OVERLAY-PEERS | - | Inherited from peer group WAN-OVERLAY-PEERS(interval: 1000, min_rx: 1000, multiplier: 10) | - | - | - | Inherited from peer group WAN-OVERLAY-PEERS |
| 192.168.42.2 | Inherited from peer group WAN-OVERLAY-PEERS | default | - | Inherited from peer group WAN-OVERLAY-PEERS | Inherited from peer group WAN-OVERLAY-PEERS | - | Inherited from peer group WAN-OVERLAY-PEERS(interval: 1000, min_rx: 1000, multiplier: 10) | - | - | - | Inherited from peer group WAN-OVERLAY-PEERS |
| 192.168.255.13 | 65104 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - | - |

#### Router BGP EVPN Address Family

##### EVPN Peer Groups

| Peer Group | Activate | Route-map In | Route-map Out | Encapsulation | Next-hop-self Source Interface |
| ---------- | -------- | ------------ | ------------- | ------------- | ------------------------------ |
| EVPN-OVERLAY-PEERS | True |  - | - | default | - |
| WAN-OVERLAY-PEERS | True |  RM-EVPN-SOO-IN | RM-EVPN-SOO-OUT | path-selection | - |

##### EVPN DCI Gateway Summary

| Settings | Value |
| -------- | ----- |
| Remote Domain Peer Groups | WAN-OVERLAY-PEERS |
| L3 Gateway Configured | True |
| L3 Gateway Inter-domain | True |

#### Router BGP IPv4 SR-TE Address Family

##### IPv4 SR-TE Peer Groups

| Peer Group | Activate | Route-map In | Route-map Out |
| ---------- | -------- | ------------ | ------------- |
| WAN-OVERLAY-PEERS | True | - | - |

#### Router BGP Link-State Address Family

##### Link-State Peer Groups

| Peer Group | Activate | Missing policy In action | Missing policy Out action |
| ---------- | -------- | ------------------------ | ------------------------- |
| WAN-OVERLAY-PEERS | True | - | - |

##### Link-State Path Selection Configuration

| Settings | Value |
| -------- | ----- |
| Role(s) | producer |

#### Router BGP Path-Selection Address Family

##### Path-Selection Peer Groups

| Peer Group | Activate |
| ---------- | -------- |
| WAN-OVERLAY-PEERS | True |

#### Router BGP VRFs

| VRF | Route-Distinguisher | Redistribute | Graceful Restart |
| --- | ------------------- | ------------ | ---------------- |
| BLUE | 192.168.255.15:100 | connected | - |
| default | 192.168.255.15:1 | - | - |
| RED | 192.168.255.15:101 | connected | - |

#### Router BGP Device Configuration

```eos
!
router bgp 65000
   router-id 192.168.255.15
   no bgp default ipv4-unicast
   maximum-paths 16
   neighbor EVPN-OVERLAY-PEERS peer group
   neighbor EVPN-OVERLAY-PEERS update-source Loopback0
   neighbor EVPN-OVERLAY-PEERS bfd
   neighbor EVPN-OVERLAY-PEERS ebgp-multihop 3
   neighbor EVPN-OVERLAY-PEERS send-community
   neighbor EVPN-OVERLAY-PEERS maximum-routes 0
   neighbor IPv4-UNDERLAY-PEERS peer group
   neighbor IPv4-UNDERLAY-PEERS route-map RM-BGP-UNDERLAY-PEERS-IN in
   neighbor IPv4-UNDERLAY-PEERS send-community
   neighbor IPv4-UNDERLAY-PEERS maximum-routes 12000
   neighbor WAN-OVERLAY-PEERS peer group
   neighbor WAN-OVERLAY-PEERS remote-as 65000
   neighbor WAN-OVERLAY-PEERS update-source Dps1
   neighbor WAN-OVERLAY-PEERS bfd
   neighbor WAN-OVERLAY-PEERS bfd interval 1000 min-rx 1000 multiplier 10
   neighbor WAN-OVERLAY-PEERS ttl maximum-hops 1
   neighbor WAN-OVERLAY-PEERS password 7 <removed>
   neighbor WAN-OVERLAY-PEERS send-community
   neighbor WAN-OVERLAY-PEERS maximum-routes 0
   neighbor 10.0.4.56 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.0.4.56 remote-as 65104
   neighbor 10.0.4.56 description site4-border1_Ethernet3
   neighbor 10.0.4.58 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.0.4.58 remote-as 65104
   neighbor 10.0.4.58 description site4-border2_Ethernet3
   neighbor 192.168.42.1 peer group WAN-OVERLAY-PEERS
   neighbor 192.168.42.1 description pf1_Dps1
   neighbor 192.168.42.2 peer group WAN-OVERLAY-PEERS
   neighbor 192.168.42.2 description pf2_Dps1
   neighbor 192.168.255.13 peer group EVPN-OVERLAY-PEERS
   neighbor 192.168.255.13 remote-as 65104
   neighbor 192.168.255.13 description site4-border1_Loopback0
   redistribute connected route-map RM-CONN-2-BGP
   !
   address-family evpn
      neighbor EVPN-OVERLAY-PEERS activate
      neighbor WAN-OVERLAY-PEERS activate
      neighbor WAN-OVERLAY-PEERS route-map RM-EVPN-SOO-IN in
      neighbor WAN-OVERLAY-PEERS route-map RM-EVPN-SOO-OUT out
      neighbor WAN-OVERLAY-PEERS encapsulation path-selection
      neighbor WAN-OVERLAY-PEERS domain remote
      neighbor default next-hop-self received-evpn-routes route-type ip-prefix inter-domain
   !
   address-family ipv4
      no neighbor EVPN-OVERLAY-PEERS activate
      neighbor IPv4-UNDERLAY-PEERS activate
      no neighbor WAN-OVERLAY-PEERS activate
   !
   address-family ipv4 sr-te
      neighbor WAN-OVERLAY-PEERS activate
   !
   address-family link-state
      neighbor WAN-OVERLAY-PEERS activate
      path-selection
   !
   address-family path-selection
      bgp additional-paths receive
      bgp additional-paths send any
      neighbor WAN-OVERLAY-PEERS activate
   !
   vrf BLUE
      rd 192.168.255.15:100
      route-target import evpn 100:100
      route-target export evpn 100:100
      router-id 192.168.255.15
      redistribute connected
   !
   vrf default
      rd 192.168.255.15:1
      route-target import evpn 1:1
      route-target export evpn 1:1
      route-target export evpn route-map RM-EVPN-EXPORT-VRF-DEFAULT
   !
   vrf RED
      rd 192.168.255.15:101
      route-target import evpn 101:101
      route-target export evpn 101:101
      router-id 192.168.255.15
      redistribute connected
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

## Filters

### Prefix-lists

#### Prefix-lists Summary

##### PL-DPS-WAN-OVERLAY

| Sequence | Action |
| -------- | ------ |
| 10 | permit 192.168.42.15/32 eq 32 |

##### PL-LOOPBACKS-EVPN-OVERLAY

| Sequence | Action |
| -------- | ------ |
| 10 | permit 192.168.255.0/24 eq 32 |

#### Prefix-lists Device Configuration

```eos
!
ip prefix-list PL-DPS-WAN-OVERLAY
   seq 10 permit 192.168.42.15/32 eq 32
!
ip prefix-list PL-LOOPBACKS-EVPN-OVERLAY
   seq 10 permit 192.168.255.0/24 eq 32
```

### Route-maps

#### Route-maps Summary

##### RM-BGP-UNDERLAY-PEERS-IN

| Sequence | Type | Match | Set | Sub-Route-Map | Continue |
| -------- | ---- | ----- | --- | ------------- | -------- |
| 40 | permit | - | extcommunity soo 192.168.255.15:102 additive | - | - |

##### RM-CONN-2-BGP

| Sequence | Type | Match | Set | Sub-Route-Map | Continue |
| -------- | ---- | ----- | --- | ------------- | -------- |
| 10 | permit | ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY | extcommunity soo 192.168.255.15:102 additive | - | - |
| 80 | permit | ip address prefix-list PL-DPS-WAN-OVERLAY | - | - | - |

##### RM-EVPN-EXPORT-VRF-DEFAULT

| Sequence | Type | Match | Set | Sub-Route-Map | Continue |
| -------- | ---- | ----- | --- | ------------- | -------- |
| 10 | permit | extcommunity ECL-EVPN-SOO | - | - | - |

##### RM-EVPN-SOO-IN

| Sequence | Type | Match | Set | Sub-Route-Map | Continue |
| -------- | ---- | ----- | --- | ------------- | -------- |
| 10 | deny | extcommunity ECL-EVPN-SOO | - | - | - |
| 20 | permit | - | - | - | - |

##### RM-EVPN-SOO-OUT

| Sequence | Type | Match | Set | Sub-Route-Map | Continue |
| -------- | ---- | ----- | --- | ------------- | -------- |
| 10 | permit | - | extcommunity soo 192.168.255.15:102 additive | - | - |

#### Route-maps Device Configuration

```eos
!
route-map RM-BGP-UNDERLAY-PEERS-IN permit 40
   description Mark prefixes originated from the LAN
   set extcommunity soo 192.168.255.15:102 additive
!
route-map RM-CONN-2-BGP permit 10
   match ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY
   set extcommunity soo 192.168.255.15:102 additive
!
route-map RM-CONN-2-BGP permit 80
   match ip address prefix-list PL-DPS-WAN-OVERLAY
!
route-map RM-EVPN-EXPORT-VRF-DEFAULT permit 10
   match extcommunity ECL-EVPN-SOO
!
route-map RM-EVPN-SOO-IN deny 10
   match extcommunity ECL-EVPN-SOO
!
route-map RM-EVPN-SOO-IN permit 20
!
route-map RM-EVPN-SOO-OUT permit 10
   set extcommunity soo 192.168.255.15:102 additive
```

### IP Extended Community Lists

#### IP Extended Community Lists Summary

| List Name | Type | Extended Communities |
| --------- | ---- | -------------------- |
| ECL-EVPN-SOO | permit | soo 192.168.255.15:102 |

#### IP Extended Community Lists Device Configuration

```eos
!
ip extcommunity-list ECL-EVPN-SOO permit soo 192.168.255.15:102
```

## ACL

### IP Access-lists

#### IP Access-lists Device Configuration

```eos
!
ip access-list ACL-INTERNET-IN_Ethernet4
   1 remark Not for PRODUCTION: This ACL is built this way because the lab has an out-of-band interface
   10 permit udp any host 100.64.40.2 eq isakmp non500-isakmp
   30 permit icmp any host 100.64.40.2
   deny ip any any
```

## VRF Instances

### VRF Instances Summary

| VRF Name | IP Routing |
| -------- | ---------- |
| BLUE | enabled |
| MGMT | disabled |
| RED | enabled |

### VRF Instances Device Configuration

```eos
!
vrf instance BLUE
!
vrf instance MGMT
!
vrf instance RED
```

## Application Traffic Recognition

### Applications

#### IPv4 Applications

| Name | Source Prefix | Destination Prefix | Protocols | Protocol Ranges | TCP Source Port Set | TCP Destination Port Set | UDP Source Port Set | UDP Destination Port Set | DSCP |
| ---- | ------------- | ------------------ | --------- | --------------- | ------------------- | ------------------------ | ------------------- | ------------------------ | ---- |
| APP-CONTROL-PLANE | - | PFX-PATHFINDERS | - | - | - | - | - | - | - |
| CRITICAL-SECRET-DATA-APP | - | - | - | - | - | - | - | - | 46 |
| NORMAL-DATA-APP | - | - | - | - | - | - | - | - | af23 |
| NOT-SO-IMPORTANT-DATA-APP | - | - | - | - | - | - | - | - | 0 |
| VIDEO-APP | - | - | tcp, udp | - | - | VIDEO-PORTS | - | VIDEO-PORTS | - |
| VOICE-APP | - | - | tcp | - | - | VOICE-PORTS | - | - | - |

### Application Profiles

#### Application Profile Name APP-PROFILE-CONTROL-PLANE

| Type | Name | Service |
| ---- | ---- | ------- |
| application | APP-CONTROL-PLANE | - |

#### Application Profile Name CRITICAL-SECRET-DATA

| Type | Name | Service |
| ---- | ---- | ------- |
| application | CRITICAL-SECRET-DATA-APP | - |

#### Application Profile Name NORMAL-DATA

| Type | Name | Service |
| ---- | ---- | ------- |
| application | NORMAL-DATA-APP | - |

#### Application Profile Name NOT-SO-IMPORTANT-DATA

| Type | Name | Service |
| ---- | ---- | ------- |
| application | NOT-SO-IMPORTANT-DATA-APP | - |

#### Application Profile Name VIDEO

| Type | Name | Service |
| ---- | ---- | ------- |
| application | VIDEO-APP | - |

#### Application Profile Name VOICE

| Type | Name | Service |
| ---- | ---- | ------- |
| application | VOICE-APP | - |

### Field Sets

#### L4 Port Sets

| Name | Ports |
| ---- | ----- |
| VIDEO-PORTS | 4242-4244 |
| VOICE-PORTS | 666-667 |

#### IPv4 Prefix Sets

| Name | Prefixes |
| ---- | -------- |
| PFX-PATHFINDERS | 192.168.42.1/32<br>192.168.42.2/32 |

### Router Application-Traffic-Recognition Device Configuration

```eos
!
application traffic recognition
   !
   application ipv4 APP-CONTROL-PLANE
      destination prefix field-set PFX-PATHFINDERS
   !
   application ipv4 CRITICAL-SECRET-DATA-APP
      dscp 46
   !
   application ipv4 NORMAL-DATA-APP
      dscp af23
   !
   application ipv4 NOT-SO-IMPORTANT-DATA-APP
      dscp 0
   !
   application ipv4 VIDEO-APP
      protocol tcp destination port field-set VIDEO-PORTS
      protocol udp destination port field-set VIDEO-PORTS
   !
   application ipv4 VOICE-APP
      protocol tcp destination port field-set VOICE-PORTS
   !
   application-profile APP-PROFILE-CONTROL-PLANE
      application APP-CONTROL-PLANE
   !
   application-profile CRITICAL-SECRET-DATA
      application CRITICAL-SECRET-DATA-APP
   !
   application-profile NORMAL-DATA
      application NORMAL-DATA-APP
   !
   application-profile NOT-SO-IMPORTANT-DATA
      application NOT-SO-IMPORTANT-DATA-APP
   !
   application-profile VIDEO
      application VIDEO-APP
   !
   application-profile VOICE
      application VOICE-APP
   !
   field-set ipv4 prefix PFX-PATHFINDERS
      192.168.42.1/32 192.168.42.2/32
   !
   field-set l4-port VIDEO-PORTS
      4242-4244
   !
   field-set l4-port VOICE-PORTS
      666-667
```

### Router Path-selection

#### TCP MSS Ceiling Configuration

| IPV4 segment size | Direction |
| ----------------- | --------- |
| auto | ingress |

#### Path Groups

##### Path Group INTERNET

| Setting | Value |
| ------  | ----- |
| Path Group ID | 102 |
| IPSec profile | CP-PROFILE |

###### Local Interfaces

| Interface name | Public address | STUN server profile(s) |
| -------------- | -------------- | ---------------------- |
| Ethernet4 | - | INTERNET-pf1-Ethernet2<br>INTERNET-pf2-Ethernet2 |

###### Dynamic Peers Settings

| Setting | Value |
| ------  | ----- |
| IP Local | - |
| IPSec | - |

###### Static Peers

| Router IP | Name | IPv4 address(es) |
| --------- | ---- | ---------------- |
| 192.168.42.1 | pf1 | 100.64.100.2 |
| 192.168.42.2 | pf2 | 100.64.200.2 |

##### Path Group MPLS

| Setting | Value |
| ------  | ----- |
| Path Group ID | 101 |
| IPSec profile | CP-PROFILE |

###### Local Interfaces

| Interface name | Public address | STUN server profile(s) |
| -------------- | -------------- | ---------------------- |
| Ethernet3 | - | MPLS-pf1-Ethernet1<br>MPLS-pf2-Ethernet1 |

###### Dynamic Peers Settings

| Setting | Value |
| ------  | ----- |
| IP Local | - |
| IPSec | - |

###### Static Peers

| Router IP | Name | IPv4 address(es) |
| --------- | ---- | ---------------- |
| 192.168.42.1 | pf1 | 172.18.100.2 |
| 192.168.42.2 | pf2 | 172.18.200.2 |

#### Load-balance Policies

| Policy Name | Jitter (ms) | Latency (ms) | Loss Rate (%) | Path Groups (priority) | Lowest Hop Count |
| ----------- | ----------- | ------------ | ------------- | ---------------------- | ---------------- |
| LB-BLUE-POLICY-DEFAULT | - | - | - | INTERNET (1)<br>MPLS (1) | False |
| LB-BLUE-POLICY-VIDEO | - | - | - | INTERNET (1)<br>MPLS (2) | False |
| LB-BLUE-POLICY-VOICE | 30 | 150 | 1 | MPLS (1)<br>INTERNET (2) | True |
| LB-DEFAULT-POLICY-CONTROL-PLANE | - | - | - | INTERNET (1)<br>MPLS (1) | False |
| LB-DEFAULT-POLICY-DEFAULT | - | - | - | INTERNET (1)<br>MPLS (1) | False |
| LB-RED-POLICY-CRITICAL-SECRET-DATA | - | - | - | MPLS (1) | False |
| LB-RED-POLICY-NORMAL-DATA | - | - | - | INTERNET (1)<br>MPLS (2) | False |
| LB-RED-POLICY-NOT-SO-IMPORTANT-DATA | - | - | - | INTERNET (1) | False |

#### Router Path-selection Device Configuration

```eos
!
router path-selection
   tcp mss ceiling ipv4 ingress
   !
   path-group INTERNET id 102
      ipsec profile CP-PROFILE
      !
      local interface Ethernet4
         stun server-profile INTERNET-pf1-Ethernet2 INTERNET-pf2-Ethernet2
      !
      peer dynamic
      !
      peer static router-ip 192.168.42.1
         name pf1
         ipv4 address 100.64.100.2
      !
      peer static router-ip 192.168.42.2
         name pf2
         ipv4 address 100.64.200.2
   !
   path-group MPLS id 101
      ipsec profile CP-PROFILE
      !
      local interface Ethernet3
         stun server-profile MPLS-pf1-Ethernet1 MPLS-pf2-Ethernet1
      !
      peer dynamic
      !
      peer static router-ip 192.168.42.1
         name pf1
         ipv4 address 172.18.100.2
      !
      peer static router-ip 192.168.42.2
         name pf2
         ipv4 address 172.18.200.2
   !
   load-balance policy LB-BLUE-POLICY-DEFAULT
      path-group INTERNET
      path-group MPLS
   !
   load-balance policy LB-BLUE-POLICY-VIDEO
      path-group INTERNET
      path-group MPLS priority 2
   !
   load-balance policy LB-BLUE-POLICY-VOICE
      latency 150
      jitter 30
      loss-rate 1
      hop count lowest
      path-group MPLS
      path-group INTERNET priority 2
   !
   load-balance policy LB-DEFAULT-POLICY-CONTROL-PLANE
      path-group INTERNET
      path-group MPLS
   !
   load-balance policy LB-DEFAULT-POLICY-DEFAULT
      path-group INTERNET
      path-group MPLS
   !
   load-balance policy LB-RED-POLICY-CRITICAL-SECRET-DATA
      path-group MPLS
   !
   load-balance policy LB-RED-POLICY-NORMAL-DATA
      path-group INTERNET
      path-group MPLS priority 2
   !
   load-balance policy LB-RED-POLICY-NOT-SO-IMPORTANT-DATA
      path-group INTERNET
```

## STUN

### STUN Client

#### Server Profiles

| Server Profile | IP address | SSL Profile | Port |
| -------------- | ---------- | ----------- | ---- |
| INTERNET-pf1-Ethernet2 | 100.64.100.2 | STUN-DTLS | 3478 |
| INTERNET-pf2-Ethernet2 | 100.64.200.2 | STUN-DTLS | 3478 |
| MPLS-pf1-Ethernet1 | 172.18.100.2 | STUN-DTLS | 3478 |
| MPLS-pf2-Ethernet1 | 172.18.200.2 | STUN-DTLS | 3478 |

### STUN Device Configuration

```eos
!
stun
   client
      server-profile INTERNET-pf1-Ethernet2
         ip address 100.64.100.2
         ssl profile STUN-DTLS
      server-profile INTERNET-pf2-Ethernet2
         ip address 100.64.200.2
         ssl profile STUN-DTLS
      server-profile MPLS-pf1-Ethernet1
         ip address 172.18.100.2
         ssl profile STUN-DTLS
      server-profile MPLS-pf2-Ethernet1
         ip address 172.18.200.2
         ssl profile STUN-DTLS
```
