Output
============

`<output>` tag described what actions to do before outputting to a physical port.

<h2>Attribute</h2>

| Attribute |       Description       |   Type  | Must |
|:---------:|:-----------------------:|:-------:|:----:|
|     id    | Unique ID in whole task | Integer |  Yes |

<h2>Child Elements</h2>

|  Eelment  |     Content    |         Description         | Must |           Note          |
|:---------:|:--------------:|:---------------------------:|:----:|:-----------------------:|
|    port   |      Ports     |  Physical ports for output  |  Yes |                         |
| stripping |    See note    |        strip a header       |  No  | Accept keyword: `vlan`. |
|     Q     | 0-4095 integer | Add or modify a VLAN header |  No  |                         |
|    QinQ   | 0-4095 integer |      Push a VLAN header     |  No  |                         |

<h3>&lt;port&gt; Tag</h3>

`<port>` tag described what physical port used as output. Port number start with prefix `P` and port number can be formatted as range. Multiple output port separated by ` ,`.

<h4>&lt;port&gt; Tag Example</h4>

```
<in>P1, P5, P10-15</in>
```

<h3>&lt;Q&gt; Tag Attribute</h3>

| Attribute |  Description  |     Type    | Must |
|:---------:|:-------------:|:-----------:|:----:|
|  priority | VLAN priority | 0-7 integer |  Yes |

**`<Q>` tag and `<QinQ>` tag cannot be used together.**

<h3>&lt;QinQ&gt; Tag Attribute</h3>

| Attribute |  Description  |     Type    | Must |
|:---------:|:-------------:|:-----------:|:----:|
|  priority | VLAN priority | 0-7 integer |  Yes |

**`<Q>` tag and `<QinQ>` tag cannot be used together.**

<h4>&lt;QinQ&gt; Limits</h4>

1. `<QinQ>` tag only can be used twice in one `<output>` tag.
2. Able to identify two tags only.
3. If a packet already tagged with one vlan (tag0) and do push vlan twice (tagA, tagB), output packet is tagged with two vlans (tag0, tagB).
4. If a packet tagged with more than one vlan (outter is tag0) and do push vlan twice (tagA, tagB), output packet is tagged with vlans (tag0 and other tag of original packet, tagB).

<h2>Example 1</h2>

```
<run>

<output id="1">
    <port>P1</port>
    <stripping>vlan</stripping>
</output>

</run>
```

<h2>Example 2</h2>

```
<run>

<output id="2">
    <port>P2</port>
    <Q>10</Q>
</output>

</run>
```

<h2>Example 3</h2>

```
<run>

<output id="3">
    <port>P3</port>
    <QinQ priority="2">15</QinQ>
    <QinQ priority="3">20</QinQ>
</output>

</run>
```

<h2>Usage</h2>

`<output id="1">` will be compiled as `O1`, Using by `<out>O1</out>` form.
