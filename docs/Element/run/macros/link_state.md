Link State
=================

Set port link state.

<h2>Child Element</h2>

|    Eelment    | Content |              Description              | Must | Note |
|:-------------:|:-------:|:-------------------------------------:|:----:|:----:|
| disable_ports |  Ports  | Physical ports that permanent disable |  Yes |      |

<h3>&lt;ports&gt; Tag</h3>

`<disable_ports>` tag described what physical ports to permanent disable. Port number start with prefix `P` and port number can be formatted as range. Multiple ports separated by `, `. Breakout port has suffix `_1` to `_4`, `_all` as all sub-ports.

<h2>Example</h2>

```
<run>

<macros>

<link_state>
    <disable_ports>P1, P2</disable_ports>
</link_state>

</macros>

</run>
```

<h2>Expansion</h2>

It won't be expand as any XML tag form.
