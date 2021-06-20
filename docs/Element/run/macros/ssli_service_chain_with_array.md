SSLi Service Chain with Array
=============

For `Array SSLi` service chain solution.

<h2>Child Elements</h2>

|          Eelment          |  Content  |                    Description                   | Must | Note |
|:-------------------------:|:---------:|:------------------------------------------------:|:----:|:----:|
|         lan\_port         |    Port   |            Physical port close to LAN            |  Yes |      |
|         wan\_port         |    Port   |            Physical port close to WAN            |  Yes |      |
| service\_chain\_lan\_port |    Port   |     Physical port close to service chain LAN     |  Yes |      |
| service\_chain\_wan\_port |    Port   |     Physical port close to service chain WAN     |  Yes |      |
|     offload\_lan\_port    |    Port   |      Physical port close to decrypt side LAN     |  Yes |      |
|     offload\_wan\_port    |    Port   |      Physical port close to decrypt side WAN     |  Yes |      |
|     onload\_lan\_port     |    Port   |      Physical port close to encrypt side LAN     |  Yes |      |
|     onload\_wan\_port     |    Port   |      Physical port close to encrypt side WAN     |  Yes |      |
|    encrypt\_filter\_id    | Filter ID | Filter ID to determine those belong to encrypted |  Yes |      |
|    decrypt\_filter\_id    | Filter ID | Filter ID to determine those belong to decrypted |  Yes |      |
|       dmz\_lan\_port      |    Port   |          Physical port close to DMZ LAN          |  No  |      |
|       dmz\_wan\_port      |    Port   |          Physical port close to DMZ WAN          |  No  |      |
|      dmz\_filter\_id      | Filter ID |    Filter ID to determine those belong to DMZ    |  No  |      |

**If `<dmz_lan_port>`, `<dmz_wan_port>` and `<dmz_filter_id>` tags are set, it will be act as with DMZ.**

**`<find>` tags set of `<encrypt_filter_id>`, `<decrypt_filter_id>` and `<dmz_filter_id>` must be mutually exclusive.**

**The [`<next>`](Element/run/regular/chain.md#next) tags macros expanded is just a way of expression. It's still affect by [`match mode`](Element/run/filter/find.md#match_mode) if apply directly.**

<h2>Topology1(with DMZ)</h2>

```
   +---------+               +-----+
   | DMZ WAN |               | WAN |
   +----+----+               +--+--+
        | P2                    | P10
    +---+-----------------------+---+            Array SSLi
    |                               |  WAN P6  +-------------+
    |                               +----------+             |
    |                               +----------+             |
    |                               |  LAN P8  |             |
    |                               |          |             |
    |                               |  WAN P7  |             |
    |                               +----------+             |
    |                               +----------+             |
    |            GRISM-A            |  LAN P5  +-------------+
    |                               |
    |                               |
    |                               |
    |                               |           Service Chain
    |                               |  WAN P4  +-------------+
    |                               +----------+             |
    |                               +----------+             |
    |                               |  LAN P3  +-------------+
    +---+-----------------------+---+
        | P1                    | P9
   +----+----+               +--+--+
   | DMZ LAN |               | LAN |
   +---------+               +-----+
```

<h2>Example1(with DMZ)</h2>

```
<run>

<macros>
    <ssli_service_chain_with_array>
        <lan_port>P9</lan_port>
        <wan_port>P10</wan_port>
        <service_chain_lan_port>P3</service_chain_lan_port>
        <service_chain_wan_port>P4</service_chain_wan_port>
        <offload_lan_port>P5</offload_lan_port>
        <offload_wan_port>P7</offload_wan_port>
        <onload_lan_port>P8</onload_lan_port>
        <onload_wan_port>P6</onload_wan_port>
        <encrypt_filter_id>F1</encrypt_filter_id>
        <decrypt_filter_id>F2</decrypt_filter_id>
        <dmz_lan_port>P1</dmz_lan_port>
        <dmz_wan_port>P2</dmz_wan_port>
        <dmz_filter_id>F3</dmz_filter_id>
    </ssli_service_chain_with_array>
</macros>

<filter id="1">
<or>
    <find name="tcp.port" relation="==" content="443"/>
</or>
</filter>

<filter id="2">
<or>
    <find name="tcp.port" relation="==" content="20443"/>
</or>
</filter>

<filter id="3">
<or>
    <find name="ip.addr" relation="==" content="10.110.0.0/16"/>
</or>
</filter>

</run>
```

<h2>Expansion1(with DMZ)</h2>

```
<run>

<filter id="1">
<or>
    <find name="https" relation="==" content=""/>
    <find name="udp.port" relation="==" content="53"/>
    <find name="arp" relation="==" content=""/>
</or>
</filter>

<filter id="2">
<or>
    <find name="https" relation="==" content=""/>
    <find name="tcp.port" relation="==" content="20443"/>
    <find name="udp.port" relation="==" content="53"/>
    <find name="arp" relation="==" content=""/>
</or>
</filter>

<filter id="3">
<or>
    <find name="arp" relation="==" content=""/>
</or>
</filter>

<filter id="4">
<or>
    <find name="ip.addr" relation="==" content="10.110.0.0/16"/>
</or>
</filter>

<regular>

<!-- LAN to Array SSLi or service chain LAN -->
<chain>
    <in>P9</in>
    <fid>F3</fid>
    <out>P5, P3</out>
    <next type="notmatch">
        <fid>F1</fid>
        <out>P5</out>
        <next type="notmatch">
            <out>P3</out>
        </next>
    </next>
</chain>

<!-- WAN to Array SSLi or service chain WAN -->
<chain>
    <in>P10</in>
    <fid>F1</fid>
    <out>P6</out>
    <next type="notmatch">
        <out>P4</out>
    </next>
</chain>

<chain>
    <in>P5</in>
    <out>P9</out>
</chain>

<chain>
    <in>P6</in>
    <out>P10</out>
</chain>

<chain>
    <in>P7</in>
    <out>P3</out>
</chain>

<chain>
    <in>P8</in>
    <out>P4</out>
</chain>

<!-- DMZ LAN to service chain LAN -->
<chain>
    <in>P1</in>
    <out>P3</out>
</chain>

<!-- DMZ WAN to service chain WAN -->
<chain>
    <in>P2</in>
    <out>P4</out>
</chain>

<!-- Service chain LAN to Array SSLi or DMZ LAN or LAN -->
<chain>
    <in>P3</in>
    <fid>F3</fid>
    <out>P1, P7</out>
    <next type="notmatch">
        <fid>F2</fid>
        <out>P7</out>
        <next type="notmatch">
            <fid>F4</fid>
            <out>P1</out>
            <next type="notmatch">
                <out>P9</out>
            </next>
        </next>
    </next>
</chain>

<!-- Service chain WAN to Array SSLi or DMZ WAN or WAN -->
<chain>
    <in>P4</in>
    <fid>F3</fid>
    <out>P2, P8</out>
    <next type="notmatch">
        <fid>F2</fid>
        <out>P8</out>
        <next type="notmatch">
            <fid>F4</fid>
            <out>P2</out>
            <next type="notmatch">
                <out>P10</out>
            </next>
        </next>
    </next>
</chain>

</regular>

</run>
```

<h2>Topology2(without DMZ)</h2>

```
                 +-----+
                 | WAN |
                 +--+--+
                    | P10
    +---------------+---------------+            Array SSLi
    |                               |  WAN P6  +-------------+
    |                               +----------+             |
    |                               +----------+             |
    |                               |  LAN P8  |             |
    |                               |          |             |
    |                               |  WAN P7  |             |
    |                               +----------+             |
    |                               +----------+             |
    |            GRISM-A            |  LAN P5  +-------------+
    |                               |
    |                               |
    |                               |
    |                               |           Service Chain
    |                               |  WAN P4  +-------------+
    |                               +----------+             |
    |                               +----------+             |
    |                               |  LAN P3  +-------------+
    +---------------+---------------+
                    | P9
                 +--+--+
                 | LAN |
                 +-----+
```

<h2>Example2(without DMZ)</h2>

```
<run>

<macros>
    <ssli_service_chain_with_array>
        <lan_port>P9</lan_port>
        <wan_port>P10</wan_port>
        <service_chain_lan_port>P3</service_chain_lan_port>
        <service_chain_wan_port>P4</service_chain_wan_port>
        <offload_lan_port>P5</offload_lan_port>
        <offload_wan_port>P7</offload_wan_port>
        <onload_lan_port>P8</onload_lan_port>
        <onload_wan_port>P6</onload_wan_port>
        <encrypt_filter_id>F1</encrypt_filter_id>
        <decrypt_filter_id>F2</decrypt_filter_id>
    </ssli_service_chain_with_array>
</macros>

<filter id="1">
<or>
    <find name="tcp.port" relation="==" content="443"/>
</or>
</filter>

<filter id="2">
<or>
    <find name="tcp.port" relation="==" content="20443"/>
</or>
</filter>

</run>
```

<h2>Expansion2(without DMZ)</h2>

```
<run>

<filter id="1">
    <or>
        <find name="https" relation="==" content=""/>
        <find name="udp.port" relation="==" content="53"/>
        <find name="arp" relation="==" content=""/>
    </or>
</filter>

<filter id="2" sessionBase="no">
    <or>
        <find name="tcp.port" relation="==" content="20443"/>
        <find name="udp.port" relation="==" content="53"/>
        <find name="arp" relation="==" content=""/>
    </or>
</filter>

<filter id="3">
    <or>
        <find name="arp" relation="==" content=""/>
    </or>
</filter>

<regular>

<!-- LAN to Array SSLi or service chain LAN -->
<chain>
    <in>P9</in>
    <fid>F3</fid>
    <out>P5, P3</out>
    <next type="notmatch">
        <fid>F1</fid>
        <out>P5</out>
        <next type="notmatch">
            <out>P3</out>
        </next>
    </next>
</chain>

<!-- WAN to Array SSLi or service chain WAN -->
<chain>
    <in>P10</in>
    <fid>F1</fid>
    <out>P6</out>
    <next type="notmatch">
        <out>P4</out>
    </next>
</chain>

<chain>
    <in>P5</in>
    <out>P9</out>
</chain>

<chain>
    <in>P6</in>
    <out>P10</out>
</chain>

<chain>
    <in>P7</in>
    <out>P3</out>
</chain>

<chain>
    <in>P8</in>
    <out>P4</out>
</chain>

<!-- Service chain LAN to Array SSLi or LAN -->
<chain>
    <in>P3</in>
    <fid>F2</fid>
    <out>P7</out>
    <next type="notmatch">
        <out>P9</out>
    </next>
</chain>

<!-- Service chain WAN to Array SSLi or WAN -->
<chain>
    <in>P4</in>
    <fid>F2</fid>
    <out>P8</out>
    <next type="notmatch">
        <out>P10</out>
    </next>
</chain>

</regular>

</run>
```
