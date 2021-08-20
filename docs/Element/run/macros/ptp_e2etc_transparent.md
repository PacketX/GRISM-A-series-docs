PTP E2ETC Transparent
=================

Enable the `PTP` `E2ETC` function on interfaces and  forward the `PTP` packets directly.

<h2>Child Element</h2>

| Eelment | Content |                Description                | Must | Note |
|:-------:|:-------:|:-----------------------------------------:|:----:|:----:|
|  ports  |  Ports  | Physical ports to transparent PTP packets |  Yes |      |

**Once configured ports, it needs to add chains(or other datapath) to forward packets.**

<h3>&lt;port&gt; Tag</h3>

`<port>` tag described what physical ports to transparent PTP packets. Port number start with prefix `P` and port number can be formatted as range. Multiple ports separated by `, `. Breakout port has suffix `_1` to `_4`, `_all` as all sub-ports.

<h2>Example</h2>

```
<run>

<macros>

<ptp_e2etc_transparent>
    <ports>P1, P2</ports>
</ptp_e2etc_transparent>

</macros>

<regular>

<chain>
    <in>P1</in>
    <out>P2</out>
</chain>

<chain>
    <in>P2</in>
    <out>P1</out>
</chain>

</regular>

</run>
```

<h2>Expansion</h2>

It won't be expand as any XML tag form.
