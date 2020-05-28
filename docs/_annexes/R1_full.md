```
R1#sh run
Building configuration...
​
Current configuration : 4022 bytes
!
version 15.7
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname R1
!
boot-start-marker
boot-end-marker
!
!
!
no aaa new-model
!
!
!
clock timezone cet 1 0
clock summer-time cest recurring last Sun Mar 3:00 last Sun Oct 3:00
mmi polling-interval 60
no mmi auto-configure
no mmi pvc
mmi snmp-timeout 180
!
!
!
!
!
no ip icmp rate-limit unreachable
!
!
!
ip dhcp excluded-address 192.168.1.1 192.168.1.100
ip dhcp excluded-address 10.192.1.1 10.192.1.100
!
ip dhcp pool LANR1
 network 10.192.1.0 255.255.255.0
 default-router 10.192.1.1
!
!
!
ip domain name lan
ip name-server 1.1.1.1
ip cef
ipv6 unicast-routing
ipv6 cef
!
multilink bundle-name authenticated
!
!
!
!
username root privilege 15 password 0 testtest
!
redundancy
!
no cdp log mismatch duplex
!
ip tcp synwait-time 5
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
interface GigabitEthernet0/0
 description LAN R1
 ip address 10.192.1.1 255.255.255.0
 ip nat inside
 ip virtual-reassembly in
 duplex auto
 speed auto
 media-type rj45
 ipv6 address FE80::1 link-local 
 ipv6 address 2001:470:C814:2000::1/64
 ipv6 address FD00:470:C814:2000::1/64
 ipv6 enable
 ipv6 eigrp 1
!
interface GigabitEthernet0/1
 description Internet connexion ip nat inside
 ip address 10.0.1.1 255.255.255.0
 ip nat outside
 ip virtual-reassembly in
 duplex auto
 speed auto
 media-type rj45
 ipv6 address 2001:470:C814:2FFF::2/64 #config manuelle
 ipv6 address autoconfig #config manuelle
 ipv6 enable
!
interface GigabitEthernet0/2
 description Link to R2
 ip address 10.192.225.1 255.255.255.0
 ip nat inside
 ip virtual-reassembly in
 duplex auto
 speed auto
 media-type rj45
 ipv6 address FE80::1 link-local
 ipv6 enable
 ipv6 eigrp 1
!
interface GigabitEthernet0/3
 description Link to R3
 ip address 10.192.226.1 255.255.255.0
 ip nat inside
 ip virtual-reassembly in
 duplex auto
 speed auto
 media-type rj45
 ipv6 address FE80::1 link-local
 ipv6 enable
 ipv6 eigrp 1
!
interface GigabitEthernet0/4
 no ip address
 shutdown
 duplex auto
 speed auto
 media-type rj45
!
interface GigabitEthernet0/5
 no ip address
 shutdown
 duplex auto
 speed auto
 media-type rj45
!
interface GigabitEthernet0/6
 no ip address
 shutdown
 duplex auto
 speed auto
 media-type rj45
!
interface GigabitEthernet0/7
 ip address dhcp
 duplex auto
 speed auto
 media-type rj45
 no cdp enable
!
!
router eigrp 1
 network 10.0.1.0 0.0.0.255
 network 10.192.1.1 0.0.0.0
 network 10.192.225.1 0.0.0.0
 network 10.192.226.1 0.0.0.0
 network 192.168.1.1 0.0.0.0
 network 192.168.225.1 0.0.0.0
 network 192.168.226.1 0.0.0.0
 redistribute static
 passive-interface GigabitEthernet0/0
 eigrp router-id 1.1.1.1
!
ip forward-protocol nd
!
!
no ip http server
no ip http secure-server
ip dns server
ip route 0.0.0.0 0.0.0.0 10.0.1.2
ip route 10.0.1.0 255.255.255.0 GigabitEthernet0/1
ip route 10.0.1.0 255.255.255.0 10.0.1.2
ip ssh version 2
ip scp server enable
!
ip access-list standard LANS
 permit 192.168.0.0 0.0.255.255
 permit 172.16.0.0 0.0.255.255
 permit 10.192.0.0 0.0.255.255
 permit 10.128.0.0 0.0.255.255
!
logging trap debugging
logging facility local1
logging host 10.192.1.101
ipv6 route 2000::/3 GigabitEthernet0/1 FE80::E3F:C7FF:FE01:5902 #config manuelle
ipv6 router eigrp 1
 passive-interface GigabitEthernet0/0
 eigrp router-id 1.1.1.1
 redistribute static
!
ipv6 ioam timestamp
!
!
!
control-plane
!
banner exec ^C
Default Multi-line exec banner
for R1 device in the CCNA Lab
^C
banner incoming ^C
Default Multi-line incoming banner
for R1 device in the CCNA Lab
^C
banner login ^C
Default Multi-line motd banner
for R1 device in the CCNA Lab
^C
banner motd ^C
Default Multi-line motd banner for R1
for R1 device in the CCNA Lab
^C
banner slip-ppp ^C
Default Multi-line slip-ppp banner
for R1 device in the CCNA Lab
^C
!
line con 0
 exec-timeout 0 0
 privilege level 15
 logging synchronous
line aux 0
 exec-timeout 0 0
 privilege level 15
 logging synchronous
line vty 0 4
 login local
 transport input ssh
!
no scheduler allocate
ntp update-calendar
ntp server ntp2.fortiguard.com minpoll 10 #config manuelle
!
end
```
