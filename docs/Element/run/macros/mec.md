MEC
==========

For `MEC`(`Mobile Edge Computing`) solution.

<h2>Child Elements</h2>

|       Eelment      |  Content  |                    Description                   | Must | Note |
|:------------------:|:---------:|:------------------------------------------------:|:----:|:----:|
|      cell_port     |    Port   |            Physical port close to cell           |  Yes |      |
|      core_port     |    Port   |            Physical port close to core           |  Yes |      |
|      lbo_ports     |    Port   |         Physical ports for local breakout        |  Yes |      |
|   not\_lbo\_ports  |    Port   |       Physical ports for non-local breakout      |  Yes |      |
|     s1ap_ports     |   Ports   | Physical ports for S1AP packets mirror or inline |  No  |      |
| s1ap\_extra\_ports |   Ports   |   Extra physical ports for S1AP packets mirror   |  No  |      |
|  cell\_filter\_id  | Filter ID |       Filter ID to determine cell datapath       |  No  |      |

**The [`<next>`](Element/run/regular/chain.md#next) tags macros expanded is just a way of expression. It's still affect by [`match mode`](Element/run/filter/find.md#match_mode) if apply directly.**

<h3>&lt;s1ap_ports&gt; Tag</h3>

**If `<s1ap_ports>` tag is not set, it will be act as 5G environment.**

`<s1ap_ports>` tag described what physical ports used for S1AP packets mirror or inline. Port number start with prefix `P` and port number can be formatted as range. Multiple output ports separated by `, `.

| Attribute |             Description            |                Type               | Must |
|:---------:|:----------------------------------:|:---------------------------------:|:----:|
|   active  | Behaving mirror, inline or nothing | 'none', 'mirror', 'inline' string |  No  |

<h4>&lt;active&gt; Defaults Priority</h4>

1. If `<s1ap_ports>` is empty and `active` has no attribute: `none`.
2. If `<s1ap_ports>` is not empty and `active` has no attribute: `mirror`.
3. Last: invalid.

<h3>&lt;s1ap_extra_ports&gt; Tag</h3>

**If `<s1ap_ports>` tag is not set, the tag won't do anything.**

`<s1ap_extra_ports>` tag described what extra physical ports used for S1AP packets mirror. Port number start with prefix `P` and port number can be formatted as range. Multiple output ports separated by `, `.

<h2>Topology</h2>

```
   +---------+         +---------+
   | 4G Cell |         | 5G Cell |
   +----+----+         +----+----+
        | P25               | P27
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
        | P26               | P28
   +----+----+         +----+----+
   | 4G Core |         | 5G Core |
   +---------+         +---------+
```

<h2>Example</h2>

```
<run>

<macros>

<!-- 4G -->
<mec>
    <cell_port>P25</cell_port>
    <core_port>P26</core_port>
    <lbo_port>P1</lbo_port>
    <not_lbo_port>P2</not_lbo_port>
    <s1ap_port "active"="mirror">P3</s1ap_port>
    <cell_filter_id>F1</cell_filter_id>
</mec>

<!-- 5G -->
<mec>
    <cell_port>P27</cell_port>
    <core_port>P28</core_port>
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

<!-- 4G: P25, P26 -->
<!-- 5G: P27, P28 -->
<!-- LBO: P1 -->
<!-- no-LBO: P2 -->
<!-- s1ap: P3 -->

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
    <in>P25</in>
    <fid>F3</fid>
    <out>P1</out>
    <next type="notmatch">
        <fid>F4</fid>
        <out>P3, P26</out>
        <next type="notmatch">
            <out>P26</out>
        </next>
    </next>
</chain>

<chain>
    <in>P26</in>
    <fid>F4</fid>
    <out>P3, P25</out>
    <next type="notmatch">
        <out>P25</out>
    </next>
</chain>

<!-- 5G -->
<chain>
    <in>P27</in>
    <fid>F3</fid>
    <out>P1</out>
    <next type="notmatch">
        <out>P28</out>
    </next>
</chain>

<chain>
    <in>P28</in>
    <out>P27</out>
</chain>

<!-- LBO -->
<chain>
    <in>P1</in>
    <next>
        <fid>F1</fid>
        <out>P25</out>
    </next>
    <next>
        <fid>F2</fid>
        <out>P27</out>
    </next>
</chain>

<!-- no-LBO -->
<chain>
    <in>P2</in>
    <next>
        <fid>F1</fid>
        <out>P26</out>
    </next>
    <next>
        <fid>F2</fid>
        <out>P28</out>
    </next>
</chain>

</regular>

</run>
```
