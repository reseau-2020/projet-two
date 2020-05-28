## Playbook de DS2 dans /inventories/ccna/host_vars/

```
hostname: DS2
stp:
  primary:
    - 20
    - 40
  secondary:
    - 1
    - 10
    - 30
    - 99
  mode: rapid-pvst
group_channels:
  - id: 2
    mode: active
    interfaces:
      - GigabitEthernet0/1
      - GigabitEthernet1/1
  - id: 3
    mode: active
    interfaces:
      - GigabitEthernet0/2
      - GigabitEthernet1/2
  - id: 4
    mode: active
    interfaces:
      - GigabitEthernet0/0
      - GigabitEthernet1/0
interfaces:
  - id: GigabitEthernet0/0
    description: "Trunk Group Channel4 to AS2"
    trunk:
      native: 99
  - id: GigabitEthernet1/0
    description: "Trunk Group Channel4 to AS2"
    trunk:
      native: 99
  - id: GigabitEthernet0/1
    description: "Trunk Group Channel2 to AS1"
    trunk:
      native: 99
  - id: GigabitEthernet1/1
    description: "Trunk Group Channel2 to AS1"
    trunk:
      native: 99
  - id: GigabitEthernet0/2
    description: "Trunk Group Channel3 to DS1"
    trunk:
      native: 99
  - id: GigabitEthernet1/2
    description: "Trunk Group Channel3 to DS1"
    trunk:
      native: 99
  - id: Port-channel2
    description: "Trunk Group Channel2 to AS1"
    trunk:
      native: 99
  - id: Port-channel3
    description: "Trunk Group Channel3 to DS1"
    trunk:
      native: 99
  - id: Port-channel4
    description: "Trunk Group Channel4 to AS2"
    trunk:
      native: 99
  - id: Vlan99
    description: "Management real gateway"
    ipv4_address: 10.128.0.253/24
    passive:
    rip:
    eigrp4:
    ospfv2:
      area: 0
      cost: 1
      pri: 255
    eigrp6:
    ospfv3:
      area: 0
      cost: 1
      pri: 255
  - id: Vlan10
    description: "Vlan10 real gateway"
    ipv4_address: 10.128.10.253/24
    passive:
    rip:
    eigrp4:
    ospfv2:
      area: 0
      cost: 1
      pri: 255
    ipv6_addresses:
      - 'FE80::D:3'
      - '2001:470:c814:2A0A::3/64'
      - 'FD00:470:c814:2A0A::3/64'
    eigrp6:
    ospfv3:
      area: 0
      cost: 1
      pri: 255
  - id: Vlan20
    description: "Vlan20 real gateway"
    ipv4_address: 10.128.20.253/24
    passive:
    rip:
    eigrp4:
    ospfv2:
      area: 0
      cost: 1
      pri: 255
    ipv6_addresses:
      - 'FE80::D:3'
      - '2001:470:c814:2A14::3/64'
      - 'FD00:470:c814:2A14::3/64'
    eigrp6:
    ospfv3:
      area: 0
      cost: 1
      pri: 255
  - id: Vlan30
    description: "Vlan30 real gateway"
    ipv4_address: 10.128.30.253/24
    passive:
    rip:
    eigrp4:
    ospfv2:
      area: 0
      cost: 1
      pri: 255
    ipv6_addresses:
      - 'FE80::D:3'
      - '2001:470:c814:2AAE::3/64'
      - 'FD00:470:c814:2AAE::3/64'
    eigrp6:
    ospfv3:
      area: 0
      cost: 1
      pri: 255
  - id: Vlan40
    description: "Vlan40 real gateway"
    ipv4_address: 10.128.40.253/24
    passive:
    rip:
    eigrp4:
    ospfv2:
      area: 0
      cost: 1
      pri: 255
    ipv6_addresses:
      - 'FE80::D:3'
      - '2001:470:c814:2A28::3/64'
      - 'FD00:470:c814:2A28::3/64'
    eigrp6:
    ospfv3:
      area: 0
      cost: 1
      pri: 255
  - id: GigabitEthernet2/0
    description: "Link to R3 Core"
    noswitchport: true
    ipv4_address: 10.128.240.2/24
    rip:
    eigrp4:
    ospfv2:
      area: 0
      cost: 1
      pri: 255
    ipv6_addresses:
      - 'FE80::D:3'
    eigrp6:
    ospfv3:
      area: 0
      cost: 1
      pri: 255
  - id: GigabitEthernet3/0
    description: "Link to R3 Core"
    noswitchport: true
    ipv4_address: 10.128.241.2/24
    rip:
    eigrp4:
    ospfv2:
      area: 0
      cost: 1
      pri: 255
    ipv6_addresses:
      - 'FE80::D:3'
    eigrp6:
    ospfv3:
      area: 0
      cost: 1
      pri: 255
  - id: GigabitEthernet2/1
    description: "Link to R2 Core"
    noswitchport: true
    ipv4_address: 10.128.242.2/24
    rip:
    eigrp4:
    ospfv2:
      area: 0
      cost: 1
      pri: 255
    ipv6_addresses:
      - 'FE80::D:3'
    eigrp6:
    ospfv3:
      area: 0
      cost: 1
      pri: 255
  - id: GigabitEthernet3/1
    description: "Link to R2 Core"
    noswitchport: true
    ipv4_address: 10.128.243.2/24
    rip:
    eigrp4:
    ospfv2:
      area: 0
      cost: 1
      pri: 255
    ipv6_addresses:
      - 'FE80::D:3'
    eigrp6:
    ospfv3:
      area: 0
      cost: 1
      pri: 255
  - id: GigabitEthernet0/3
    description: "Stub interface"
    stub: true
  - id: GigabitEthernet1/3
    description: "Stub interface"
    stub: true
  - id: GigabitEthernet2/2
    description: "Stub interface"
    stub: true
  - id: GigabitEthernet2/3
    description: "Stub interface"
    stub: true
  - id: GigabitEthernet3/2
    description: "Stub interface"
    stub: true
routing:
  rid: 5.5.5.5
  eigrp_as: 1
  fhrp:
    - id: Vlan10
      group: 10
      protocol: ipv4
      address: 10.128.10.254
      preempt: false
      mode: standby
    - id: Vlan20
      group: 20
      protocol: ipv4
      address: 10.128.20.254
      priority: 150
      preempt: true
      mode: standby
    - id: Vlan30
      group: 30
      protocol: ipv4
      address: 10.128.30.254
      preempt: false
      mode: standby
    - id: Vlan40
      group: 40
      protocol: ipv4
      address: 10.128.40.254
      priority: 150
      preempt: true
      mode: standby
    - id: Vlan10
      group: 16
      protocol: ipv6
      address: 'FE80::D:1'
      preempt: false
      mode: standby
    - id: Vlan20
      group: 26
      protocol: ipv6
      address: 'FE80::D:1'
      priority: 150
      preempt: true
      mode: standby
    - id: Vlan30
      group: 36
      protocol: ipv6
      address: 'FE80::D:1'
      preempt: false
      mode: standby
    - id: Vlan40
      group: 46
      protocol: ipv6
      address: 'FE80::D:1'
      priority: 150
      preempt: true
      mode: standby
dhcp:
  dhcp_pool:
    - id: "VLAN10"
      network: 10.128.10.0
      netmask: 255.255.255.0
      router: 10.128.10.254
      dns: 1.1.1.1
    - id: "VLAN20"
      network: 10.128.20.0
      netmask: 255.255.255.0
      router: 10.128.20.254
      dns: 1.1.1.1
    - id: "VLAN30"
      network: 10.128.30.0
      netmask: 255.255.255.0
      router: 10.128.30.254
      dns: 1.1.1.1
    - id: "VLAN40"
      network: 10.128.40.0
      netmask: 255.255.255.0
      router: 10.128.40.254
      dns: 1.1.1.1
  dhcp_excluded:
    - start: 10.128.10.1
      end: 10.128.10.50
    - start: 10.128.10.100
      end: 10.128.10.254
    - start: 10.128.20.1
      end: 10.128.20.50
    - start: 10.128.20.100
      end: 10.128.20.254
    - start: 10.128.30.1
      end: 10.128.30.50
    - start: 10.128.30.100
      end: 10.128.30.254
    - start: 10.128.40.1
      end: 10.128.40.50
    - start: 10.128.40.100
      end: 10.128.40.254
```
