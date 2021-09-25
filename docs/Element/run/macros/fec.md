FEC
=================

Enable FEC(forward error correction).

<h2>Child Element</h2>

| Eelment | Content |         Description         | Must | Note |
|:-------:|:-------:|:---------------------------:|:----:|:----:|
|  ports  |  Ports  | Physical ports that use FEC |  Yes |      |

Only works on port with speed is:

1. 100 Gbps
2. 40 Gbps
3. 25 Gbps

and auto negotiation.

<h3>&lt;ports&gt; Tag</h3>

`<ports>` tag described what physical ports to permanent disable. Port number start with prefix `P` and port number can be formatted as range. Multiple ports separated by `, `. Breakout port has suffix `_1` to `_4`, `_all` as all sub-ports.

<h2>Example</h2>

```
<run>

<macros>

<fec>
    <ports>P1, P2</ports>
</fec>

</macros>

</run>
```

<h2>Expansion</h2>

It won't be expand as any XML tag form.
