```
hostname DS1
!
boot-start-marker
boot-end-marker
!
!
!
username root privilege 15 password 0 testtest
no aaa new-model
!
!
!
!
!
!
ip dhcp excluded-address 172.16.10.50 172.16.10.254
ip dhcp excluded-address 172.16.20.50 172.16.20.254
ip dhcp excluded-address 172.16.30.50 172.16.30.254
ip dhcp excluded-address 172.16.40.50 172.16.40.254
ip dhcp excluded-address 10.128.10.50 10.128.10.254
ip dhcp excluded-address 10.128.20.50 10.128.20.254
ip dhcp excluded-address 10.128.30.50 10.128.30.254
ip dhcp excluded-address 10.128.40.50 10.128.40.254
!
ip dhcp pool VLAN10
 network 10.128.10.0 255.255.255.0
 default-router 10.128.10.254
 dns-server 8.8.8.8
!
ip dhcp pool VLAN20
 network 10.128.20.0 255.255.255.0
 default-router 10.128.20.254
 dns-server 8.8.8.8
!
ip dhcp pool VLAN30
 network 10.128.30.0 255.255.255.0
 default-router 10.128.30.254
 dns-server 8.8.8.8
!
ip dhcp pool VLAN40
 network 10.128.40.0 255.255.255.0
 default-router 10.128.40.254
 dns-server 8.8.8.8
!
!
ip domain-name lan
ip cef
ipv6 unicast-routing
ipv6 cef
!
!
!
spanning-tree mode rapid-pvst
spanning-tree extend system-id
spanning-tree vlan 1,10,30,99 priority 24576
spanning-tree vlan 20,40 priority 28672
!
!
vlan 4000
 name STUB
!
!
!
!
!
!
!
!
!
!
!
!
!
!
interface Port-channel1
 description Trunk Group Channel1 to AS1
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 99
 switchport mode trunk
!
interface Port-channel3
 description Trunk Group Channel3 to DS2
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 99
 switchport mode trunk
!
interface Port-channel5
 description Trunk Group Channel5 to AS2
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 99
 switchport mode trunk
!
interface GigabitEthernet0/0
 description Trunk Group Channel1 to AS1
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 99
 switchport mode trunk
 negotiation auto
 channel-group 1 mode active
!
interface GigabitEthernet0/1
 description Trunk Group Channel5 to AS2
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 99
 switchport mode trunk
 negotiation auto
 channel-group 5 mode active
!
interface GigabitEthernet0/2
 description Trunk Group Channel3 to DS2
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 99
 switchport mode trunk
 negotiation auto
 channel-group 3 mode active
!
interface GigabitEthernet0/3
 switchport access vlan 4000
 switchport mode access
 shutdown
 negotiation auto
!
interface GigabitEthernet1/0
 description Trunk Group Channel1 to AS1
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 99
 switchport mode trunk
 negotiation auto
 channel-group 1 mode active
!
interface GigabitEthernet1/1
 description Trunk Group Channel5 to AS2
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 99
 switchport mode trunk
 negotiation auto
 channel-group 5 mode active
!
interface GigabitEthernet1/2
 description Trunk Group Channel3 to DS2
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 99
 switchport mode trunk
 negotiation auto
 channel-group 3 mode active
!
interface GigabitEthernet1/3
 switchport access vlan 4000
 switchport mode access
 shutdown
 negotiation auto
!
interface GigabitEthernet2/0
 description Link to R2 Core
 no switchport
 ip address 10.128.225.2 255.255.255.0
 negotiation auto
 ipv6 address FE80::D:2 link-local
 ipv6 enable
 ipv6 eigrp 1
!
interface GigabitEthernet2/1
 description Link to R3 Core
 no switchport
 ip address 10.128.226.2 255.255.255.0
 negotiation auto
 ipv6 address FE80::D:2 link-local
 ipv6 enable
 ipv6 eigrp 1
!
interface GigabitEthernet2/2
 switchport access vlan 4000
 switchport mode access
 shutdown
 negotiation auto
!
interface GigabitEthernet2/3
 switchport access vlan 4000
 switchport mode access
 shutdown
 negotiation auto
!
interface GigabitEthernet3/0
 description Link to R2 Core
 no switchport
 ip address 10.128.224.2 255.255.255.0
 negotiation auto
 ipv6 address FE80::D:2 link-local
 ipv6 enable
 ipv6 eigrp 1
!
interface GigabitEthernet3/1
 description Link to R3 Core
 no switchport
 ip address 10.128.227.2 255.255.255.0
 negotiation auto
 ipv6 address FE80::D:2 link-local
 ipv6 enable
 ipv6 eigrp 1
!
interface GigabitEthernet3/2
 switchport access vlan 4000
 switchport mode access
 shutdown
 negotiation auto
!
interface GigabitEthernet3/3
 no switchport
 ip address dhcp
 negotiation auto
 no cdp enable
!
interface Vlan10
 description Vlan10 real gateway
 ip address 10.128.10.252 255.255.255.0
 standby version 2
 standby 10 ip 10.128.10.254
 standby 10 priority 150
 standby 16 ipv6 FE80::D:1
 standby 16 priority 150
 ipv6 address FE80::D:2 link-local
 ipv6 address 2001:470:C814:2A0A::2/64
 ipv6 address FD00:470:C814:2A0A::2/64
 ipv6 enable
 ipv6 eigrp 1
!
interface Vlan20
 description Vlan20 real gateway
 ip address 10.128.20.252 255.255.255.0
 standby version 2
 standby 20 ip 10.128.20.254
 standby 26 ipv6 FE80::D:1
 ipv6 address FE80::D:2 link-local
 ipv6 address 2001:470:C814:2A14::2/64
 ipv6 address FD00:470:C814:2A14::2/64
 ipv6 enable
 ipv6 eigrp 1
!
interface Vlan30
 description Vlan30 real gateway
 ip address 10.128.30.252 255.255.255.0
 standby version 2
 standby 30 ip 10.128.30.254
 standby 30 priority 150
 standby 36 ipv6 FE80::D:1
 standby 36 priority 150
 ipv6 address FE80::D:2 link-local
 ipv6 address 2001:470:C814:2AAE::2/64
 ipv6 address FD00:470:C814:2AAE::2/64
 ipv6 enable
 ipv6 eigrp 1
!
interface Vlan40
 description Vlan40 real gateway
 ip address 10.128.40.252 255.255.255.0
 standby version 2
 standby 40 ip 10.128.40.254
 standby 46 ipv6 FE80::D:1
 ipv6 address FE80::D:2 link-local
 ipv6 address 2001:470:C814:2A28::2/64
 ipv6 address FD00:470:C814:2A28::2/64
 ipv6 enable
 ipv6 eigrp 1
!
interface Vlan99
 description Management real gateway
 ip address 10.128.0.252 255.255.255.0
 ipv6 eigrp 1
!
!
router eigrp 1
 network 10.128.0.252 0.0.0.0
 network 10.128.10.252 0.0.0.0
 network 10.128.20.252 0.0.0.0
 network 10.128.30.252 0.0.0.0
 network 10.128.40.252 0.0.0.0
 network 10.128.224.2 0.0.0.0
 network 10.128.225.2 0.0.0.0
 network 10.128.226.2 0.0.0.0
 network 10.128.227.2 0.0.0.0
 network 172.16.0.252 0.0.0.0
 network 172.16.10.252 0.0.0.0
 network 172.16.20.252 0.0.0.0
 network 172.16.30.252 0.0.0.0
 network 172.16.40.252 0.0.0.0
 network 172.16.224.2 0.0.0.0
 network 172.16.225.2 0.0.0.0
 network 172.16.226.2 0.0.0.0
 network 172.16.227.2 0.0.0.0
 passive-interface Vlan99
 passive-interface Vlan10
 passive-interface Vlan20
 passive-interface Vlan30
 passive-interface Vlan40
 eigrp router-id 4.4.4.4
!
ip forward-protocol nd
!
ip http server
!
ip ssh version 2
ip ssh server algorithm encryption aes128-ctr aes192-ctr aes256-ctr
ip ssh client algorithm encryption aes128-ctr aes192-ctr aes256-ctr
ip scp server enable
!
!
ipv6 router eigrp 1
 passive-interface Vlan10
 passive-interface Vlan20
 passive-interface Vlan30
 passive-interface Vlan40
 passive-interface Vlan99
 eigrp router-id 4.4.4.4
!
!
!
!
!
control-plane
!
banner exec ^C
Default Multi-line exec banner
for DS1 device in the CCNA Lab
^C
banner incoming ^C
Default Multi-line incoming banner
for DS1 device in the CCNA Lab
^C
banner login ^C
Default Multi-line motd banner
for DS1 device in the CCNA Lab
^C
banner motd ^C
Default Multi-line motd banner for DS1
for DS1 device in the CCNA Lab
^C
banner slip-ppp ^C
Default Multi-line slip-ppp banner
for DS1 device in the CCNA Lab
^C
!
line con 0
line aux 0
line vty 0 4
 login local
 transport input ssh
!
!
end
```
