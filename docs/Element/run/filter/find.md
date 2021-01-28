Find
============

`<find>` tag described what exact field and content to match with.

<h2>Attribute</h2>

| Attribute |      Description      |    Type   | Must |
|:---------:|:---------------------:|:---------:|:----:|
|    name   | Match field in packet |   String  |  Yes |
|  relation |    Match operation    | See below |  Yes |
|  content  |     Match content     | See below |  Yes |

<h2>Name</h2>

All integer type content is accepted following prefix as carry:

* `0b, 0B`: Binary.
* `0o, 0O, 0`: Octal.
* `0x, 0X`: Hexadecimal.

|         Name        |       Relation       |     Content     |             Description            |               Example              |                                                                         Note                                                                        |
|:-------------------:|:--------------------:|:---------------:|:----------------------------------:|:----------------------------------:|:---------------------------------------------------------------------------------------------------------------------------------------------------:|
|       eth.addr      | ==, !=, >, >=, <, <= |   MAC address   |  Source or destination MAC address |    eth.addr == 02:00:00:00:00:00   |                                                                                                                                                     |
|       eth.src       | ==, !=, >, >=, <, <= |   MAC address   |         Source MAC address         |    eth.src == 02:00:00:00:00:00    |                                                                                                                                                     |
|       eth.dst       | ==, !=, >, >=, <, <= |   MAC address   |       Destination MAC address      |    eth.dst == 02:00:00:00:00:00    |                                                                                                                                                     |
|       eth.type      |          ==          |      String     |       Ethernet next protocol       |          eth.type == ipv4          | Accept keywords: `ipv4`, `arp`, `wol`, `rarp`, `ipv6`, `lldp`, `ptp`, `mpls_unicast`, `mpls_multicast`, `pppoe_discovery`, `pppoe_session`, `eapol` |
|       vlan.id       |          ==          |  0-4095 integer |               VLAN ID              |           vlan.id == 100           |                                                                                                                                                     |
|    vlan.priority    |          ==          |   0-7 integer   |            VLAN priority           |         vlan.priority == 5         |                                                                                                                                                     |
|    mpls\_unicast    |          ==          |      Empty      |           Is MPLS unicast          |         mpls\_unicast == ""        |                                                                                                                                                     |
|   mpls\_multicast   |          ==          |      Empty      |          Is MPLS multicast         |        mpls\_multicast == ""       |                                                                                                                                                     |
|         arp         |          ==          |      Empty      |               Is ARP               |              arp == ""             |                                                                                                                                                     |
|      arp.opcode     |          ==          | 0-65535 integer |         ARP operation type         |           arp.opcode == 1          |                                                                                                                                                     |
| arp.src.proto\_ipv4 | ==, !=, >, >=, <, <= |   IPv4 address  |  ARP sender protocol address(IPv4) | arp.src.proto\_ipv4 == 192.168.1.1 |                                                                     Accept CIDR                                                                     |
|       arp.spa       | ==, !=, >, >=, <, <= |   IPv4 address  |   Alias to 'arp.src.proto\_ipv4'   |      arp.spa == 192.168.1.0/24     |                                                                     Accept CIDR                                                                     |
| arp.dst.proto\_ipv4 | ==, !=, >, >=, <, <= |   IPv4 address  |  ARP target protocol address(IPv4) | arp.dst.proto\_ipv4 == 192.168.1.1 |                                                                     Accept CIDR                                                                     |
|       arp.tpa       | ==, !=, >, >=, <, <= |   IPv4 address  |   Alias to 'arp.dst.proto\_ipv4'   |      arp.tpa == 192.168.1.0/24     |                                                                     Accept CIDR                                                                     |
|         rarp        |          ==          |      Empty      |               Is RARP              |             rarp == ""             |                                                                                                                                                     |
|   pppoe_discovery   |          ==          |      Empty      |         Is PPPoE discovery         |        pppoe_discovery == ""       |                                                                                                                                                     |
|    pppoe_session    |          ==          |      Empty      |          Is PPPoE Session          |         pppoe_session == ""        |                                                                                                                                                     |
|        eapol        |          ==          |      Empty      |           Is EAP over LAN          |             eapol == ""            |                                                                                                                                                     |
|         lldp        |          ==          |      Empty      |               Is LLDP              |             lldp == ""             |                                                                                                                                                     |
|          ip         |          ==          |      Empty      |               Is IPv4              |              ip == ""              |                                                                                                                                                     |
|       ip.addr       | ==, !=, >, >=, <, <= |   IPv4 address  | Source or destination IPv4 address |       ip.addr == 192.168.1.1       |                                                                     Accept CIDR                                                                     |
|        ip.src       | ==, !=, >, >=, <, <= |   IPv4 address  |         Source IPv4 address        |        ip.src == 192.168.1.1       |                                                                     Accept CIDR                                                                     |
|        ip.dst       | ==, !=, >, >=, <, <= |   IPv4 address  |      Destination IPv4 address      |        ip.dst == 192.168.1.1       |                                                                     Accept CIDR                                                                     |
|       ip.proto      |          ==          |  0-255 interger |         IPv4 next protocol         |           ip.proto == tcp          |                 Accept keywords: `icmp`, `icmp4`, `icmpv4`, `igmp`, `tcp`, `udp`, `gre`, `icmp6`, `icmpv6`, `eigrp`, `ospf`, `sctp`                 |
|         ipv6        |          ==          |      Empty      |               Is IPv6              |             ipv6 == ""             |                                                                                                                                                     |
|      ipv6.addr      | ==, !=, >, >=, <, <= |   IPv6 address  | Source or destination IPv6 address |          ipv6.addr == ::1          |                                                                     Accept CIDR                                                                     |
|       ipv6.src      | ==, !=, >, >=, <, <= |   IPv6 address  |         Source IPv6 address        |           ipv6.src == ::1          |                                                                     Accept CIDR                                                                     |
|       ipv6.dst      | ==, !=, >, >=, <, <= |   IPv6 address  |      Destination IPv6 address      |        ipv6.dst == fe80::/64       |                                                                     Accept CIDR                                                                     |
|       ipv6.nxt      |          ==          |  0-255 interger |         IPv6 next protocol         |           ipv6.nxt == udp          |                 Accept keywords: `icmp`, `icmp4`, `icmpv4`, `igmp`, `tcp`, `udp`, `gre`, `icmp6`, `icmpv6`, `eigrp`, `ospf`, `sctp`                 |
|         tcp         |          ==          |      Empty      |               Is TCP               |              tcp == ""             |                                                                  Both IPv4 and IPv6                                                                 |
|       tcp.port      | ==, !=, >, >=, <, <= | 0-65535 integer |   Source or destination TCP port   |           tcp.port == 80           |                                                                  Both IPv4 and IPv6                                                                 |
|     tcp.srcport     | ==, !=, >, >=, <, <= | 0-65535 integer |           Source TCP port          |          tcp.srcport == 80         |                                                                  Both IPv4 and IPv6                                                                 |
|       tcp.src       | ==, !=, >, >=, <, <= | 0-65535 integer |       Alias to 'tcp.srcport'       |            tcp.src == 80           |                                                                  Both IPv4 and IPv6                                                                 |
|     tcp.dstport     | ==, !=, >, >=, <, <= | 0-65535 integer |        Destination TCP port        |         tcp.dstport == 443         |                                                                  Both IPv4 and IPv6                                                                 |
|       tcp.dst       | ==, !=, >, >=, <, <= | 0-65535 integer |       Alias to 'tcp.dstport'       |           tcp.dst == 443           |                                                                  Both IPv4 and IPv6                                                                 |
|         udp         |          ==          |      Empty      |               Is UDP               |              udp == ""             |                                                                  Both IPv4 and IPv6                                                                 |
|       udp.port      | ==, !=, >, >=, <, <= | 0-65535 integer |   Source or destination UDP port   |           udp.port == 53           |                                                                  Both IPv4 and IPv6                                                                 |
|     udp.srcport     | ==, !=, >, >=, <, <= | 0-65535 integer |           Source UDP port          |         udp.srcport == 123         |                                                                  Both IPv4 and IPv6                                                                 |
|       udp.src       | ==, !=, >, >=, <, <= | 0-65535 integer |       Alias to 'udp.srcport'       |           udp.src == 123           |                                                                  Both IPv4 and IPv6                                                                 |
|     udp.dstport     | ==, !=, >, >=, <, <= | 0-65535 integer |        Destination UDP port        |          udp.dstport == 53         |                                                                  Both IPv4 and IPv6                                                                 |
|       udp.dst       | ==, !=, >, >=, <, <= | 0-65535 integer |       Alias to 'udp.dstport'       |            udp.dst == 53           |                                                                  Both IPv4 and IPv6                                                                 |
|         sctp        |          ==          |      Empty      |               Is SCTP              |             sctp == ""             |                                                                  Both IPv4 and IPv6                                                                 |
|      sctp.port      | ==, !=, >, >=, <, <= | 0-65535 integer |   Source or destination SCTP port  |          sctp.port == 3868         |                                                                  Both IPv4 and IPv6                                                                 |
|     sctp.srcport    | ==, !=, >, >=, <, <= | 0-65535 integer |          Source SCTP port          |        sctp.srcport == 3868        |                                                                  Both IPv4 and IPv6                                                                 |
|       sctp.src      | ==, !=, >, >=, <, <= | 0-65535 integer |       Alias to 'sctp.srcport'      |          sctp.src == 3868          |                                                                  Both IPv4 and IPv6                                                                 |
|     sctp.dstport    | ==, !=, >, >=, <, <= | 0-65535 integer |        Destination SCTP port       |        sctp.dstport == 36412       |                                                                  Both IPv4 and IPv6                                                                 |
|       sctp.dst      | ==, !=, >, >=, <, <= | 0-65535 integer |       Alias to 'sctp.dstport'      |          sctp.dst == 36412         |                                                                  Both IPv4 and IPv6                                                                 |
|         icmp        |          ==          |      Empty      |              Is ICMPv4             |             icmp == ""             |                                                                                                                                                     |
|      icmp.type      |          ==          |  0-255 integer  |             ICMPv4 type            |           icmp.type == 8           |                                                                                                                                                     |
|      icmp.code      |          ==          |  0-255 integer  |             ICMPv4 code            |           icmp.code == 0           |                                                                                                                                                     |
|        icmpv6       |          ==          |      Empty      |              Is ICMPv6             |            icmpv6 == ""            |                                                                                                                                                     |
|     icmpv6.type     |          ==          |  0-255 integer  |             ICMPv6 type            |         icmpv6.type == 135         |                                                                                                                                                     |
|     icmpv6.code     |          ==          |  0-255 integer  |             ICMPv6 code            |          icmpv6.code == 0          |                                                                                                                                                     |
|         igmp        |          ==          |      Empty      |               Is IGMP              |             igmp == ""             |                                                                                                                                                     |
|       5-tuple       |          ==          |    See below    |              See below             |              See below             |                                                                                                                                                     |

<h2>5-tuple</h2>

<h3>Format</h3>

1. `[Source IP address] [Destination IP address] [IP next protocol] [Source port] [Destination port]`.
2. `[Source IP address] [Destination IP address] [ICMP/ICMPv6] [ICMP type] [ICMP code]`.
3. `[Source IP address] [Destination IP address] [IP next protocol] - -`.

* `Source IP address`: Accept CIDR.
* `Destination IP address`: Accept CIDR.
* `IP next protocol`: 0-255 integer and accept keyword: `icmp`, `icmp4`, `icmpv4`, `igmp`, `tcp`, `udp`, `gre`, `icmp6`, `icmpv6`, `eigrp`, `ospf`, `sctp`.
* `Source port`: 0-65535 integer.
* `Destination port`: 0-65535 integer.
* `ICMP type`: 0-255 integer.
* `ICMP code`: 0-255 integer.
* `-`: Don't care, but don't take advantage of `-`.

**I don't know what do you mean:**

1. `- - - 12345 80`
2. `192.168.1.1 192.168.1.2 - 12345 80`

<h3>Examples</h3>

1. `192.168.1.1 192.168.1.203 tcp 12345 80`
2. `192.168.1.1 192.168.1.203 tcp - 80`
3. `192.168.1.0/24 - 6 - 80`
4. `fe80::/64 - - - 80`
5. `192.168.1.0/24 - - - -`
6. `- - 17 12345 80`
7. `192.168.1.1 192.168.1.203 1 8 0`
8. `192.168.1.1 192.168.1.203 58 135 0`
9. `192.168.1.1 192.168.1.203 2 - -`
10. `192.168.1.1 192.168.1.203 sctp 36412 36412`
11. `192.168.1.0/24 192.168.1.203 icmpv4 8 0`
12. `- ::1 icmpv6 135 0`
13. `- ::1 icmp6 136 0`
14. `192.168.1.1 192.168.1.203 gre - -`

<h2>Combine</h2>

The names basically still the same as above, but will be expanded as described.

|       Name      |           Description          |                                                       Expansion                                                      |
|:---------------:|:------------------------------:|:--------------------------------------------------------------------------------------------------------------------:|
|       mpls      |  Is MPLS unicast and multicast |                                       eth.type == 0x8847 or eth.type == 0x8848                                       |
|      pppoe      | Is PPPoE discovery and session |                                       eth.type == 0x8863 or eth.type == 0x8864                                       |
|       gre       |             Is GRE             |                                           ip.proto == 47 or ipv6.nxt == 47                                           |
|       ospf      |             Is OSPF            |                                           ip.proto == 89 or ipv6.nxt == 89                                           |
|      eigrp      |            Is EIGRP            |                                           ip.proto == 88 or ipv6.nxt == 88                                           |
|       ptp       |   Is PTP included over UDP/IP  |                               eth.type == 0x88f7 or udp.port == 319 or udp.port == 320                               |
|       ndp       |             Is NDP             |      icmpv6.type == 133 or icmpv6.type == 134 or icmpv6.type == 135 or icmpv6.type == 136 or icmpv6.type == 137      |
|       bgp       |             Is BGP             |                                                    tcp.port == 179                                                   |
|       rip       |           Is RIPv1/v2          |                                                    udp.port == 520                                                   |
|      ripng      |            Is RIPng            |                                                    udp.port == 521                                                   |
|       dns       |           Is DNS/mDNS          |              udp.port == 53 or tcp.port == 53 or udp.port == 5353 or tcp.port == 5353 or sctp.port == 53             |
|       doh       |        Is DNS over HTTPS       |                                                    tcp.port == 853                                                   |
|       dot       |         Is DNS over TLS        |                                                    tcp.port == 853                                                   |
|       http      |             Is HTTP            |                                                    tcp.port == 80                                                    |
|      https      |            Is HTTPS            |                                                    tcp.port == 443                                                   |
|       quic      |            Is iQUIC            |                                                    udp.port == 443                                                   |
|       ssh       |             Is SSH             |                                           tcp.port == 22 or sctp.port == 22                                          |
|       sftp      |             Is SFTP            |                                                    tcp.port == 22                                                    |
|       ftp       |         Is FTP/FTP-data        |                                           udp.port == 20 or udp.port == 21                                           |
|       l2tp      |             Is L2TP            |                                                   udp.port == 1701                                                   |
|     openvpn     |           Is OpenVPN           |                                         udp.port == 1194 or tcp.port == 1194                                         |
|       ntp       |             Is NTP             |                                          udp.port == 123 or tcp.port == 123                                          |
|      telnet     |            Is Telnet           |                                                    tcp.port == 23                                                    |
|       smtp      |             Is SMTP            |                                                    tcp.port == 25                                                    |
|      smtps      |        Is SMTP over TLS        |                                                    tcp.port == 465                                                   |
| smtp_submission |        Is SMTP encrypted       |                                                    tcp.port == 587                                                   |
|       pop3      |             Is POP3            |                                                    tcp.port == 110                                                   |
|      pop3s      |            Is POP3S            |                                                    tcp.port == 995                                                   |
|       imap      |             Is IMAP            |                                                    tcp.port == 143                                                   |
|      imaps      |        Is IMAP over TLS        |                                                    tcp.port == 993                                                   |
|      whois      |            Is WHOIS            |                                                    tcp.port == 43                                                    |
|       dhcp      |        Is DHCP and proxy       |                                 udp.port == 67 or udp.port == 68 or udp.port == 4011                                 |
|      dhcpv6     |            Is DHCPv6           |                                 udp.port == 546 or udp.port == 547 or tcp.port == 547                                |
|       tftp      |             Is TFTP            |                                                    udp.port == 69                                                    |
|     netbios     |           Is Netbios           |                       tcp.port == 139 or tcp.port == 445 or udp.port == 137 or udp.port == 138                       |
|       snmp      |             Is SNMP            |                                          udp.port == 161 or tcp.port == 161                                          |
|     snmptrap    |          Is SNMP trap          |                                          udp.port == 162 or tcp.port == 162                                          |
|       smux      |      Is SNMP Multiplexing      |                                                    tcp.port == 199                                                   |
|   snmp_patrol   |         Is SNMP Patrol         |                                                   udp.port == 8161                                                   |
|      syslog     |            Is Syslog           |                                                    udp.port == 514                                                   |
|     netflow     |           Is Netflow           |           udp.port == 2055 or udp.port == 9555 or udp.port == 9995 or udp.port == 9025 or udp.port == 9026           |
|      ipfix      |            Is IPFIX            |                                         udp.port == 4739 or tcp.port == 4739                                         |
|       ssdp      |             Is SSDP            |                                         udp.port == 1900 or tcp.port == 1900                                         |
|      radius     |            Is Radius           | udp.port == 1812 or udp.port == 1813 or udp.port == 3799 or udp.port == 1645 or udp.port == 1646 or udp.port == 1700 |
|       hsrp      |          Is HSRPv1/v2          |                                         udp.port == 1985 or udp.port == 2029                                         |
|       rdp       |             Is RDP             |                                         tcp.port == 3389 or udp.port == 3389                                         |
|      vxlan      |            Is VXLAN            |                                                   udp.port == 4789                                                   |
|    vxlan_gpe    |          Is VXLAN-GPE          |                                                   udp.port == 4790                                                   |
|       sip       |             Is SIP             |                                         tcp.port == 5060 or udp.port == 5060                                         |
|       sips      |         Is SIP over TLS        |                                                   tcp.port == 5061                                                   |
|       tor       |             Is Tor             |                                         tcp.port == 9001 or tcp.port == 9030                                         |
|      gtpv0      |            Is GTPv0            |                                         udp.port == 3386 or tcp.port == 3386                                         |
|  gtpv1_control  |    Is GTPv1 control channel    |                                         udp.port == 2123 or tcp.port == 2123                                         |
|    gtpv1_data   |      Is GTPv1 data channel     |                                         udp.port == 2152 or tcp.port == 2152                                         |
|     diameter    |           Is Diameter          |                                         sctp.port == 3868 or tcp.port == 3868                                        |
|       s1ap      |             Is S1AP            |                                                  sctp.port == 36412                                                  |

<h2>Example 1</h2>

```
<run>

<filter id="1">
<or>
    <find name="eth.src" relation="==" content="02:00:00:00:00:00"/>
    <find name="ip.addr" relation="==" content="172.16.95.0/24"/>
    <find name="ip.addr" relation="==" content="10.10.8.0/24"/>
    <find name="udp.port" relation="==" content="53"/>
</or>
</filter>

</run>
```

<h2>Example 2</h2>

```
<run>

<filter id="2">
<or>
    <find name="ip.addr" relation="==" content="8.8.8.8"/>
    <find name="ip.addr" relation="==" content="8.8.4.4"/>
    <find name="ip.addr" relation="==" content="1.1.1.1"/>
    <find name="ip.addr" relation="==" content="1.0.0.1"/>
</or>
</filter>

</run>
```
