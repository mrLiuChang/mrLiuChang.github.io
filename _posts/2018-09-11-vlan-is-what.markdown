---
layout: post
title: "Vlan is What???"
date: "2018-09-11 15:14:56 +0800"
categories: [tech]
---
>VLAN（Virtual Local Area Network）的中文名为"虚拟局域网"。
虚拟局域网（VLAN）是一组逻辑上的设备和用户，这些设备和用户并不受物理位置的限制，可以根据功能、部门及应用等因素将它们组织起来，相互之间的通信就好像它们在同一个网段中一样，由此得名虚拟局域网。VLAN是一种比较新的技术，工作在OSI参考模型的第2层和第3层，一个VLAN就是一个广播域，VLAN之间的通信是通过第3层的路由器来完成的。与传统的局域网技术相比较，VLAN技术更加灵活，它具有以下优点： 网络设备的移动、添加和修改的管理开销减少；可以控制广播活动；可提高网络的安全性。
在计算机网络中，一个二层网络可以被划分为多个不同的广播域，一个广播域对应了一个特定的用户组，默认情况下这些不同的广播域是相互隔离的。不同的广播域之间想要通信，需要通过一个或多个路由器。这样的一个广播域就称为VLAN。
---百度百科


>虚拟区域（Virtual Local Area Network或简写VLAN, V-LAN）是一种建构于局域网交换技术（LAN Switch）的网络管理的技术，网管人员可以借此透过控制交换机有效分派出入局域网的数据包到正确的出入端口，达到对不同实体局域网中的设备进行逻辑分群（Grouping）管理，并降低局域网内大量数据流通时，因无用数据包过多导致拥塞的问题，以及提升局域网的信息安全保障。---wiki


VLAN的运作原理与实现方式
物理层（physical layer）
直接以交换机上的端口做为划分VLAN的基础。

这个方式的优点是简单与直观，因此，运用这种设置VLAN的情况十分普遍。但因为是物理层的设置，所以比较适合在规模不大的组织。

数据链接层（data link layer）
以每台主机的MAC地址做为划分VLAN的基础。方法是先创建一个比较复杂的数据库，通常为某网络设备的MAC地址与VLAN的映射关系数据库。当该网络设备连接到端口后，交换机会向VMPS（VLAN管理策略服务器）来请求这个数据库。找到相应映射关系，完成端口到VLAN的分配。

这个方式的优点是即使计算机在实体上的位置不同，也不影响VLAN的运作。但缺点是网管人员必须在交换机中设置组织内每一台设备MAC地址与VLAN间的映射关系数据库。因此，这种设置策略的管理复杂度会随着越来越多的设备、与实体位置的群落、和不同工作任务需要而增加。

网络层（network layer）
以每台设备的IP地址做为划分VLAN的基础，以子网视为VLAN设置的依据。

这个方式的优点是当网管人员已经将内部网段做好规划与分配的情况下，将可大辐降低网管人员规划并设置VLANs架构的复杂度。但缺点是原本传统交换机不需要对讯框作任何处理，但在这个机制下，交换机不但必须剖析讯框（Frame），还必须进一步取出Source IP与Destination IP进行比对，连带降低交换机接收与分派数据包的效率。

>一个VLAN相当于OSI模型第2层的广播域，它能将广播控制在一个VLAN内部。而不同VLAN之间或VLAN与LAN / WAN的数据通信必须通过第3层（网络层）完成。否则，即便是同一交换机上的连接端口，假如它们不处于同一个VLAN，正常情况下也无法进行数据通信
