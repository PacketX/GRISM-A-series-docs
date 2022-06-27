GRISM (A-series)
================

With `GRISM` XML, you can describes the structure of Network Packet Broker(NPB).

<h2>Features</h2>

* Easy to learn and use, simple to edit.
* User-Friendly.
* Easy to integrate with third-party software.
* Simple and lightweight.
* RestAPI.

<h2>A Simple GRISM XML</h2>

This is an example for filter out `udp port 53`.

```
<run>

<filter id="1">
<or>
    <find name="udp.port" relation="==" content="53"/>
</or>
</filter>

<regular>

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
