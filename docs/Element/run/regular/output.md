Output
============

`<output>` tag described what actions to do before outputting to a physical port.

<h2>Attribute</h2>

| Attribute |       Description       |   Type  |
|:---------:|:-----------------------:|:-------:|
|     id    | Unique ID in whole task | Integer |

<h2>Child Elements</h2>

|  Eelment  |     Content    |         Description         |                          Note                         |
|:---------:|:--------------:|:---------------------------:|:-----------------------------------------------------:|
|    port   |     A port     |  Physical ports for output  |                                                       |
| stripping |    See note    |        strip a header       |                  Accept keyword: vlan                 |
|     Q     | 0-4095 integer | Add or modify a VLAN header | '\<Q\>' tag and '\<QinQ\>' tag cannot be use together |
|    QinQ   | 0-4095 integer |      Push a VLAN header     | '\<Q\>' tag and '\<QinQ\>' tag cannot be use together |

<h3>&lt;Q&gt; Tag Attribute</h3>

| Attribute |  Description  |     Type    |
|:---------:|:-------------:|:-----------:|
|  priority | VLAN priority | 0-7 integer |

<h3>&lt;QinQ&gt; Tag Attribute</h3>

| Attribute |  Description  |     Type    |
|:---------:|:-------------:|:-----------:|
|  priority | VLAN priority | 0-7 integer |

<h4>&lt;QinQ&gt; Limits</h4>

1. `<QinQ>` tag only can be used twice in one `<output>` tag.
2. Able to identify two tags only.
3. If a packet already tagged with one vlan (tag0) and do push vlan twice (tagA, tagB), output packet is tagged with two vlans (tag0, tagB).
4. If a packet tagged with more than one vlan (outter is tag0) and do push vlan twice (tagA, tagB), output packet is tagged with vlans (tag0 and other tag of original packet, tagB).

<h2>Example 1</h2>

```
<run>

<regular>

<output id="1">
    <port>P1</port>
    <stripping>vlan</stripping>
</output>

</regular>

</run>
```

<h2>Example 2</h2>

```
<run>

<regular>

<output id="2">
    <port>P2</port>
    <Q>10</Q>
</output>

</regular>

</run>
```

<h2>Example 3</h2>

```
<run>

<regular>

<output id="3">
    <port>P3</port>
    <QinQ priority="2">15</QinQ>
    <QinQ priority="3">20</QinQ>
</output>

</regular>

</run>
```

<h2>Usage</h2>

`<output id="1">` will be compiled as `O1`, used by `<out>O1</out>` tag within [`<chain>`](Element/run/regular/chain.md) tag.
