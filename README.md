# TN40xx
Tehuti Networks TN40XX Network Adapter driver for FreeBSD
=========================================================

Release Notes
=============
Version 1.0, svn 3714, 2019/01/22
	- Initial release for FreeBSD 11.2 and 12.0

Version 1.1, svn 3737, 2019/02/26
	- Fix for TCP Retries
	- Fix for short packet handling (SMB RX fail)
	- Fix wrong NIC name reporting
	- Latest PHY firmware and code files as of Linux 0.3.6.17.1
	- Remove some redundant error messages

Supported FreeBSD versions:
---------------------------
	tn40xx_freebsd11_amd64-1.1.txz - FreeBSD/FreeNAS 11.2, 64b version
	tn40xx_freebsd12_amd64-1.1.txz - FreeBSD 12.0, 64b version

Supported TN40XX based products:
--------------------------------

VID /DID /SVID/SDID	Interface			Print Name
------------------------------------------------------------------
1fc9/4020/1fc9/3015	CX4 				"TN9030 10GbE CX4 Ethernet Adapter"
1fc9/4020/180c/2040	CX4 				"IEI Mustang-200 10GbE Ethernet Adapter"
1fc9/4022/1fc9/3015	SFP+ APM QT2025 		"TN9310 10GbE SFP+ Ethernet Adapter"
1fc9/4022/1186/4d00	SFP+ APM QT2025			"D-Link DXE-810S 10GbE SFP+ Ethernet Adapter"
1fc9/4022/1043/8709	SFP+ APM QT2025			"ASUS XG-C100F 10GbE SFP+ Ethernet Adapter"
1fc9/4022/1432/8103	SFP+ APM QT2025			"Edimax 10 Gigabit Ethernet SFP+ PCI Express Adapter"
1fc9/4024/1fc9/3015	10GBASE-T Pele MV88X3110	"TN9210 10GBase-T Ethernet Adapter"
1fc9/4025/1fc9/3015	NBASE-T Aquantia AQR105		"TN9510 10GBase-T/NBASE-T Ethernet Adapter"
1fc9/4025/1186/2900	10GBASE-T Aquantia AQ2104	"D-Link DXE-810T 10GBase-T Ethernet Adapter"
1fc9/4025/1432/8102	NBASE-T Aquantia AQR105		"Edimax 10 Gigabit Ethernet PCI Express Adapter"
1fc9/4026/1fc9/3015	SFP+ TI TLK10232		"TN9610 10GbE SFP+ Ethernet Adapter"
1fc9/4026/4c52/1000	SFP+ TI TLK10232		"LR-Link LREC6860AF 10 Gigabit Ethernet Adapter"
1fc9/4027/1fc9/3015	NBASE-T Pele MV88X3310	"TN9710P 10GBase-T/NBASE-T Ethernet Adapter"
1fc9/4027/1432/8104	NBASE-T Pele MV88X3310	"Edimax 10 Gigabit Ethernet PCI Express Adapter"
1fc9/4027/1154/0368	NBASE-T Pele MV88X3310	"Buffalo LGY-PCIE-MG Ethernet Adapter"
1fc9/4027/1546/4027	NBASE-T Pele MV88X3310	"IOI GE10-PCIE4XG202P 10Gbase-T/NBASE-T Ethernet Adapter"
1fc9/4027/4c52/1001	NBASE-T Pele MV88X3310	"LR-Link LREC6860BT 10 Gigabit Ethernet Adapter"
1fc9/4027/1baa/3310	NBASE-T Pele MV88X3310	"QNAP LAN-10G1T-THT 10Gbase-T/NBASE-T Ethernet Adapter"
1fc9/4527/1fc9/3015	5GBASE-T Pele MV88E2010	"TN9710Q 5GBase-T/NBASE-T Ethernet Adapter"


Installing the package
----------------------
	% sudo pkg add tn40xx_freebsd12_amd64-1.1.txz
	% sudo reboot


	If pkg add command fails with the following message:
	% sudo pkg add tn40xx_freebsd12_amd64-1.1.txz
	Signature for pkg not available

	It could mean pkg tool is not correctly setup on your system. In this case, run pkg with no arguments and while host have internet access:
	% sudo pkg
	The package management tool is not yet installed on your system.
	Do you want to fetch and install it now [y/N]:
	
	Enter or answer "y" to complete pkg tool installation

Verifying installed package
---------------------------
	% pkg info | grep tn40

Uninstalling the package
------------------------
	% sudo pkg delete tn40xx_freebsd12_amd64-1.1
	% sudo reboot

Configuring adapter
-------------------
	% sudo vi /etc/rc.conf
	For DHCP:
		ifconfig_tn400="DHCP"
	For Static IP, ipv4:
		ifconfig_tn400="inet 17.0.0.201 netmask 255.255.255.0"
	For Static IP, ipv4 and MTU:
		ifconfig_tn400="inet 17.0.0.201 netmask 255.255.255.0 mtu 9000"
	For Static IP, ipv6:
		ifconfig_tn400_ipv6="inet6 2017::201 prefixlen 64"
	For Bonding, failover mode, tn400+tn401, DHCP:
		ifconfig_tn400="up"
		ifconfig_tn401="up"
		cloned_interfaces="lagg0"
		ifconfig_lagg0="laggproto failover laggport tn400 laggport tn401 DHCP"
	% sudo reboot

	* If there is more than one adapter present, they will be named tn400, tn401, etc.
