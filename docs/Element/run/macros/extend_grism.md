Extend GRISM
============

Behave as a `GRISM` extended physical ports.

<h2>Child Elements</h2>

|     Eelment     |     Content    |          Description          | Must |                                        Note                                       |
|:---------------:|:--------------:|:-----------------------------:|:----:|:---------------------------------------------------------------------------------:|
|        in       |      Ports     |    Physical ports for input   |  Yes |                                                                                   |
|       out       |      Ports     | Physical ports for for output |  Yes |                                                                                   |
| vlan\_id\_start | 0-4095 integer |   The VLAN ID start tagging   |  Yes | If start ID plus maximum output port number is greater than 4095 will be invalid. |

<h3>&lt;in&gt; Tag</h3>

`<in>` tag described what physical ports used as input. Port number start with prefix `P` and port number can be formatted as range. Multiple input ports separated by `, `. Breakout port has suffix `_1` to `_4`, `_all` as all sub-ports.

<h3>&lt;out&gt; Tag</h3>

`<out>` tag described what physical ports used as output. Port number start with prefix `P` and port number can be formatted as range. Multiple output ports separated by `, `. Breakout port has suffix `_1` to `_4`, `_all` as all sub-ports.

<h2>Topology</h2>

```
        +     +
     P1 |     | P2
    +---+-----+---+         +-------------+
    |             |         |             |
    |             |   P10   |             |
    |   GRISM-A   +---------+   GRISM-T   |
    |             |         |             |
    |             |         |             |
    +---+-----+---+         +-------------+
     P3 |     | P7
        +     +
```

<h2>Example</h2>

```
<run>

<macros>
    <extend_grism>
        <in>P1-3, P7</in>
        <out>P10</out>
        <vlan_id_start>101</vlan_id_start>
    </extend_grism>
</macros>

</run>
```

<h2>Expansion</h2>

```
<run>

<output id="1">
    <port>P10</port>
    <Q>101</Q>
</output>

<output id="2">
    <port>P10</port>
    <Q>102</Q>
</output>

<output id="3">
    <port>P10</port>
    <Q>103</Q>
</output>

<output id="4">
    <port>P10</port>
    <Q>107</Q>
</output>

<output id="5">
    <port>P1</port>
    <stripping>vlan</stripping>
</output>

<output id="6">
    <port>P2</port>
    <stripping>vlan</stripping>
</output>

<output id="7">
    <port>P3</port>
    <stripping>vlan</stripping>
</output>

<output id="8">
    <port>P7</port>
    <stripping>vlan</stripping>
</output>

<filter id="1">
<or>
    <find name="vlan.id" relation="==" content="101" />
</or>
</filter>

<filter id="2">
<or>
    <find name="vlan.id" relation="==" content="102" />
</or>
</filter>

<filter id="3">
<or>
    <find name="vlan.id" relation="==" content="103" />
</or>
</filter>

<filter id="4">
<or>
    <find name="vlan.id" relation="==" content="107" />
</or>
</filter>

<regular>

<chain>
    <in>P1</in>
    <out>O1</out>
</chain>

<chain>
    <in>P2</in>
    <out>O2</out>
</chain>

<chain>
    <in>P3</in>
    <out>O3</out>
</chain>

<chain>
    <in>P7</in>
    <out>O4</out>
</chain>

<chain>
    <in>P10</in>
    <next>
        <fid>F1</fid>
        <out>O5</out>
    </next>
    <next>
        <fid>F2</fid>
        <out>O6</out>
    </next>
    <next>
        <fid>F3</fid>
        <out>O7</out>
    </next>
    <next>
        <fid>F4</fid>
        <out>O8</out>
    </next>
</chain>

</regular>

</run>
```
