Chain
============

`<chain>` tag described the actual packet flow chart (sort of).

<h2>Child Elements</h2>

| Eelment |  Content  |               Description              |                  Note                  |
|:-------:|:---------:|:--------------------------------------:|:--------------------------------------:|
|    in   |   Ports   |        Physical ports for input        |  Can be multiple inputs separated by , |
|   out   |   Ports   | Physical ports or output ID for output | Can be multiple outputs separated by , |
|   fid   | Filter ID |            Filter unique ID            |                                        |
|   next  | See below |         Continue chain content         |                                        |

<h3>&lt;in&gt; Tag</h3>

`<in>` tag described what physical port used as input. Port number start with prefix `P`, and port number can be formatted as range.

<h4>&lt;in&gt; Tag Example</h4>

```
<in>P1, P5, P10-15</in>
```

<h3>&lt;out&gt; Tag</h3>

`<out>` tag described what physical port or [`<output>`](output.md) tag used as output. Port number start with prefix `P`, and port number can be formatted as range. `0` and `drop` means drop packets.

| Attribute |  Description  |                 Type                |
|:---------:|:-------------:|:-----------------------------------:|
|    type   | How to output | 'duplicate' or 'loadBalance' string |

**If attribute `type` is not specified, `duplicate` is chosen.**

<h4>&lt;out&gt; Limits</h4>

If attribute `type` is `loadBalance`, there is couple limits:

1. Cannot use [`<output>`](output.md) tag as an output, which mean: O1, O2 is an invalid output port in this situation.

<h4>&lt;out&gt; Tag Example</h4>

```
<out>P1, P5, P10-15, O1, O3, O5-7</out>
```

<h3>&lt;fid&gt; Tag</h3>

`<fid>` tag described what [`<filter>`](filter.md) to use, one filter id only.

<h4>&lt;fid&gt; Tag Example</h4>

```
<fid>F1</fid>
```

<h3>&lt;next&gt; Tag</h3>

`<next>` tag described when if packet match or not match a filter actions that the continue tags.

| Attribute |           Description          |             Type             |
|:---------:|:------------------------------:|:----------------------------:|
|    type   | What condition of \<next\> tag | 'match' or 'notmatch' string |

<h4>&lt;next&gt; Tag Example</h4>

```
<in>P1</in>
<fid>F1</fid>
<next>
    <fid>F2</fid>
    <out>P2</out>
</next>
<next type="notmatch">
    <fid>F3</fid>
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
    <fid>F1</fid>
    <out>P2</out>
</chain>

</regular>

</run>
```

<h2>Example 2</h2>

<h3>Chart</h3>

```
P1 -> F1 ↴
         -> F2 -> P2
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

<filter id="2">
<or>

...

</or>
</filter>

<chain>
    <in>P1</in>
    <fid>F1</fid>
    <next>
        <fid>F2</fid>
        <out>P2</out>
    </next>
    <next type="notmatch">
        <out>P3</out>
    </next>
</chain>

</regular>

</run>
```

<h2>Example 3</h2>

<h3>Chart</h3>

```
P1 -> F1 ↴
         -> F2 -> P2
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
    <fid>F1</fid>
    <next>
        <fid>F2</fid>
        <out>P2</out>
        <next type="notmatch">
            <out>O1</out>
        </next>
    </next>
</chain>

</regular>

</run>
```
