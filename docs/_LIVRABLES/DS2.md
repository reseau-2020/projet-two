```
hostname DS2
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
ip dhcp excluded-address 172.16.10.1 172.16.10.50
ip dhcp excluded-address 172.16.10.100 172.16.10.254
ip dhcp excluded-address 172.16.20.1 172.16.20.50
ip dhcp excluded-address 172.16.20.100 172.16.20.254
ip dhcp excluded-address 172.16.30.1 172.16.30.50
ip dhcp excluded-address 172.16.30.100 172.16.30.254
ip dhcp excluded-address 172.16.40.1 172.16.40.50
ip dhcp excluded-address 172.16.40.100 172.16.40.254
ip dhcp excluded-address 10.128.10.1 10.128.10.50
ip dhcp excluded-address 10.128.10.100 10.128.10.254
ip dhcp excluded-address 10.128.20.1 10.128.20.50
ip dhcp excluded-address 10.128.20.100 10.128.20.254
ip dhcp excluded-address 10.128.30.1 10.128.30.50
ip dhcp excluded-address 10.128.30.100 10.128.30.254
ip dhcp excluded-address 10.128.40.1 10.128.40.50
ip dhcp excluded-address 10.128.40.100 10.128.40.254
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
spanning-tree vlan 1,10,30,99 priority 28672
spanning-tree vlan 20,40 priority 24576
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
interface Port-channel2
 description Trunk Group Channel2 to AS1
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 99
 switchport mode trunk
!
interface Port-channel3
 description Trunk Group Channel3 to DS1
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 99
 switchport mode trunk
!
interface Port-channel4
 description Trunk Group Channel4 to AS2
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 99
 switchport mode trunk
!
interface GigabitEthernet0/0
 description Trunk Group Channel4 to AS2
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 99
 switchport mode trunk
 negotiation auto
 channel-group 4 mode active
!
interface GigabitEthernet0/1
 description Trunk Group Channel2 to AS1
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 99
 switchport mode trunk
 negotiation auto
 channel-group 2 mode active
!
interface GigabitEthernet0/2
 description Trunk Group Channel3 to DS1
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
 description Trunk Group Channel4 to AS2
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 99
 switchport mode trunk
 negotiation auto
 channel-group 4 mode active
!
interface GigabitEthernet1/1
 description Trunk Group Channel2 to AS1
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 99
 switchport mode trunk
 negotiation auto
 channel-group 2 mode active
!
interface GigabitEthernet1/2
 description Trunk Group Channel3 to DS1
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
 description Link to R3 Core
 no switchport
 ip address 10.128.240.2 255.255.255.0
 negotiation auto
 ipv6 address FE80::D:3 link-local
 ipv6 enable
 ipv6 eigrp 1
!
interface GigabitEthernet2/1
 description Link to R2 Core
 no switchport
 ip address 10.128.242.2 255.255.255.0
 negotiation auto
 ipv6 address FE80::D:3 link-local
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
 description Link to R3 Core
 no switchport
 ip address 10.128.241.2 255.255.255.0
 negotiation auto
 ipv6 address FE80::D:3 link-local
 ipv6 enable
 ipv6 eigrp 1
!
interface GigabitEthernet3/1
 description Link to R2 Core
 no switchport
 ip address 10.128.243.2 255.255.255.0
 negotiation auto
 ipv6 address FE80::D:3 link-local
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
 ip address 10.128.10.253 255.255.255.0
 standby version 2
 standby 10 ip 10.128.10.254
 standby 16 ipv6 FE80::D:1
 ipv6 address FE80::D:3 link-local
 ipv6 address 2001:470:C814:2A0A::3/64
 ipv6 address FD00:470:C814:2A0A::3/64
 ipv6 enable
 ipv6 eigrp 1
!
interface Vlan20
 description Vlan20 real gateway
 ip address 10.128.20.253 255.255.255.0
 standby version 2
 standby 20 ip 10.128.20.254
 standby 20 priority 150
 standby 26 ipv6 FE80::D:1
 standby 26 priority 150
 ipv6 address FE80::D:3 link-local
 ipv6 address 2001:470:C814:2A14::3/64
 ipv6 address FD00:470:C814:2A14::3/64
 ipv6 enable
 ipv6 eigrp 1
!
interface Vlan30
 description Vlan30 real gateway
 ip address 10.128.30.253 255.255.255.0
 standby version 2
 standby 30 ip 10.128.30.254
 standby 36 ipv6 FE80::D:1
 ipv6 address FE80::D:3 link-local
 ipv6 address 2001:470:C814:2AAE::3/64
 ipv6 address FD00:470:C814:2AAE::3/64
 ipv6 enable
 ipv6 eigrp 1
!
interface Vlan40
 description Vlan40 real gateway
 ip address 10.128.40.253 255.255.255.0
 standby version 2
 standby 40 ip 10.128.40.254
 standby 40 priority 150
 standby 46 ipv6 FE80::D:1
 standby 46 priority 150
 ipv6 address FE80::D:3 link-local
 ipv6 address 2001:470:C814:2A28::3/64
 ipv6 address FD00:470:C814:2A28::3/64
 ipv6 enable
 ipv6 eigrp 1
!
interface Vlan99
 description Management real gateway
 ip address 10.128.0.253 255.255.255.0
 ipv6 eigrp 1
!
!
router eigrp 1
 network 10.128.0.253 0.0.0.0
 network 10.128.10.253 0.0.0.0
 network 10.128.20.253 0.0.0.0
 network 10.128.30.253 0.0.0.0
 network 10.128.40.253 0.0.0.0
 network 10.128.240.2 0.0.0.0
 network 10.128.241.2 0.0.0.0
 network 10.128.242.2 0.0.0.0
 network 10.128.243.2 0.0.0.0
 network 172.16.0.253 0.0.0.0
 network 172.16.10.253 0.0.0.0
 network 172.16.20.253 0.0.0.0
 network 172.16.30.253 0.0.0.0
 network 172.16.40.253 0.0.0.0
 network 172.16.240.2 0.0.0.0
 network 172.16.241.2 0.0.0.0
 network 172.16.242.2 0.0.0.0
 network 172.16.243.2 0.0.0.0
 passive-interface Vlan99
 passive-interface Vlan10
 passive-interface Vlan20
 passive-interface Vlan30
 passive-interface Vlan40
 eigrp router-id 5.5.5.5
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
 eigrp router-id 5.5.5.5
!
!
!
!
!
control-plane
!
banner exec ^C
Default Multi-line exec banner
for DS2 device in the CCNA Lab
^C
banner incoming ^C
Default Multi-line incoming banner
for DS2 device in the CCNA Lab
^C
banner login ^C
Default Multi-line motd banner
for DS2 device in the CCNA Lab
^C
banner motd ^C
Default Multi-line motd banner for DS2
for DS2 device in the CCNA Lab
^C
banner slip-ppp ^C
Default Multi-line slip-ppp banner
for DS2 device in the CCNA Lab
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
