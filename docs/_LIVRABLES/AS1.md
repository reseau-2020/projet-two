```
hostname AS1
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
!
!
ip domain-name lan
ip cef
no ipv6 cef
!
!
!
spanning-tree mode rapid-pvst
spanning-tree extend system-id
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
 description Trunk Group Channel1 to DS1
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 99
 switchport mode trunk
!
interface Port-channel2
 description Trunk Group Channel2 to DS2
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 99
 switchport mode trunk
!
interface GigabitEthernet0/0
 description Trunk Group Channel1 to DS1
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 99
 switchport mode trunk
 negotiation auto
 channel-group 1 mode active
!
interface GigabitEthernet0/1
 description Trunk Group Channel2 to DS2
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 99
 switchport mode trunk
 negotiation auto
 channel-group 2 mode active
!
interface GigabitEthernet0/2
 switchport access vlan 4000
 switchport mode access
 shutdown
 negotiation auto
!
interface GigabitEthernet0/3
 switchport access vlan 4000
 switchport mode access
 shutdown
 negotiation auto
!
interface GigabitEthernet1/0
 description Trunk Group Channel1 to DS1
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 99
 switchport mode trunk
 negotiation auto
 channel-group 1 mode active
!
interface GigabitEthernet1/1
 description Trunk Group Channel2 to DS2
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 99
 switchport mode trunk
 negotiation auto
 channel-group 2 mode active
!
interface GigabitEthernet1/2
 switchport access vlan 4000
 switchport mode access
 shutdown
 negotiation auto
!
interface GigabitEthernet1/3
 switchport access vlan 4000
 switchport mode access
 shutdown
 negotiation auto
!
interface GigabitEthernet2/0
 description PC1 vlan 10 access port
 switchport access vlan 10
 switchport mode access
 negotiation auto
 spanning-tree portfast edge
 spanning-tree bpduguard enable
!
interface GigabitEthernet2/1
 description PC2 vlan 20 access port
 switchport access vlan 20
 switchport mode access
 negotiation auto
 spanning-tree portfast edge
 spanning-tree bpduguard enable
!
interface GigabitEthernet2/2
 description PC3 vlan 30 access port
 switchport access vlan 30
 switchport mode access
 negotiation auto
 spanning-tree portfast edge
 spanning-tree bpduguard enable
!
interface GigabitEthernet2/3
 description PC4 vlan 40 access port
 switchport access vlan 40
 switchport mode access
 negotiation auto
 spanning-tree portfast edge
 spanning-tree bpduguard enable
!
interface GigabitEthernet3/0
 switchport access vlan 4000
 switchport mode access
 shutdown
 negotiation auto
!
interface GigabitEthernet3/1
 switchport access vlan 4000
 switchport mode access
 shutdown
 negotiation auto
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
interface Vlan99
 description Management interface
 ip address 10.128.0.1 255.255.255.0
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
!
!
!
!
control-plane
!
banner exec ^C
Default Multi-line exec banner
for AS1 device in the CCNA Lab
^C
banner incoming ^C
Default Multi-line incoming banner
for AS1 device in the CCNA Lab
^C
banner login ^C
Default Multi-line motd banner
for AS1 device in the CCNA Lab
^C
banner motd ^C
Default Multi-line motd banner for AS1
for AS1 device in the CCNA Lab
^C
banner slip-ppp ^C
Default Multi-line slip-ppp banner
for AS1 device in the CCNA Lab
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
