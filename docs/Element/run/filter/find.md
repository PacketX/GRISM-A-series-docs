Find
============

`<find>` tag described what exact field and content to match with.

<h2>Attribute</h2>

| Attribute |      Description      |    Type   | Must |
|:---------:|:---------------------:|:---------:|:----:|
|    name   | Match field in packet |   String  |  Yes |
|  relation |    Match operation    | See below |  Yes |
|  content  |     Match content     | See below |  Yes |

<h2 id="name">Name</h2>

All integer type content is accepted following prefix as carry:

* `0b, 0B`: Binary.
* `0o, 0O, 0`: Octal.
* `0x, 0X`: Hexadecimal.

|         Name         |       Relation       |     Content     |                   Description                  |               Example              |                      Note                      |
|:--------------------:|:--------------------:|:---------------:|:----------------------------------------------:|:----------------------------------:|:----------------------------------------------:|
|       eth.addr       | ==, !=, >, >=, <, <= |   MAC address   |        Source or destination MAC address       |    eth.addr == 02:00:00:00:00:00   |                                                |
|        eth.src       | ==, !=, >, >=, <, <= |   MAC address   |               Source MAC address               |    eth.src == 02:00:00:00:00:00    |                                                |
|        eth.dst       | ==, !=, >, >=, <, <= |   MAC address   |             Destination MAC address            |    eth.dst == 02:00:00:00:00:00    |                                                |
|       eth.type       |          ==          |      String     |             Ethernet next protocol             |          eth.type == ipv4          |                   See below.                   |
|       eth.bcast      |        ==, !=        |      Empty      |      Destination MAC address is broadcast      |           eth.bcast == ""          |                                                |
|       eth.mcast      |        ==, !=        |      Empty      |    Destination MAC address is IPv4 multicast   |           eth.mcast == ""          |                                                |
|      eth.mcastv6     |        ==, !=        |      Empty      |    Destination MAC address is IPv6 multicast   |          eth.mcastv6 == ""         |                                                |
|        vlan.id       |          ==          |  0-4095 integer |                     VLAN ID                    |           vlan.id == 100           |                                                |
|     vlan.priority    |          ==          |   0-7 integer   |                  VLAN priority                 |         vlan.priority == 5         |                                                |
|       vlan.dei       |        ==, !=        |   0-1 integer   |                    VLAN DEI                    |            vlan.dei == 1           |          Conflicts with `match mode`.          |
|       vlan.tci       |          ==          | 0-65535 integer |                    VLAN TCI                    |         vlan.tci == 0x0010         |          Conflicts with `match mode`.          |
|     mpls\_unicast    |          ==          |      Empty      |                 Is MPLS unicast                |         mpls\_unicast == ""        |                                                |
|    mpls\_multicast   |          ==          |      Empty      |                Is MPLS multicast               |        mpls\_multicast == ""       |                                                |
|          arp         |          ==          |      Empty      |                     Is ARP                     |              arp == ""             |                                                |
|      arp.opcode      |          ==          | 0-65535 integer |               ARP operation type               |           arp.opcode == 1          |                                                |
|  arp.src.proto\_ipv4 | ==, !=, >, >=, <, <= |   IPv4 address  |        ARP sender protocol address(IPv4)       | arp.src.proto\_ipv4 == 192.168.1.1 |                  Accept CIDR.                  |
|        arp.spa       | ==, !=, >, >=, <, <= |   IPv4 address  |         Alias to 'arp.src.proto\_ipv4'         |      arp.spa == 192.168.1.0/24     |                  Accept CIDR.                  |
|  arp.dst.proto\_ipv4 | ==, !=, >, >=, <, <= |   IPv4 address  |        ARP target protocol address(IPv4)       | arp.dst.proto\_ipv4 == 192.168.1.1 |                  Accept CIDR.                  |
|        arp.tpa       | ==, !=, >, >=, <, <= |   IPv4 address  |         Alias to 'arp.dst.proto\_ipv4'         |      arp.tpa == 192.168.1.0/24     |                  Accept CIDR.                  |
|         rarp         |          ==          |      Empty      |                     Is RARP                    |             rarp == ""             |                                                |
|    pppoe_discovery   |          ==          |      Empty      |               Is PPPoE discovery               |        pppoe_discovery == ""       |                                                |
|     pppoe_session    |          ==          |      Empty      |                Is PPPoE Session                |         pppoe_session == ""        |                                                |
|         eapol        |          ==          |      Empty      |                 Is EAP over LAN                |             eapol == ""            |                                                |
|         lldp         |          ==          |      Empty      |                     Is LLDP                    |             lldp == ""             |                                                |
|     slow_protocol    |          ==          |      Empty      |                Is Slow Protocol                |         slow_protocol == ""        |                                                |
|          ip          |          ==          |      Empty      |                     Is IPv4                    |              ip == ""              |                                                |
|        ip.addr       | ==, !=, >, >=, <, <= |   IPv4 address  |       Source or destination IPv4 address       |       ip.addr == 192.168.1.1       |                  Accept CIDR.                  |
|        ip.src        | ==, !=, >, >=, <, <= |   IPv4 address  |               Source IPv4 address              |        ip.src == 192.168.1.1       |                  Accept CIDR.                  |
|        ip.dst        | ==, !=, >, >=, <, <= |   IPv4 address  |            Destination IPv4 address            |        ip.dst == 192.168.1.1       |                  Accept CIDR.                  |
|       ip.proto       |          ==          |  0-255 interger |               IPv4 next protocol               |           ip.proto == tcp          |                   See below.                   |
|       ip.mcast       |          ==          |      Empty      |          Destination is IPv4 multicast         |           ip.mcast == ""           |                   See below.                   |
|     ip.src_mcast     |          ==          |      Empty      |            Source is IPv4 multicast            |         ip.src_mcast == ""         |                   See below.                   |
|      ip.fragment     |          ==          |      Empty      |                  IPv4 fragment                 |        ip.fragment == "yes"        |     Conflicts with `match mode`. See below.    |
|         ipv6         |          ==          |      Empty      |                     Is IPv6                    |             ipv6 == ""             |                                                |
|       ipv6.addr      | ==, !=, >, >=, <, <= |   IPv6 address  |       Source or destination IPv6 address       |          ipv6.addr == ::1          | Conflicts with none `match mode`. Accept CIDR. |
|       ipv6.src       | ==, !=, >, >=, <, <= |   IPv6 address  |               Source IPv6 address              |           ipv6.src == ::1          | Conflicts with none `match mode`. Accept CIDR. |
|       ipv6.dst       | ==, !=, >, >=, <, <= |   IPv6 address  |            Destination IPv6 address            |        ipv6.dst == fe80::/64       | Conflicts with none `match mode`. Accept CIDR. |
|       ipv6.nxt       |          ==          |  0-255 interger |               IPv6 next protocol               |           ipv6.nxt == udp          |                   See below.                   |
|    ipv6.mcast_rsvd   |        ==, !=        |      Empty      |             Is IPv6 multicast group            |        ipv6.mcast_rsvd == ""       |  Conflicts with none `match mode`. See below.  |
| ipv6.mcast_all_nodes |          ==          |      Empty      |             Is IPv6 multicast group            |     ipv6.mcast_all_nodes == ""     |  Conflicts with none `match mode`. See below.  |
|  ipv6.mcast_all_rtrs |          ==          |      Empty      |             Is IPv6 multicast group            |      ipv6.mcast_all_rtrs == ""     |  Conflicts with none `match mode`. See below.  |
|  ipv6.mcast_sol_node |        ==, !=        |      Empty      |             Is IPv6 multicast group            |      ipv6.mcast_sol_node == ""     |  Conflicts with none `match mode`. See below.  |
|   ipv6.mcast_flood   |          ==          |      Empty      |             Is IPv6 multicast group            |       ipv6.mcast_flood == ""       |  Conflicts with none `match mode`. See below.  |
|      ipv6.mcast      |          ==          |      Empty      |             Is IPv6 multicast group            |          ipv6.mcast == ""          |  Conflicts with none `match mode`. See below.  |
|         mldv1        |          ==          |      Empty      |             Is IPv6 multicast group            |             mldv1 == ""            |  Conflicts with none `match mode`. See below.  |
|         mldv2        |          ==          |      Empty      |             Is IPv6 multicast group            |             mldv2 == ""            |  Conflicts with none `match mode`. See below.  |
|          tcp         |          ==          |      Empty      |                     Is TCP                     |              tcp == ""             |               Both IPv4 and IPv6.              |
|       tcp.port       | ==, !=, >, >=, <, <= | 0-65535 integer |         Source or destination TCP port         |           tcp.port == 80           |               Both IPv4 and IPv6.              |
|      tcp.srcport     | ==, !=, >, >=, <, <= | 0-65535 integer |                 Source TCP port                |          tcp.srcport == 80         |               Both IPv4 and IPv6.              |
|        tcp.src       | ==, !=, >, >=, <, <= | 0-65535 integer |             Alias to 'tcp.srcport'             |            tcp.src == 80           |               Both IPv4 and IPv6.              |
|      tcp.dstport     | ==, !=, >, >=, <, <= | 0-65535 integer |              Destination TCP port              |         tcp.dstport == 443         |               Both IPv4 and IPv6.              |
|        tcp.dst       | ==, !=, >, >=, <, <= | 0-65535 integer |             Alias to 'tcp.dstport'             |           tcp.dst == 443           |               Both IPv4 and IPv6.              |
|    tcp.flags.[800]   |        ==, !=        |      Empty      |      TCP flag reserved highest bit is set      |        tcp.flags.[800] == ""       |          Conflicts with `match mode`.          |
|    tcp.flags.[400]   |        ==, !=        |      Empty      |       TCP flag reserved second bit is set      |        tcp.flags.[400] == ""       |          Conflicts with `match mode`.          |
|    tcp.flags.[200]   |        ==, !=        |      Empty      |       TCP flag reserved lowest bit is set      |        tcp.flags.[200] == ""       |          Conflicts with `match mode`.          |
|     tcp.flags.ns     |        ==, !=        |      Empty      |               TCP flag NS is set               |         tcp.flags.ns == ""         |          Conflicts with `match mode`.          |
|     tcp.flags.cwr    |        ==, !=        |      Empty      |               TCP flag CWR is set              |         tcp.flags.cwr == ""        |          Conflicts with `match mode`.          |
|     tcp.flags.ecn    |        ==, !=        |      Empty      |               TCP flag ECN is set              |         tcp.flags.ecn == ""        |          Conflicts with `match mode`.          |
|     tcp.flags.ece    |        ==, !=        |      Empty      | TCP flag ECE is set (Alias to 'tcp.flags.ecn') |         tcp.flags.ece == ""        |          Conflicts with `match mode`.          |
|     tcp.flags.urg    |        ==, !=        |      Empty      |               TCP flag URG is set              |         tcp.flags.urg == ""        |          Conflicts with `match mode`.          |
|     tcp.flags.ack    |        ==, !=        |      Empty      |               TCP flag ACK is set              |         tcp.flags.ack == ""        |          Conflicts with `match mode`.          |
|    tcp.flags.push    |        ==, !=        |      Empty      |              TCP flag PUSH is set              |        tcp.flags.push == ""        |          Conflicts with `match mode`.          |
|     tcp.flags.psh    |        ==, !=        |      Empty      |            Alias to 'tcp.flags.push'           |         tcp.flags.psh == ""        |          Conflicts with `match mode`.          |
|    tcp.flags.reset   |        ==, !=        |      Empty      |              TCP flag RESET is set             |        tcp.flags.reset == ""       |          Conflicts with `match mode`.          |
|     tcp.flags.rst    |        ==, !=        |      Empty      |           Alias to 'tcp.flags.reset'           |         tcp.flags.rst == ""        |          Conflicts with `match mode`.          |
|     tcp.flags.syn    |        ==, !=        |      Empty      |               TCP flag SYN is set              |         tcp.flags.syn == ""        |          Conflicts with `match mode`.          |
|     tcp.flags.fin    |        ==, !=        |      Empty      |               TCP flag FIN is set              |         tcp.flags.fin == ""        |          Conflicts with `match mode`.          |
|       tcp.flags      |        ==, !=        |  TCP flags set  |                  TCP flags set                 |       tcp.flags == "+syn-ack"      |     Conflicts with `match mode`. See below.    |
|          udp         |          ==          |      Empty      |                     Is UDP                     |              udp == ""             |               Both IPv4 and IPv6.              |
|       udp.port       | ==, !=, >, >=, <, <= | 0-65535 integer |         Source or destination UDP port         |           udp.port == 53           |               Both IPv4 and IPv6.              |
|      udp.srcport     | ==, !=, >, >=, <, <= | 0-65535 integer |                 Source UDP port                |         udp.srcport == 123         |               Both IPv4 and IPv6.              |
|        udp.src       | ==, !=, >, >=, <, <= | 0-65535 integer |             Alias to 'udp.srcport'             |           udp.src == 123           |               Both IPv4 and IPv6.              |
|      udp.dstport     | ==, !=, >, >=, <, <= | 0-65535 integer |              Destination UDP port              |          udp.dstport == 53         |               Both IPv4 and IPv6.              |
|        udp.dst       | ==, !=, >, >=, <, <= | 0-65535 integer |             Alias to 'udp.dstport'             |            udp.dst == 53           |               Both IPv4 and IPv6.              |
|         sctp         |          ==          |      Empty      |                     Is SCTP                    |             sctp == ""             |               Both IPv4 and IPv6.              |
|       sctp.port      | ==, !=, >, >=, <, <= | 0-65535 integer |         Source or destination SCTP port        |          sctp.port == 3868         |               Both IPv4 and IPv6.              |
|     sctp.srcport     | ==, !=, >, >=, <, <= | 0-65535 integer |                Source SCTP port                |        sctp.srcport == 3868        |               Both IPv4 and IPv6.              |
|       sctp.src       | ==, !=, >, >=, <, <= | 0-65535 integer |             Alias to 'sctp.srcport'            |          sctp.src == 3868          |               Both IPv4 and IPv6.              |
|     sctp.dstport     | ==, !=, >, >=, <, <= | 0-65535 integer |              Destination SCTP port             |        sctp.dstport == 36412       |               Both IPv4 and IPv6.              |
|       sctp.dst       | ==, !=, >, >=, <, <= | 0-65535 integer |             Alias to 'sctp.dstport'            |          sctp.dst == 36412         |               Both IPv4 and IPv6.              |
|         icmp         |          ==          |      Empty      |                    Is ICMPv4                   |             icmp == ""             |                                                |
|       icmp.type      |          ==          |  0-255 integer  |                   ICMPv4 type                  |           icmp.type == 8           |                                                |
|       icmp.code      |          ==          |  0-255 integer  |                   ICMPv4 code                  |           icmp.code == 0           |                                                |
|        icmpv6        |          ==          |      Empty      |                    Is ICMPv6                   |            icmpv6 == ""            |                                                |
|      icmpv6.type     |          ==          |  0-255 integer  |                   ICMPv6 type                  |         icmpv6.type == 135         |                                                |
|      icmpv6.code     |          ==          |  0-255 integer  |                   ICMPv6 code                  |          icmpv6.code == 0          |                                                |
|         igmp         |          ==          |      Empty      |                     Is IGMP                    |             igmp == ""             |                                                |
|        5-tuple       |          ==          |    See below    |                    See below                   |              See below             |                                                |

<h2>eth.type</h2>

<h3>Keywords</h3>

1. `ipv4`
2. `arp`
3. `wol`
4. `rarp`
5. `ipv6`
6. `lldp`
7. `ptp`
8. `mpls_unicast`
9. `mpls_multicast`
10. `pppoe_discovery`
11. `pppoe_session`
12. `eapol`
13. `slow_protocol`

<h2>ip.proto / ipv6.nxt</h2>

<h3>Keywords</h3>

1. `icmp`
2. `icmp4`
3. `icmpv4`
4. `igmp`
5. `tcp`
6. `udp`
7. `gre`
8. `icmp6`
9. `icmpv6`
10. `eigrp`
11. `ospf`
12. `sctp`
13. `vrrp`

<h2>ip.mcast</h2>

Will be expand as:

```
eth.dst == 01:00:00:00:00:00 and ip.dst == 224.0.0.0/4
```

<h2>ip.src_mcast</h2>

Will be expand as:

```
ip.src == 224.0.0.0/4
```

<h2>ip.fragment</h2>

<h3>Keywords</h3>

1. `no`
2. `yes`
3. `first`
4. `later`
5. `not_later`

<h2>ipv6.mcast_rsvd</h2>

Will be expand as:

```
ipv6.dst == ff00::/fff0:ffff:ffff:ffff:ffff:ffff:ffff:ffff
```

<h2>ipv6.mcast_all_nodes</h2>

Will be expand as:

```
ipv6.dst == ff01::1 or ipv6.dst == ff02::1
```

<h2>ipv6.mcast_all_rtrs</h2>

Will be expand as:

```
ipv6.dst == ff01::2 or ipv6.dst == ff02::2 or ipv6.dst == ff05::2
```

<h2>ipv6.mcast_sol_node</h2>

Will be expand as:

```
ipv6.dst == ff02::1:ff00:0/104
```

<h2>ipv6.mcast_flood</h2>

Will be expand as:

```
(eth.dst == 33:33:00:00:00:00/ff:ff:00:00:00:00 and ipv6.dst == ff00::/fff0:ffff:ffff:ffff:ffff:ffff:ffff:ffff) or (eth.dst == 33:33:00:00:00:00/ff:ff:00:00:00:00 and ipv6.dst == ff01::1) or (eth.dst == 33:33:00:00:00:00/ff:ff:00:00:00:00 and ipv6.dst == ff01::2) or (eth.dst == 33:33:00:00:00:00/ff:ff:00:00:00:00 and ipv6.dst == ff02::1) or (eth.dst == 33:33:00:00:00:00/ff:ff:00:00:00:00 and ipv6.dst == ff02::1:ff00:0/104) or (eth.dst == 33:33:00:00:00:00/ff:ff:00:00:00:00 and ipv6.dst == ff02::2) or (eth.dst == 33:33:00:00:00:00/ff:ff:00:00:00:00 and ipv6.dst == ff05::2)
```

<h2>ipv6.mcast</h2>

Will be expand as:

```
eth.dst == 33:33:00:00:00:00/ff:ff:00:00:00:00 and ipv6.dst == ff00::/8
```

<h2>mldv1</h2>

Will be expand as:

```
(ipv6.src == fe80::/10 and icmpv6.type == 130) or (ipv6.src == fe80::/10 and icmpv6.type == 131) or (ipv6.src == fe80::/10 and icmpv6.type == 132)
```

<h2>mldv2</h2>

Will be expand as:

```
ipv6.dst == ff02::16 and icmpv6.type == 143
```

<h2>tcp.flags</h2>

<h3>Keywords</h3>

1. `[800]`
2. `[400]`
3. `[200]`
4. `ns`
5. `cwr`
6. `ecn`
7. `ece`
8. `urg`
9. `ack`
10. `push`
11. `psh`
12. `reset`
13. `rst`
14. `syn`
15. `fin`

Combine the prefix `+` or `-` represent must set or must not set. Take an example `+syn-ack` means must set `SYN` flag and must not set `ACK` and other flags is don't care.

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
3. `192.168.1.1 192.168.1.2 icmpv6 8 -`

<h3>Examples</h3>

1. `192.168.1.1 192.168.1.203 tcp 12345 80`
2. `192.168.1.1 192.168.1.203 tcp - 80`
3. `192.168.1.0/24 - 6 - 80`
4. `fe80::/64 - - - 80`
5. `192.168.1.0/24 - - - -`
6. `- - 17 12345 80`
7. `192.168.1.1 192.168.1.203 1 8 0`
8. `192.168.1.1 192.168.1.203 2 - -`
9. `192.168.1.1 192.168.1.203 sctp 36412 36412`
10. `192.168.1.0/24 192.168.1.203 icmpv4 8 0`
11. `- ::1 icmpv6 135 0`
12. `- ::1 icmp6 136 0`
13. `192.168.1.1 192.168.1.203 gre - -`

<h2 id="combine">Combine</h2>

The names basically still the same as above, but will be expanded as described.

**The `relation` and `content` only can be `"=="` and `""`.**

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
|       dns       |             Is DNS             |                        udp.port == 53 or tcp.port == 53 or tcp.port == 5353 or sctp.port == 53                       |
|       mdns      |             Is mDNS            |                                                   udp.port == 5353                                                   |
|      llmnr      |            Is LLMNR            |                                                   udp.port == 5353                                                   |
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
|       x2ap      |             Is X2AP            |                                                  sctp.port == 36422                                                  |
|       ngap      |             Is NGAP            |                                                  sctp.port == 38412                                                  |

<h2 id="match_mode">Match mode</h2>

Those `<find>` tags are mutually exclusive. It means cannot be used together in the **whole task**.

There is three `match mode` type:

1. `None match mode`
2. `Match mode(prefer v4)`
3. `Match mode(prefer v6)`

<h3>None match mode tags</h3>

* `vlan.dei`
* `vlan.tci`
* `ip.fragment`
* `ipv6.fragment`
* `tcp.flags.[800]`
* `tcp.flags.[400]`
* `tcp.flags.[200]`
* `tcp.flags.ns`
* `tcp.flags.cwr`
* `tcp.flags.ecn`
* `tcp.flags.ece`
* `tcp.flags.urg`
* `tcp.flags.ack`
* `tcp.flags.push`
* `tcp.flags.psh`
* `tcp.flags.reset`
* `tcp.flags.rst`
* `tcp.flags.syn`
* `tcp.flags.fin`
* `tcp.flags`

<h3>Match mode tags</h3>

* `ipv6.addr`
* `ipv6.src`
* `ipv6.dst`
* `ipv6.mcast_rsvd`
* `ipv6.mcast_all_nodes`
* `ipv6.mcast_all_rtrs`
* `ipv6.mcast_sol_node`
* `ipv6.mcast_flood`
* `ipv6.mcast`
* `mldv1`
* `mldv2`

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
