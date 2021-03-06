EN-DC
============

For `EN-DC`(`E-UTRA-NR - Dual Connectivity`) solution.

<h2>Child Elements</h2>

|     Eelment     |   Content  |           Description           | Must |               Note               |
|:---------------:|:----------:|:-------------------------------:|:----:|:--------------------------------:|
| enb\_cell\_port |    Port    | Physical port close to eNB cell |  Yes |                                  |
| enb\_core\_port |    Port    | Physical port close to eNB core |  Yes |                                  |
| gnb\_cell\_port |    Port    | Physical port close to gNB cell |  Yes |                                  |
| gnb\_core\_port |    Port    | Physical port close to gNB core |  Yes |                                  |
| enb\_cell\_addr | IP address |       eNB cell IP address       |  Yes | Both IPv4 and IPv6. Accept CIDR. |
| gnb\_cell\_addr | IP address |       gNB cell IP address       |  Yes | Both IPv4 and IPv6. Accept CIDR. |

**`<enb_cell_addr>` and `<gnb_cell_addr>` must be has the same IP address vresion.**

**The [`<next>`](Element/run/regular/chain.md#next) tags macros expanded is just a way of expression. It's still affect by [`match mode`](Element/run/filter/find.md#match_mode) if apply directly.**

<h2>Topology</h2>

```
   +---------+         +---------+
   | 4G Cell |         | 5G Cell |
   +----+----+         +----+----+
        | P49               | P51
    +---+-------------------+---+
    |                           |
    |                           |
    |                           |
    |                           |
    |                           |
    |          GRISM-A          |
    |                           |
    |                           |
    |                           |
    |                           |
    |                           |
    +---+-------------------+---+
        | P50               | P52
   +----+----+         +----+----+
   | 4G Core |         | 5G Core |
   +---------+         +---------+
```

<h2>Example</h2>

```
<run>

<macros>

<!-- EN-DC -->
<en_dc>
    <enb_cell_port>P49</enb_cell_port>
    <enb_core_port>P50</enb_core_port>
    <gnb_cell_port>P51</gnb_cell_port>
    <gnb_core_port>P52</gnb_core_port>
    <enb_cell_addr>192.168.200.169</enb_cell_addr>
    <gnb_cell_addr>192.168.216.65</gnb_cell_addr>
</en_dc>

</macros>

</run>
```

<h2>Expansion</h2>

```
<run>

<filter id="1">
<and>
    <find name="ip.src" relation="==" content="192.168.200.169"/>
</and>
</filter>

<filter id="2">
<and>
    <find name="ip.dst" relation="==" content="192.168.216.65"/>
</and>
</filter>

<filter id="3">
<and>
    <find name="ip.src" relation="==" content="192.168.216.65"/>
</and>
</filter>

<filter id="4">
<and>
    <find name="ip.dst" relation="==" content="192.168.200.169"/>
</and>
</filter>

<regular>

<chain>
    <in>P49</in>
    <fid>F1</fid>
    <next>
        <fid>F2</fid>
        <out>P50</out>
    </next>
</chain>

<chain>
    <in>P50</in>
    <fid>F3</fid>
    <next>
        <fid>F4</fid>
        <out>P49</out>
    </next>
</chain>

<chain>
    <in>P51</in>
    <fid>F3</fid>
    <next>
        <fid>F4</fid>
        <out>P52</out>
    </next>
</chain>

<chain>
    <in>P52</in>
    <fid>F1</fid>
    <next>
        <fid>F2</fid>
        <out>P51</out>
    </next>
</chain>

</regular>

</run>
```
