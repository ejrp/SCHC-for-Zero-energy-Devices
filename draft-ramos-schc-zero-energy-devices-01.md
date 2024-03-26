---
stand_alone: true
ipr: trust200902
docname: draft-ramos-schc-zero-energy-devices-00
area: Internet
wg: SCHC Working Group
kw: Internet-Draft
cat: info
submissiontype: IETF
pi:
  symrefs: 'yes'
  sortrefs: 'yes'
  strict: 'yes'
  compact: 'yes'
  toc: 'yes'

title: Static Context Header Compression and Fragmentation for Zero Energy Devices
abbrev: SCHC for Zero Energy Devices
wg: SCHC Working Group
author:
- ins: E. Ramos
  name: Edgar Ramos
  org: Ericsson
  street: Hirsalantie 11
  city: 02420 Jorvas, Kirkkonummi
  country: Finland
  email: edgar.ramos@ericsson.com
- ins: L. Corneo
  name: Lorenzo Corneo
  org: Ericsson
  street: Hirsalantie 11
  city: 02420 Jorvas, Kirkkonummi
  country: Finland
  email: lorenzo.corneo@ericsson.com
- ins: A. Minaburo
  name: Ana Minaburo
  org: Consultant
  street: rue Rennes
  city: 35510 Cesson-Sevigne
  country: France
  email: anaminaburo@gmail.com
normative:
  RFC5234:
  RFC2119:
  RFC8724:
informative:


--- abstract

This document describes the use of SCHC for very constraint devices. The use of SCHC will bring connectivity to devices with restrained use of battery and long delays.

--- middle

# Introduction

Zero Energy (ZE) devices are wireless host used in an IoT applications using harvesting energy. These devices can be connected in different topologies and will require a new infrastructure that maintains the connection alive during the long delays these hosts use for communicate. This document explains the different topologies and how SCHC can improve the communication in an 3GPP network.
ToDo, (REPLACE).

This document normatively references {{RFC5234}} and has more
information in 3GPPdocA and 3GPPdocB. (REPLACE)

# Conventions and Definitions

{::boilerplate bcp14-tagged}


# Zero Energy Devices
Zero Energy (ZE) devices are ultra-low-power small electronic circuits that can be used in Internet of Things (IoT) applications. Typically, a ZE device solely relies on the energy that is harvested from the surrounding environment through an energy harvester, e.g., a small solar panel or Radio Frequencies (RF). The harvested energy is often stored in small rechargeable batteries or super-capacitors. However, the most constrained ZE devices are completely passive and could lack energy storage. ZE energy devices typically contain sensors, e.g., temperature, as well as a radio interface to offload sensor readings.

ZE devices do not require any battery replacement, or manual charging, as they harvest energy from their surrounding environment. ZE devices might be small, and come in the form of sensors (which report on data from readings and measurements), trackers (which report on the location of an object or a living being), or actuators (which prompt other machines to operate).

The widespread adoption of ZE devices will lead to a massive reduction in both the cost and power needed to run and maintain IoT systems, making them more scalable. Gathering data from these devices also has the potential to drive higher productivity, pollution reduction, and enriched lifestyles, without requiring any additional energy. Furthermore, battery-less devices are better for the environment and can be managed with simple processes, from manufacturing to disposal.


# Cellular Based ZE-Devices

## 3GPP device classification

At the time of writing, the 3GPP TR 38.848 collects decisions regarding "Ambient IoT", which is another name for ZE IoT used throughout this draft. In that document, three different types of ZE devices are specified based on their energy storage capacity and their RF transmission capabilities.

* Device type A:  Fully passive devices, without any energy storage capability. The peak power consumption is expected to be less than 10 uW. The wireless communication technology used is backscatter communication.

* Device type B. Semi-passive devices with limited energy storage, e.g., super-capacitor or coin-cell battery. The peak power consumption is expected to be in the order of few hundreds of uW. The wireless communication technology used is backscatter communication with the stored energy possible to be used for amplification of the backscattered signal.

* Device type C. Active devices with energy storage. The peak power consumption is expected to be less than 10 mW. The wireless communication technology used is active communication and independent signal generation.

The type of devices A, B, and C are able to demodulate control, data, etc from the relevant entity in RAN according to connectivity topology.

## 3GPP ZE IoT topologies

3GPP currently discusses four topologies to enable communication between ZE devices and the cellular network. Most capable ZE devices may be able to communicate directly with a base station (BS). On the other hand, more constrained ZE devices may need the assistance of intermediary nodes, for example, to provide carrier signals or energy to excite and power up the device. We would focus so far on the topology 1 in this document.

### Topology 1
In Topology 1, see {{Fig-Topo1}}, the ZE device directly and bidirectionally communicates with a base station (BS). The communication between the BS and the ZE device includes device data and/or signaling.

~~~~~~

+----+       +----+
| BS | <---> | ZE |
+----+       +----+
~~~~~~
{: #Fig-Topo1 title='Topology 1. The base station (BS) and ZE device communicate directly.'}


### Topology 2
In Topology 2, see {{Fig-Topo2}}, the ZE device communicates bidirectionally with an intermediate node (IN) between the device and BS. In this topology, the intermediate node can be a ZE-enabled relay, such as a user equipment (UE), meaning other mobile device or equipment, or a repeater. The IN transfers ZE data and/or signaling between BS and the ZE device.

~~~~~~

+----+   Uu  +----+       +----+
| BS | <---> | IN | <---> | ZE |
+----+       +----+       +----+
~~~~~~
{: #Fig-Topo2 title='Topology 2. The base station (BS) and ZE device communicate through an intermediary node (IN).'}

### Topology 3
In Topology 3, see {{Fig-Topo3D}} and {{Fig-Topo3U}}, the ZE device transmits data/signalling to a BS, and receives data/signalling from the assisting node (AN). Alternatively, the ZE device receives data/signaling from a BS and transmits data/signaling to the AN. In this topology, the AN can be a ZE-enabled relay, for example, another UE.


~~~~~~

+----+    Uu    +----+
| BS |--------->| AN |
+----+          +----+
   ^               |
   |    +----+     |
   +----| ZE |<----+
        +----+

~~~~~~
{: #Fig-Topo3D title='Topology 3 (downlink assistance). The base station (BS) utilizes an assisting node (AN) to transmit data to the ZE device.'}


~~~~~~

+----+    Uu    +----+
| BS |<---------| AN |
+----+          +----+
   |               ^
   |    +----+     |
   +--->| ZE |-----+
        +----+
~~~~~~
{: #Fig-Topo3U title='Topology 3 (uplink assistance). An assisting node (AN) relays to the base station (BS) the ZE UL transmission.'}


### Topology 4
In Topology 4, see {{Fig-Topo4}}, the ZE device communicates bidirectionally with a UE. The communication between UE and the ZE device includes ZE data and/or signaling.

~~~~~~

+----+       +----+
| UE | <---> | ZE |
+----+       +----+
~~~~~~
{: #Fig-Topo4 title='Topology 4. A user equipment (UE) and ZE device communicate directly.'}


## User plane characteristics for a Cellular ZE-devices

The nature of the ZE devices requires some changes in the architecture of the radio network protocol stack to minimize the power consumption on the transmissions and simplify operations. The reception of data, even control signaling, also requires energy.

In a design for ZE devices design, the energy that is harvested is preferred to be used for the device's transmissions. Since the ZE devices are expected to have highly uplink-dominated traffic, and therefore the minimization of downlink transmissions (including feedback) can be anticipated.

Also, the transmission opportunities and characteristics require that the handling of the packets is tolerant to delays in the reception and reassembling due to the inherent unreliability of the source of power for such transmissions. Even so, these devices coexist with legacy and the more capable devices that will be utilizing the same mobile networks, and the changes should be compatible with the type of equipment that is typically utilized for cellular networks to favor adoption and economy of scale.

Due to the restricted power on ZE devices, the user plane is expected to be simplified and optimized to reduce the overhead and the need for handling multiple levels of feedback. The power restriction itself and the possible lack of link adaptation and reduction of the feedback might increase the probability of packet loss and in some scenarios also the probability of interference.  This is due to the deployment of many devices in close vicinity that are power-charged by the same type of energy source and therefore possibly activated simultaneously, which may cause access collision to the network as well as interference to other cells.

The mentioned restrictions make the design of the user plane for these kinds of devices is challenging and requires compromises on the current design. This would imply an iterative approach on what components and procedures are kept and which ones are new with respect to the regular cellular devices' operation.

For example, to increase the efficiency, the transmissions may be done at the same time as accessing the network, meaning the utilization of the RACH (Random Access Channel) to reduce the control signaling. Transmissions using RACH are susceptible to collision since they are mostly multiplexed by preambles and timing chosen randomly by the device and currently are not scheduled as the traditional user plane transmission are. The minimization of downlink signaling may have an impact on the possibility of having scheduled traffic, in addition to the impossibility of the network of knowing if a device has enough energy to monitor a particular downlink signaling channel.

The need to reduce overhead and optimize the number of bits over the air to reduce the power required to transmit is a clear requirement of the ZE devices. Consequently, the use of SCHC (Static Context Header Compression) {{RFC8724}} has a great potential to reduce the quantity of data needed to be sent over the air, as well as provide elements that can be used to increase reliability, support for fragmentation, and potentially manage the problem of the long delays between transmissions. The delays may happen when a device has just enough energy for transmitting certain packets but not enough to empty the buffer. Part of the energy might be needed for the reception of packets from the network.

The network is capable of managing the possibility that a full object might not be received soon after a transmission is started. This increases the requirement of how long the fragments and packet might need to be kept in buffers, so it is avoided to lose the energy that the devices have used in the initial transmission(s). This enables that the device can continue with the rest of the packets once the power for a new transmission has been harvested. Of course, the buffers should be stored as long as it makes sense for the use case of the device, and therefore it might require certain degree of configuration, in some cases at the devices and in others at the network, or both.

The possibility of collisions between transmissions and the lack of power control and link adaptation may affect the reliability of the delivery of packets. But still, the restriction of power for transmitting and reception and the delays make challenging the support for reliability based on retransmissions. In this respect, we could think that there is a tradeoff between the reliability and additional delay in receiving the data. In some scenarios, these delays could make sense and in others, the delay could make the packets irrelevant to their use case. In that sense equally to the previous point, the configuration of the delays targeting for reliability is important.

From the required characteristics outlined for the user plane, the use of SCHC becomes relevant to fulfill them. SCHC offers fragmented packet corruption detection, and delivery reliability window-based mechanisms, such as ACK-always (Each fragment delivery is explicitly acknowledged) and ACK-on Error (only detected losses trigger delivery reports outlining the fragment loss).  

The requirements can be addressed with some additional complements to support the deployment of SCHC into the cellular Zero Energy device scenarios. For example, adding support for object transport in contrast to only IP packet support, and providing better management of long delays. In addition, a solution to enable the set up of the contexts and rules that make sure there is alignment between the network and the devices on the management of packets. Part of this can be accomplished by imagining that a complete object fits an imaginary jumbo IP package and SCHC would then fragment such packet into pieces that can be fitted in the radio transport block.

In this way, a great part of the overhead is removed and the SCHC services would take care of the reliability and delay-friendly transmission of packets. In addition, there is the possibility of integrating even further SCHC to the cellular lower protocol layers, for example by not relying on feedback from MAC for the reliability of transmission of packets but instead using the fragment bitmap from SCHC. This also may improve the power efficiency of each transmission since the device does not need to monitor the feedback channel after each transmission.

The big challenge in using SCHC in this fashion is how to configure the SCHC fragmentation and reassembly entities. A Dev using SCHC and the endpoint where SCHC is terminated in the network with the relevant context information so the transmitter and the receiver have an understanding of what are the parameters of operation for this particular case, which would depend on the network load and devices power availability for transmission and the maximum allowed delay configuration.


### End-to-end view

The traffic characteristics of ZE devices and their use case might drive the development of the end-to-end interactions and protocol stack. In mostly uplink-dominated cases, the device would produce information that needs to be collected due to the potential delays by a platform instead of being transmitted to a particular application due to the requirement of availability. In the case of applications using the generated data, would in most cases fetch the data from such platforms, and therefore the connectivity towards the final application might not be direct. Therefore, it is highly probable that the direct communication stack can in most cases be assumed to be mediated by a data collection platform.

One option is that such a platform is provided by operators. In that case, it makes sense to incorporate SCHC as part of the protocol stack between the network and the terminal. This option would require some knowledge of the application protocol stack by the mobile network so that effective compression can be realized. This type of deployment would maximize the energy efficiency by optimizing the compression up to the transport block level reducing additional overhead from padding and lower layers headers. In this scenario the application would only receive the payload whenever a packet or an object is fully assembled, reducing the need for additional implementation to application logic. When transmitting a complete object in full, SCHC could be utilized in a similar way to a transport protocol due to its fragmentation features. They enable transmissions over long periods of time and reconstruct the full object after receiving all fragments and also provide some reliability control on the fragments transmitted.

Another option is the enabling of configurable data collection platforms, which would imply providing SCHC support over the top in the application layer. For this option, the SCHC packets would look like non-IP traffic for the network, and the reliability of the packets, delay management, and reassembling of fragments need to be handled by the application. Therefore, the delays in transmissions and changes in network connection points need to be handled and accounted for.


## SCHC as a size and delay-optimized transmission mechanism

SCHC mechanisms can be used to provide reliability and segmentation and then extended to provide delay-tolerant transmissions of large objects. This can be done by using the SCHC Fragmentation/Reassembly mechanism Ack on Error {{RFC8724}} which divides the object into smaller chunks called tiles that are transmitted according to a network's scheduled occasions considering the device power saving and state configuration. The configuration and setup of SCHC object transfer session considering the network and terminal states according to the needs of each ZE device matching to their use case becomes a critical functionality to address.

### General architecture

The {{Fig-Archi}} shows a high configuration of the network communication between a ZE Device and an Application Server (App). ZE Dev have short-live intermittent connections and need a middle host called proxy that will maintain the connection state even the communication is discontinued with the ZE Dev and a continue communication with the Application Server. The proxy may answer to some request instead of the ZE Dev.   


~~~~~~

+-------+        +-------+       +--------+
|       | <--->  |       | <---> |        |
|  ZE   |  ...   | Proxy | <---> | App.   |
| Dev.  |(delay) | (SCHC)| <---> | Server |
|(SCHC) | <--->  |       | <---> | (SCHC) |
|       |  ...   |       | <---> |        |
+-------+        +-------+       +--------+

~~~~~~
{: #Fig-Archi title='High Level Communication Architecture'}



### Device-initiated transmissions

Once a device is onboarded into a network, or during the network connection procedure, it must be configured with a new threshold value MAX_OBJECT_SIZE, measured in bytes). This configuration could be also pre-defined and notified to the network using out-of-band methods. This is used to compare with the object size to be transmitted. If the object size exceeds such threshold, it means that it is required to operate with a delay-friendly transmission configuration and it will use the most adequate SCHC delay values that are capable of handling the object size to be transmitted by the device. The most adequate configuration is such that can handle (bigger or equal) the size of the object to be transmitted according to the MAX_OBJECT_SIZE associated configuration.

To avoid collisions and help with the network management of multiple devices accessing the network simultaneously, the configuration could include a Best Effort Transfer Interval (BETI). A BETI configuration is meant to provide pacing information to the SCHC device. After each BETI the device attempts to transfer number of SCHC tiles. The value of BETI could be based on a timer (send new fragment every X second), transmission occasions (send every X occasion), or radio events (paging, DRX/DTX cycle, etc.). Also, the values of BETI can be also determined by a random timer given by a configured range. The number of tiles to send in each BETI, a Tile Count (TC) parameter, is by default 1 but can be configured by the network to be higher number.

The SCHC Rule for these devices may be a well-known rule that will not need to be updated. If the Proxy has several devices attached, it must recognize which one is sending.


### Network initiated transmission
If there is a need for the network to transmit data to a device in some cases may require transmitting to a large number of devices and potentially even the same network delivery points (e.g., radio base stations). To accomplish this in a scenario where the compressor entity is in the cellular network, it will need to have a copy of the object to be delivered to the device to transmit it to the device according to a suitable scheduling and agreed configuration. As mentioned before, this would require the network to provide APIs to Applications Servers (AS) that either provide an interface to upload to the network the object to be transferred beforehand or a proxy IP address for large object transfers that would buffer the object for further transmission if the data were from the application layer. The delivery may reuse the same mechanisms used to provide IP tunneling transmissions or non-IP transmissions already specified in the cellular standards.


## SCHC context configuration and additional parameters for ZE transmission

### Context provisioning

SCHC successful header compression happens only when a common context is shared between sender and receiver. Typically, context provisioning is outside the scope of SCHC RFC documents, mainly because there may be several ways to implement it. However, the most constrained ZE devices, e.g., 3GPP ZE type 0, may not be able to receive packets from the network, thus dramatically restricting context provisioning possibilities. Hence, this document also discusses how a SCHC context may be provisioned to ZE devices with no reception capabilities.

Discussion of the possibilities:

*  Standardized set of rules that ZE device manufacturers include in their firmware. Viable solution but may lead to even more heterogeneity in the IoT ecosystem. In fact, different vendors may support different non-overlapping subsets of SCHC contexts or none at all.

* Third-party entities or device owners upload and maintain the SCHC contexts, for example flashing the MCU. Manual process and not really scalable.

* NFC or equivalent interfaces for SCHC context provisioning. Add costs for the interface, it is a non-scalable manual process.

* Use of a well-known rules, provisioned at device configuration.


### Context updating

Since SCHC works with static context information, it is not likely (or desired) to update the SCHC delay tolerant configurations very often (e.g., more than once a week -- what exactly is "often" depends on the device capabilities and typical communication frequency), so the most feasible options are that the network would produce a set of pre-configured configurations that are addressed individually with a configuration ID. This means that the network could configure, for example, rules for one device for maximum SCHC packet size large, medium, and small and use three context groups where it applies this parameter setting. In turn, the SCHC MAX_PACKET_SIZE will be set to such values.


In the case of SCHC being utilized as a transport protocol to transmit an object, the size of the tiles used to fragment the object could be set to the MTU of the bearer where the transmission will be realized, for example, if the data is transmitted using regular transmission channels, the MTU would be 1358 bytes in most of the cases.
The SCHC standard fragmentation inactivity timers and fragmentation retransmission timers can be also set according to the scheduling calculation and the expected time of delivery (based on the schedule) for the large packets. Those timers are applied to the fragments that are transmitted and their acknowledgments.

The network can use the expected scheduling time for one of the rule groups and set several parameters according to multiple scheduling situations, for example, extra-long delay, long delay, medium delay, sort delay, and no delay.
In a situation with a delay configuration, the retransmission timer and the inactivity timer would be set to a reasonable value (e.g., 24 hours), meanwhile, in no delay settings, the timers would be set to significantly smaller values (e.g., 10 minutes). The values of the timers would be also correlated to the SCHC window (i.e., successive tiles in a group) size selected which translates to how many transmissions of the tiles are expected to check the correct reception of the tiles belonging to one window. Shorter timers would correspond to shorter window sizes (i.e., a smaller number of tiles would be sent, and hence shorter retransmission/inactivity time is appropriate), meanwhile, larger timer values would correspond to larger window sizes. The window size would also depend on how many tiles the object is fragmented into.

The profile also would have information in reference to the maximum number of Attempts, meaning how many retransmissions of one packet (after the retransmission timer has expired) should be attempted before aborting the transmission. In cases of devices with a history of bad coverage (known from, e.g., connectivity logs for that device), this setting could be set to a higher number (for example 10), and in more common cases for a cellular network where reliability is high, to just one retransmission. Similarly, if the uplink seems to be the problem, then the adjustment could be done in the MAX_ACK_REQUESTS, where the sender would poll the receiver to transmit a bitmap with the received packets if needed and retransmit the request if the retransmit timer expires the number of times that MAX_ACK_REQUESTS is configured to.


### Payload compression
ToDo

### Fragmentation parameters
ToDo

# IANA considerations
This document has no IANA actions.

# Security considerations
This document does not add any security considerations and follows the {{RFC8724}}.


--- back

# Appendix A {#AppendixA}

This becomes an Appendix (REPLACE)

# Acknowledgements


The authors would like to thank (in alphabetic order): ToDo
