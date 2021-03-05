MEC
==========

For `MEC`(`Mobile Edge Computing`) solution.

<h2>Child Elements</h2>

|      Eelment     |  Content  |              Description              | Must | Note |
|:----------------:|:---------:|:-------------------------------------:|:----:|:----:|
|     cell_port    |    Port   |      Physical port close to cell      |  Yes |      |
|     core_port    |    Port   |      Physical port close to core      |  Yes |      |
|     lbo_port     |    Port   |    Physical port for local breakout   |  Yes |      |
|  not\_lbo\_port  |    Port   |  Physical port for non-local breakout |  Yes |      |
|     s1ap_port    |    Port   | Physical port for S1AP packets mirror |  No  |      |
| cell\_filter\_id | Filter ID |  Filter ID to determine cell datapath |  No  |      |

**If `<s1ap_port>` tag is not set, it will be act as 5G environment.**

**The [`<next>`](Element/run/regular/chain.md#next) tags macros expanded is just a way of expression. It's still affect by [`match mode`](Element/run/filter/find.md#match_mode) if apply directly.**

<h2>Topology</h2>

```
   +---------+         +---------+
   | 4G Cell |         | 5G Cell |
   +----+----+         +----+----+
        | P49               | P51
    +---+-------------------+---+            +-------------+
    |                           |   LBO P1   |             |
    |                           +------------+             |
    |                           |            |             |
    |                           | not-LBO P2 |             |
    |                           +------------+   GRISM-T   |
    |          GRISM-A          |            |             |
    |                           |   S1AP P3  |             |
    |                           +------------+             |
    |                           |            |             |
    |                           |            +-------------+
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

<ptp_e2etc_transparent>
    <port>P49-52</port>
</ptp_e2etc_transparent>

<!-- 4G -->
<mec>
    <cell_port>P49</cell_port>
    <core_port>P50</core_port>
    <lbo_port>P1</lbo_port>
    <not_lbo_port>P2</not_lbo_port>
    <s1ap_port>P3</s1ap_port>
    <cell_filter_id>F1</cell_filter_id>
</mec>

<!-- 5G -->
<mec>
    <cell_port>P51</cell_port>
    <core_port>P52</core_port>
    <lbo_port>P1</lbo_port>
    <not_lbo_port>P2</not_lbo_port>
    <cell_filter_id>F2</cell_filter_id>
</mec>

</macros>

<filter id="1">
<or>
    <find name="ipv6.addr" relation="==" content="fe08::1"/>
</or>
</filter>

<filter id="2">
<or>
    <find name="ip.addr" relation="==" content="192.168.2.1"/>
</or>
</filter>

</run>
```

<h2>Expansion</h2>

```
<run>

<!-- 4G: P49, P50 -->
<!-- 5G: P51, P52 -->
<!-- LBO: P1 -->
<!-- no-LBO: P2 -->
<!-- s1ap: P3 -->

<macros>

<ptp_e2etc_transparent>
    <port>P49-52</port>
</ptp_e2etc_transparent>

</macros>

<filter id="1">
<or>
    <find name="ipv6.addr" relation="==" content="fe08::1"/>
</or>
</filter>

<filter id="2">
<or>
    <find name="ip.addr" relation="==" content="192.168.2.1"/>
</or>
</filter>

<filter id="3">
<or>
    <find name="gtpv1_data" relation="==" content=""/>
</or>
</filter>

<filter id="4">
<or>
    <find name="s1ap" relation="==" content=""/>
</or>
</filter>

<regular>

<!-- 4G -->
<chain>
    <in>P49</in>
    <fid>F3</fid>
    <out>P1</out>
    <next type="notmatch">
        <fid>F4</fid>
        <out>P3, P50</out>
        <next type="notmatch">
            <out>P50</out>
        </next>
    </next>
</chain>

<chain>
    <in>P50</in>
    <fid>F4</fid>
    <out>P3, P49</out>
    <next type="notmatch">
        <out>P49</out>
    </next>
</chain>

<!-- 5G -->
<chain>
    <in>P51</in>
    <fid>F3</fid>
    <out>P1</out>
    <next type="notmatch">
        <out>P52</out>
    </next>
</chain>

<chain>
    <in>P52</in>
    <out>P51</out>
</chain>

<!-- LBO -->
<chain>
    <in>P1</in>
    <next>
        <fid>F1</fid>
        <out>P49</out>
    </next>
    <next>
        <fid>F2</fid>
        <out>P51</out>
    </next>
</chain>

<!-- no-LBO -->
<chain>
    <in>P2</in>
    <next>
        <fid>F1</fid>
        <out>P50</out>
    </next>
    <next>
        <fid>F2</fid>
        <out>P52</out>
    </next>
</chain>

</regular>

</run>
```
