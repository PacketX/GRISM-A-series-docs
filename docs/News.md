News
=========

<h2>2022/03/30</h2>

* [`<macros>`](Element/run/macros.md): add `<ngap_ports>` tag and `<ngap_extra_ports>` tag in [`<mec>`](Element/run/macros/mec.md) tag.

<h2>2021/09/25</h2>

* [`<output>`](Element/run/output.md): update the limitation of `<QinQ>` tag.
* [`<macros>`](Element/run/macros.md): add [`<fec>`](Element/run/macros/fec.md) tag for [`FEC`](Element/run/macros/fec.md).

<h2>2021/09/24</h2>

* [`<chain>`](Element/run/regular/chain.md): `<out>` tag added one limit.

<h2>2021/09/05</h2>

* [`<out>`](Element/run/regular/chain.md): `type` attribute `fastFailover` aceept `<output>` tag type output.

<h2>2021/09/04</h2>

* [`<chain>`](Element/run/regular/chain.md): redefine `<next>` tag behavior.
* [`<out>`](Element/run/regular/chain.md): `type` attribute add `fastFailover`.

<h2>2021/09/01</h2>

* [`<filter>`](Element/run/filter.md): add `<and>` tag support.
* [`<fid>`](Element/run/regular/chain.md): add `type` attribute.
* [`<find>`](Element/run/filter/find.md): add `slow_protocol` and `vrrp`.

<h2>2021/08/21</h2>

* [`<macros>`](Element/run/macros.md): add [`<link_state>`](Element/run/macros/link_state.md) tag for [`Link State`](Element/run/macros/link_state.md).

<h2>2021/07/30</h2>

* Multiple ports argument accepts breakout port format.

<h2>2021/07/29</h2>

* [`<macros>`](Element/run/macros.md): change the default befavior of `<lbo_ports>` tag and `<not_lbo_port>` tag.

<h2>2021/07/28</h2>

* [`<macros>`](Element/run/macros.md): `<lbo_port>` tag rename to `<lbo_ports>` tag and support multiple output ports.
* [`<macros>`](Element/run/macros.md): `<not_lbo_port>` tag rename to `<not_lbo_ports>` tag and support multiple output ports.

<h2>2021/07/21</h2>

* [`<macros>`](Element/run/macros.md): `<s1ap_port>` tag rename to `<s1ap_ports>` tag and support multiple output ports.
* [`<macros>`](Element/run/macros.md): add `<s1ap_extra_ports>` tag.

<h2>2021/07/12</h2>

* [`<macros>`](Element/run/macros.md): `<s1ap_port>` tag add `active` attribute in [`<mec>`](Element/run/macros/mec.md) tag.

<h2>2021/06/20</h2>

* [`<macros>`](Element/run/macros.md): rename [`<ssli_service_chain>`](Element/run/macros/ssli_service_chain_with_array.md) tag to [`<ssli_service_chain_with_array>`](Element/run/macros/ssli_service_chain_with_array.md) tag.

<h2>2021/03/18</h2>

* [`<find>`](Element/run/filter/find.md): remove `mpls.label` and `mpls.bottom`.

<h2>2021/03/16</h2>

* [`<output>`](Element/run/output.md): add `<pop>` tag.

<h2>2021/03/06</h2>

* [`<macros>`](Element/run/macros.md): add [`<en_dc>`](Element/run/macros/en_dc.md) tag for [`E-UTRA-NR - Dual Connectivity`](Element/run/macros/en_dc.md).

<h2>2021/03/05</h2>

* [`Find combine`](Element/run/filter/find.md#combine): add `X2AP`.

<h2>2021/02/22</h2>

* [`<macros>`](Element/run/macros.md): add [`<mec>`](Element/run/macros/mec.md) tag for [`MEC`](Element/run/macros/mec.md).
* [`<macros>`](Element/run/macros.md): add [`<ssli_service_chain>`](Element/run/macros/ssli_service_chain.md) tag for [`SSLi service chain`](Element/run/macros/ssli_service_chain.md).

<h2>2021/02/21</h2>

* [`<macros>`](Element/run/macros.md): add [`<ptp_e2etc_transparent>`](Element/run/macros/ptp_e2etc_transparent.md) tag for [`PTP E2ETC Transparent`](Element/run/macros/ptp_e2etc_transparent.md).

<h2>2021/02/20</h2>

* [`Find name`](Element/run/filter/find.md#name): add `eth.bcast`, `eth.mcast`, `eth.mcastv6`, `vlan.dei`, `vlan.tci`, `mpls.label`, `mpls.tc`, `mpls.bottom`, `ip.mcast`, `ip.src_mcast`, `ip.fragment`, `ipv6.mcast_rsvd`, `ipv6.mcast_all_nodes`, `ipv6.mcast_all_rtrs`, `ipv6.mcast_sol_node`, `ipv6.mcast_flood`, `ipv6.mcast`, `mldv1`, `mldv2`, `ipv6.fragment`, `tcp.flags.[800]`, `tcp.flags.[400]`, `tcp.flags.[200]`, `tcp.flags.ns`, `tcp.flags.cwr`, `tcp.flags.ecn`, `tcp.flags.ece`, `tcp.flags.urg`, `tcp.flags.ack`, `tcp.flags.push`, `tcp.flags.psh`, `tcp.flags.reset`, `tcp.flags.rst`, `tcp.flags.syn`, `tcp.flags.fin` and `tcp.flags`.
* [`Find combine`](Element/run/filter/find.md#combine): add `mDNS` and `LLMNR`.
* Some [`<find>`](Element/run/filter/find.md) tags assign two type [`match mode`](Element/run/filter/find.md#match_mode) and [`none match mode`](Element/run/filter/find.md#match_mode).

<h2>2021/02/19</h2>

* RestAPI: replace [`/login`](RestAPI/Submit.md#login) with [`/direct_login`](RestAPI/Submit.md#login).

<h2>2021/02/01</h2>

* Move [`<output>`](Element/run/output.md) tag to category tags.
* [`<filter>`](Element/run/filter.md): [`<find>`](Element/run/filter/find.md) tag add [`NGAP`](https://www.etsi.org/deliver/etsi_ts/138400_138499/138413/15.00.00_60/ts_138413v150000p.pdf) protocol.

<h2>2021/01/27</h2>

* Introducing a new tag [`<regular>`](Element/run/regular.md) and lead into category tags concept.
