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

|         Name        |       Relation       |     Content     |             Description            |               Example              |                                                      Note                                                     |
|:-------------------:|:--------------------:|:---------------:|:----------------------------------:|:----------------------------------:|:-------------------------------------------------------------------------------------------------------------:|
|       eth.addr      | ==, !=, >, >=, <, <= |   MAC address   |  Source or destination MAC address |    eth.addr == 02:00:00:00:00:00   |                                                                                                               |
|       eth.src       | ==, !=, >, >=, <, <= |   MAC address   |         Source MAC address         |    eth.src == 02:00:00:00:00:00    |                                                                                                               |
|       eth.dst       | ==, !=, >, >=, <, <= |   MAC address   |       Destination MAC address      |    eth.dst == 02:00:00:00:00:00    |                                                                                                               |
|       eth.type      |          ==          |      String     |       Ethernet next protocol       |          eth.type == ipv4          | Accept keyword: `ipv4`, `arp`, `wol`, `rarp`, `ipv6`, `lldp`, `ptp`, `mpls`, `mpls_unicast`, `mpls_multicast` |
|       vlan.id       |          ==          |  0-4095 integer |               VLAN ID              |           vlan.id == 100           |                                                                                                               |
|    vlan.priority    |          ==          |   0-7 integer   |            VLAN priority           |         vlan.priority == 5         |                                                                                                               |
|         mpls        |          ==          |      Empty      |               Is MPLS              |               mpls ==              |                                                                                                               |
|    mpls\_unicast    |          ==          |      Empty      |           Alias to 'mpls'          |          mpls\_unicast ==          |                                                                                                               |
|   mpls\_multicast   |          ==          |      Empty      |          Is MPLS multicast         |         mpls\_multicast ==         |                                                                                                               |
|         arp         |          ==          |      Empty      |               Is ARP               |               arp ==               |                                                                                                               |
|      arp.opcode     |          ==          | 0-65535 integer |         ARP operation type         |           arp.opcode == 1          |                                                                                                               |
| arp.src.proto\_ipv4 | ==, !=, >, >=, <, <= |   IPv4 address  |  ARP sender protocol address(IPv4) | arp.src.proto\_ipv4 == 192.168.1.1 |                                                  Accept CIDR                                                  |
|       arp.spa       | ==, !=, >, >=, <, <= |   IPv4 address  |   Alias to 'arp.src.proto\_ipv4'   |      arp.spa == 192.168.1.0/24     |                                                  Accept CIDR                                                  |
| arp.dst.proto\_ipv4 | ==, !=, >, >=, <, <= |   IPv4 address  |  ARP target protocol address(IPv4) | arp.dst.proto\_ipv4 == 192.168.1.1 |                                                  Accept CIDR                                                  |
|       arp.tpa       | ==, !=, >, >=, <, <= |   IPv4 address  |   Alias to 'arp.dst.proto\_ipv4'   |      arp.tpa == 192.168.1.0/24     |                                                  Accept CIDR                                                  |
|          ip         |          ==          |      Empty      |               Is IPv4              |                ip ==               |                                                                                                               |
|       ip.addr       | ==, !=, >, >=, <, <= |   IPv4 address  | Source or destination IPv4 address |       ip.addr == 192.168.1.1       |                                                  Accept CIDR                                                  |
|        ip.src       | ==, !=, >, >=, <, <= |   IPv4 address  |         Source IPv4 address        |        ip.src == 192.168.1.1       |                                                  Accept CIDR                                                  |
|        ip.dst       | ==, !=, >, >=, <, <= |   IPv4 address  |      Destination IPv4 address      |        ip.dst == 192.168.1.1       |                                                  Accept CIDR                                                  |
|       ip.proto      |          ==          |  0-255 interger |         IPv4 next protocol         |           ip.proto == tcp          |           Accept keyword: `icmp`, `icmp4`, `icmpv4`, `tcp`, `udp`, `gre`, `icmp6`, `icmpv6`, `sctp`           |
|         ipv6        |          ==          |      Empty      |               Is IPv6              |               ipv6 ==              |                                                                                                               |
|      ipv6.addr      | ==, !=, >, >=, <, <= |   IPv6 address  | Source or destination IPv6 address |          ipv6.addr == ::1          |                                                  Accept CIDR                                                  |
|       ipv6.src      | ==, !=, >, >=, <, <= |   IPv6 address  |         Source IPv6 address        |           ipv6.src == ::1          |                                                  Accept CIDR                                                  |
|       ipv6.dst      | ==, !=, >, >=, <, <= |   IPv6 address  |      Destination IPv6 address      |        ipv6.dst == fe80::/64       |                                                  Accept CIDR                                                  |
|       ipv6.nxt      |          ==          |  0-255 interger |         IPv6 next protocol         |           ipv6.nxt == udp          |           Accept keyword: `icmp`, `icmp4`, `icmpv4`, `tcp`, `udp`, `gre`, `icmp6`, `icmpv6`, `sctp`           |
|         tcp         |          ==          |      Empty      |               Is TCP               |               tcp ==               |                                               Both IPv4 and IPv6                                              |
|       tcp.port      | ==, !=, >, >=, <, <= | 0-65535 integer |   Source or destination TCP port   |           tcp.port == 80           |                                               Both IPv4 and IPv6                                              |
|     tcp.srcport     | ==, !=, >, >=, <, <= | 0-65535 integer |           Source TCP port          |          tcp.srcport == 80         |                                               Both IPv4 and IPv6                                              |
|       tcp.src       | ==, !=, >, >=, <, <= | 0-65535 integer |       Alias to 'tcp.srcport'       |            tcp.src == 80           |                                               Both IPv4 and IPv6                                              |
|     tcp.dstport     | ==, !=, >, >=, <, <= | 0-65535 integer |        Destination TCP port        |         tcp.dstport == 443         |                                               Both IPv4 and IPv6                                              |
|       tcp.dst       | ==, !=, >, >=, <, <= | 0-65535 integer |       Alias to 'tcp.dstport'       |           tcp.dst == 443           |                                               Both IPv4 and IPv6                                              |
|         udp         |          ==          |      Empty      |               Is UDP               |               udp ==               |                                               Both IPv4 and IPv6                                              |
|       udp.port      | ==, !=, >, >=, <, <= | 0-65535 integer |   Source or destination UDP port   |           udp.port == 53           |                                               Both IPv4 and IPv6                                              |
|     udp.srcport     | ==, !=, >, >=, <, <= | 0-65535 integer |           Source UDP port          |         udp.srcport == 123         |                                               Both IPv4 and IPv6                                              |
|       udp.src       | ==, !=, >, >=, <, <= | 0-65535 integer |       Alias to 'udp.srcport'       |           udp.src == 123           |                                               Both IPv4 and IPv6                                              |
|     udp.dstport     | ==, !=, >, >=, <, <= | 0-65535 integer |        Destination UDP port        |          udp.dstport == 53         |                                               Both IPv4 and IPv6                                              |
|       udp.dst       | ==, !=, >, >=, <, <= | 0-65535 integer |       Alias to 'udp.dstport'       |            udp.dst == 53           |                                               Both IPv4 and IPv6                                              |
|         sctp        |          ==          |      Empty      |               Is SCTP              |               sctp ==              |                                               Both IPv4 and IPv6                                              |
|      sctp.port      | ==, !=, >, >=, <, <= | 0-65535 integer |   Source or destination SCTP port  |          sctp.port == 3868         |                                               Both IPv4 and IPv6                                              |
|     sctp.srcport    | ==, !=, >, >=, <, <= | 0-65535 integer |          Source SCTP port          |        sctp.srcport == 3868        |                                               Both IPv4 and IPv6                                              |
|       sctp.src      | ==, !=, >, >=, <, <= | 0-65535 integer |       Alias to 'sctp.srcport'      |          sctp.src == 3868          |                                               Both IPv4 and IPv6                                              |
|     sctp.dstport    | ==, !=, >, >=, <, <= | 0-65535 integer |        Destination SCTP port       |        sctp.dstport == 36412       |                                               Both IPv4 and IPv6                                              |
|       sctp.dst      | ==, !=, >, >=, <, <= | 0-65535 integer |       Alias to 'sctp.dstport'      |          sctp.dst == 36412         |                                               Both IPv4 and IPv6                                              |
|         icmp        |          ==          |      Empty      |              Is ICMPv4             |               icmp ==              |                                                                                                               |
|      icmp.type      |          ==          |  0-255 integer  |             ICMPv4 type            |           icmp.type == 8           |                                                                                                               |
|      icmp.code      |          ==          |  0-255 integer  |             ICMPv4 code            |           icmp.code == 0           |                                                                                                               |
|        icmpv6       |          ==          |      Empty      |              Is ICMPv6             |              icmpv6 ==             |                                                                                                               |
|     icmpv6.type     |          ==          |  0-255 integer  |             ICMPv6 type            |         icmpv6.type == 135         |                                                                                                               |
|     icmpv6.code     |          ==          |  0-255 integer  |             ICMPv6 code            |          icmpv6.code == 0          |                                                                                                               |
|       5-tuple       |          ==          |    See below    |              See below             |              See below             |                                                                                                               |
|         gre         |          ==          |      Empty      |               Is GRE               |               gre ==               |                                                                                                               |

<h2>5-tuple</h2>

<h3>Format</h3>

1. `[Source IP address] [Destination IP address] [IP next protocol] [Source port] [Destination port]`.
2. `[Source IP address] [Destination IP address] [IP next protocol] [ICMP type] [ICMP code]`.

* `Source IP address`: Accept CIDR.
* `Destination IP address`: Accept CIDR.
* `IP next protocol`: 0-255 integer and accept keyword: `icmp`, `icmp4`, `icmpv4`, `tcp`, `udp`, `gre`, `icmp6`, `icmpv6`, `sctp`.
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
