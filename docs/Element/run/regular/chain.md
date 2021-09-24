Chain
============

`<chain>` tag described the actual packet flow chart (sort of).

<h2>Child Elements</h2>

| Eelment |  Content  |          Description         | Must | Note |
|:-------:|:---------:|:----------------------------:|:----:|:----:|
|    in   |   Ports   |   Physical ports for input   |  Yes |      |
|   next  | See below | Packet match priority tables |  Yes |      |

<h3>&lt;in&gt; Tag</h3>

`<in>` tag described what physical ports used as input. Port number start with prefix `P` and port number can be formatted as range. Multiple input ports separated by `, `. Breakout port has suffix `_1` to `_4`, `_all` as all sub-ports.

<h4>&lt;in&gt; Tag Example</h4>

```
<in>P1, P5, P10-15, P20_1, P20_2, P24_all</in>
```

<h3 id="next">&lt;next&gt; Tag</h3>

`<next>` tag described what the packet match priority tables. The depth it is, the priority lower it gets.

<h4>Child Elements</h4>

| Eelment |  Content  |               Description              | Must | Note |
|:-------:|:---------:|:--------------------------------------:|:----:|:----:|
|   fid   | Filter ID |                Filter ID               |  No  |      |
|   out   |   Ports   | Physical ports or output ID for output |  Yes |      |

<h4 id="next_limits">&lt;next&gt; Limits</h4>

If any filter is in `match mode`, there is only one `<next>` tag can be used, whatever the filter is used or not. See [`match mode`](Element/run/filter/find.md#match_mode).

<h5>&lt;fid&gt; Tag</h5>

`<fid>` tag described what [`<filter>`](Element/run/filter.md) to use, multiple filter id is acceptable.

| Attribute |      Description      |         Type         | Must |
|:---------:|:---------------------:|:--------------------:|:----:|
|    type   | How to combine filter | 'or' or 'and' string |  No  |

**If attribute `type` is not specified, `or` is chosen.**

<h5>&lt;fid&gt; Tag Example</h5>

```
<fid type="and">F1, F2</fid>
```

<h5>&lt;out&gt; Tag</h5>

`<out>` tag described what physical ports or [`<output>`](Element/run/output.md) tag used as output. Port number start with prefix `P` and port number can be formatted as range. `0` and `drop` means drop packets. Multiple output ports separated by `, `. Breakout port has suffix `_1` to `_4`, `_all` as all sub-ports.

| Attribute |  Description  |                         Type                        | Must |
|:---------:|:-------------:|:---------------------------------------------------:|:----:|
|    type   | How to output | 'duplicate', 'loadBalance' or 'fastFailover' string |  No  |

`duplicate` will flood packets to all output ports. `loadBalance` will do load blance by 5-tuple to selected ports. `fastFailover` will send packet to first output port, if link down then try next output port.

**If attribute `type` is not specified, `duplicate` is chosen.**

<h5>&lt;out&gt; Limits</h5>

If attribute `type` is `loadBalance`, there is couple limits:

1. Cannot use [`<output>`](Element/run/output.md) tag as an output, which mean: O1, O2 is an invalid output port in this situation.
2. The maximum number of member ports is `8`.
3. The maximum number of `<out type="loadBalance">` tags is `48`.
4. Physical output port cannot be duplicated. Take an example:

```
<run>

<output id="1">
    <port>P2</port>
    <QinQ>100</QinQ>
</output>

<output id="2">
    <port>P2</port>
    <QinQ>101</QinQ>
</output>

<regular>
<chain>
    <in>P1</in>
    <next>
        <out>P2, O1</out>
    </next>
</chain>

<chain>
    <in>P1</in>
    <next>
        <out>O1, O2</out>
    </next>
</chain>
</regular>

</run>
```

All of above chains are invalid. `P2, O1` physical output port are `P2`, and `O1, O2` are `P2` too.

<h5>&lt;out&gt; Tag Example</h5>

```
<out>P1, P5, P10-15, P20_1, P20_2, P24_all, O1, O3, O5-7</out>
```

<h4>&lt;next&gt; Tag Example</h4>

```
<in>P1</in>
<next>
    <fid>F1</fid>
    <out>P2</out>
</next>
<next>
    <fid>F2</fid>
    <out>P3</out>
</next>
```

<h2>Example 1</h2>

<h3>Chart</h3>

```
P1 -> F1 -> P2
```

<h3>Task</h3>

```
<run>

<regular>

<filter id="1">
<or>

...

</or>
</filter>

<chain>
    <in>P1</in>
    <next>
        <fid>F1</fid>
        <out>P2</out>
    </next>
</chain>

</regular>

</run>
```

<h2>Example 2</h2>

<h3>Chart</h3>

```
P1 -> F1 ↴
         -> P2
         !> P3
```

<h3>Task</h3>

```
<run>

<regular>

<filter id="1">
<or>

...

</or>
</filter>

<chain>
    <in>P1</in>
    <next>
        <fid>F1</fid>
        <out>P2</out>
    </next>
    <next>
        <out>P3</out>
    </next>
</chain>

</regular>

</run>
```

<h2>Example 3</h2>

<h3>Chart</h3>

```
P1 -> F1 -> P2
    ↳ F2 -> P3
   !> O1
```

<h3>Task</h3>

```
<run>

<regular>

<filter id="1">
<or>

...

</or>
</filter>

<filter id="2">
<or>

...

</or>
</filter>

<output id="1">

...

</output>

<chain>
    <in>P1</in>
    <next>
        <fid>F1</fid>
        <out>P2</out>
    </next>
    <next>
        <fid>F2</fid>
        <out>P3</out>
    </next>
    <next>
        <out>O1</out>
    </next>
</chain>

</regular>

</run>
```
