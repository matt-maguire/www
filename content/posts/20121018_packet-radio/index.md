+++
title = "Packet Radio Resources"
date = 2013-03-21
lastmod = 2025-01-26T11:59:47+11:00
tags = ["HamRadio", "PacketRadio", "AX25"]
categories = ["Blog"]
draft = false
weight = 3001
author = "Matt Maguire"
+++

## AX.25 Packet on Linux {#ax-dot-25-packet-on-linux}

Recently I have been experimenting with setting up packet radio on an ASUS netbook running Debian Linux. There have been a few challenges with this, which I describe below.

-   Installation of AX25 Lib/Tools/Apps: the problem here is that the packages supplied with debian are a little old, and assume that the kernel has support for legacy BSD-style pseudo-terminals. However, in the standard kernel distributed with debian linux, support for the legacy ptys has not been compiled in. So, I had a choice of recompiling the kernel to support the legacy ptys, or else compile up newer versions of the ax25libs/tools/apps that support the new Unix98-style ptys. I chose the latter.
-   The AX.25 "call" program supplied in the ax25apps package was crashing when I resized the window, and didn't have support for any scrollback buffer. I found a [patch](https://marc.info/?l=linux-hams&m=126174094113550) on the web that addresses these issues, but it was built against ax25-apps-0.0.8-rc2, and wouldn't apply cleanly to the more recent version of call.c in CVS that supports the Unix98 PTYs. So, I updated the patch to make it compatible with the newer release, and you can [download the updated patch here](call_patch.txt).
-   I had an issue with getting [linux soundmodem](http://www.baycom.org/~tom/ham/soundmodem/) to work at 300 baud. I found the fix was to use modem tones less than 4\*300=1200 Hz. However, I wanted to cover the national APRS freqnency on 40 m and the frequency used by our local packet group around 1 kHz higher together at the same time with the same radio. I found a [testing patch](soundmodem_300baud_patch.txt) against soundmodem-0.16 that removes this restriction on the modem tones.
-   The aprsd packaged with debian doesn't support multiple TNCs, and you can only run one instance of it on the system. So, I looked to the APRX daemon instead. This does support multiple TNCs, but unfortunately kept dumping core on my system. I made a [small patch](aprx_telemetry_patch.txt) to prevent this from happening.
-   I found a patch to provide Internode Protocol (INP3) Support for Linux on [this page](http://sharon.pi8zaa.ampr.org/users/pe1rxq/inp3.html). However, the patch was written against linux kernel 2.6.4, and does not apply cleanly to the latest 2.6.32 kernel. So, I [updated the patch](2.6.32-inp3_007_patch.txt). However, I found that the patch causes a soft lockup when taking an AX25 interface out of service, due to a deadlock with the `spin_locks`, so some additional work is needed to address this. Investigations so far indicate a problem with `nr_del_node_found()`, which removes neighbours and nodes that are no longer valid. When it is called, sometimes there is already a lock on the global neighbour list, and sometimes there is also a lock on the global node list. When removing a neighbours or node, an attempt will be made to acquire a lock on the global neighbour or node list, and if such a lock is already existing, there is a deadlock, and the CPU will just spin. Will need to think about the best way to clean up the code.


## Packet Radio Resources {#packet-radio-resources}

I have collected here some useful links in my search to learn about packet radio.

-   [AMPRNet Portal](https://portal.ampr.org/index.php) to manage 44.0.0.0/8 IP allocations and ampr.org DNS resource records.
-   [Packet Radio Overview](http://wiki.complete.org/PacketRadio) at Complete.Org
-   [Larry Kenney WB9LOZ](http://www.choisser.com/hamradio/packet.html) packet radio information page, including his [Introduction to Packet Radio](http://www.choisser.com/packet/) series of articles
-   [VK2DOT Packet Site](http://vk2dot.dyndns.org/), including his [Packet Setup page](http://vk2dot.dyndns.org/XR32/VK2DOT-IP-Setup.htm).
-   [Linux AX.25 Page](http://www.linux-ax25.org/wiki/Main_Page)
-   [DB0ANF BBS page](http://www.db0anf.de/app/bbs), can view packet BBS via web
-   [JNOS](http://www.langelaar.net/projects/jnos2/downloads/linux/) developer page (and [KF8KK's JNOS Setup Guide](http://www.kf8kk.com/packet/jnos-linux/linux-jnos-setup-ftpusers-txt.htm))
-   [Soundcard Packet page](http://www.soundcardpacket.org/) by Ralph Miles NM5RM
-   [IP-in-UDP encapsulation](http://www.langelaar.net/projects/jnos2/documents/ipudp.txt) with JNOS
-   [AMPR Gates](http://www.ampr-gates.net/frame_e.htm) covering packet radio gateways and AXIP encapsulation
-   [Hosts and DNS Zone files](ftp://hamradio.ucsd.edu/pub/) for ampr.org domain and 44.0.0.0/8 net
-   [N5VDA Packet page](http://www.vdazone.org/lantzdocs/packet.html), with list of packet software
-   [AMPR Gateways page](http://www.ampr-gates.net/frame_e.htm)
-   [DX Cluster](http://www.dxcluster.info/dxcsoft.htm) information page
-   [Net/Rom Manual](http://www.a00.de/tcpgroup/1988/msg00006.php) and [Original NET/ROM Paper](http://www.ir3ip.net/iw3fqg/doc/ipax25.htm)
-   Running [Packet radio on Linux](http://www.qbjnet.com/packet.html)
-   Linux [AX25 HOWTO](http://www.tldp.org/HOWTO/AX25-HOWTO/)
-   [AGWPE Yahoo Group](http://groups.yahoo.com/group/SV2AGW/)
-   [AX.25 Protocol standard](http://www.tapr.org/pdf/AX25.2.2.pdf)
-   [Timewave](http://www.timewave.com/download.html) manufacturer of PK232 TNC
-   [FBB Main Page](http://ftp.f6fbb.org/) and [FBB Setup for Dummies](http://www.qsl.net/ok2pen/LinuXFBB.htm)
-   [AX25IP README](http://mirror.switch.ch/ftp/pool/3/mirror/hamradio-ucsd/packet/misc/README.ax25ip)
-   [FPAC Mini-HOWTP](http://rose.fpac.free.fr/)
-   [N8UR Packet pages](http://www.febo.com/packet/index.html)
-   [TAPR SW Library](ftp://ftp.tapr.org/software_lib/Linux/ax25/)
-   Linux [AX25 SW Suite](http://www.linux-ax25.org/wiki/CVS) and [AX25SPY Page](http://linkt.de/ax25spyd/)
-   The [Problem with Digipeaters](https://qsl.net/vk2rq/digipeaters.html) by Tom Clark W3IWI, 1986
-   Installing [LinBPQ](https://dl.dropbox.com/u/31910649/InstallingLINBPQ.htm) BPQ Node/Mail/Chat on Linux


## Packet Activity in the Local Area {#packet-activity-in-the-local-area}

| Freq/MHz     | Callsigns                                    | Comments                                                                         |
|--------------|----------------------------------------------|----------------------------------------------------------------------------------|
| 7.03717      | VK2IO BBS, VK2WL, VK2SX, VK2TT, VK4ALN et al | LSB w 1600/1800 Hz tones: The VK2IO HF BBS is stand-alone,                       |
|              |                                              | but VK2IO-1 node has connectivity via Internet                                   |
| 145.175      | VK2MB-1, VK2BEN-1 et al                      | National APRS Frequency, lots of activity                                        |
| 144.875      | VK2TGB                                       | VK2TGB BBS in the Blue Mountains (part-time only)                                |
| 147.575      | VK2IO-1, VK2AMW-7, VK2DOT-1                  | VK2IO BBS/VK2IO-1 node is separate instance from VK2IO on HF;                    |
|              |                                              | links to wider packet network with message forwarding (via RF only, no internet) |
|              |                                              | I can connect from my QTH via VK2AMW-7 node/digipeater                           |
| 439.150(out) | VK2RAG data regenerator                      | Reception is too marginal from my QTH, need a better antenna :-)                 |
| 434.150(in)  |                                              | Would then be able to access VK2IO, VK2DOT.                                      |

There are some others I will check out listed on [Steve VK2KFJ's Packet page](http://www.qsl.net/vk2kfj/pacradio.html), (the info is dated, so some of these systems may not be running anymore)
Channels on the VK2MB Packet Digipeater

On the radio used for packet digipeating at the MWRS club house, the following channels have been programmed:

| Channel | Freq/MHz | Comments                                                                  |
|---------|----------|---------------------------------------------------------------------------|
| 1       | 147.575  | Main Channel (default), access VK2IO-1, VK2DOT-1, VK2RQ-7, VK2AMW-7 nodes |
| 2       | 144.425  | VK2RSY beacon (for diagnostics/testing)                                   |
| 3       | 144.700  | Backup port to VK2DOT-1 XRouter                                           |
| 4       | 144.875  | Backup port to VK2IO &amp; VK2TGB BBS systems                             |
| 5       | 145.175  | Primary APRS frequency in VK                                              |
| 6       | 145.200  | Alternate local packet frequency                                          |
| 7       | 147.600  | Alternate local packet frequency                                          |
