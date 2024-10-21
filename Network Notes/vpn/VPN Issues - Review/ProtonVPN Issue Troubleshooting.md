
0807
Clear App Data - protonVPN
logs showed looking for: ProtonVPN Starter in Container, plist in Application Support, couldnt find
deleted ~/Library/Preferences/ch.proton.mac.plist
resetNet

recreated plist in Container 
~/Library/Containers/ch.protonvpn.mac/Data/Library/Preferences/ch.protonvpn.mac.plist
ch.protonvpn.mac.plist UnprotectedNetwork = 1

~/Library/Group\ Containers/J6S6Q257EK.group.ch.protonvpn.mac/.com.apple.containermanagerd.metadata.plist

Starterfile found in ~/Library/Application\ Support/com.apple.sharedfilelist/com.apple.LSSharedFileList.ApplicationRecentDocuments/ch.protonvpn.protonvpnstarter.sfl3

~/Library/Containers/ch.protonvpn.mac/Data/Library/Application\ Support/
~/Library/Containers/ch.protonvpn.mac/Data/Library/Cookies
~/Library/Containers/ch.protonvpn.mac/Data/Library/Cache


###### VPN File Locations 
~/Library/Containers/ch.protonvpn.mac
~/Library/Application\ Support/com.apple.sharedfilelist/com.apple.LSSharedFileList.ApplicationRecentDocuments/ch.protonvpn.protonvpnstarter.sfl3

/Library/Preferences/com.apple.networkextension.plist  - VPN configuration
/Library/Preferences/com.apple.networkextension.cache.plist  - VPN route subnets

keychains  

flush routes (flush ; flush6)

reset Network Configurations/ Interfaces ( resetNet )

systemextensionsctl uninstall J6S6Q257EK ch.protonvpn.mac.WireGuard-Extension
systemextensionsctl uninstall J6S6Q257EK ch.protonvpn.mac.OpenVPN-Extension


##### failed to delete vpn
* Failed to delete item | {"error":"Error Domain=NSCocoaErrorDomain Code=4 \"“Caches” couldn’t be removed.\" UserInfo={NSUserStringVariant=(\n    Remove\n), NSFilePath=\/Users\/meuthu\/Library\/Containers\/ch.protonvpn.mac\/Data\/Library\/Caches, NSUnderlyingError=0x6000016a3d50 {Error Domain=NSPOSIXErrorDomain Code=2 \"No such file or directory\"}}","path":"\/Users\/meuthu\/Library\/Containers\/ch.protonvpn.mac\/Data\/Library\/Caches"}
* Failed to delete item | {"error":"Error Domain=NSCocoaErrorDomain Code=4 \"“ch.protonvpn.mac.plist” couldn’t be removed.\" UserInfo={NSUserStringVariant=(\n    Remove\n), NSFilePath=\/Users\/meuthu\/Library\/Containers\/ch.protonvpn.mac\/Data\/Library\/Containers\/ch.protonvpn.mac\/Data\/Library\/Preferences\/ch.protonvpn.mac.plist, NSUnderlyingError=0x6000016a18c0 {Error Domain=NSPOSIXErrorDomain Code=2 \"No such file or directory\"}}","path":"\/Users\/meuthu\/Library\/Containers\/ch.protonvpn.mac\/Data\/Library\/Containers\/ch.protonvpn.mac\/Data\/Library\/Preferences\/ch.protonvpn.mac.plist"}
* Failed to delete item | {"error":"Error Domain=NSCocoaErrorDomain Code=4 \"“ch.protonvpn.ProtonVPNStarter” couldn’t be removed.\" UserInfo={NSUserStringVariant=(\n    Remove\n), NSFilePath=\/Users\/meuthu\/Library\/Containers\/ch.protonvpn.mac\/Data\/Library\/Containers\/ch.protonvpn.ProtonVPNStarter, NSUnderlyingError=0x6000016a13e0 {Error Domain=NSPOSIXErrorDomain Code=2 \"No such file or directory\"}}","path":"\/Users\/meuthu\/Library\/Containers\/ch.protonvpn.mac\/Data\/Library\/Containers\/ch.protonvpn.ProtonVPNStarter"}
* Failed to delete item |{"path":"\/Users\/meuthu\/Library\/Containers\/ch.protonvpn.mac\/Data\/Library\/Application Support\/database.sqlite","error":"Error Domain=NSCocoaErrorDomain Code=4 \"“database.sqlite” couldn’t be removed.\" UserInfo={NSUserStringVariant=(\n    Remove\n), NSFilePath=\/Users\/meuthu\/Library\/Containers\/ch.protonvpn.mac\/Data\/Library\/Application Support\/database.sqlite, NSUnderlyingError=0x6000016a03f0 {Error Domain=NSPOSIXErrorDomain Code=2 \"No such file or directory\"}}"}


--------------------

update pfctl - macsed_pf_anchors

--------------------
/Library/Preferences/SystemConfiguration/preferences.plist 
Killall -9 cfprefsd

cfprefsd
cfprefsd provides preferences services for the CFPreferences and NSUserDefaults APIs.
The CF in CFPreferences stands for Core Foundation. According to Apple's developer documentation, Core Foundation is how your Mac manages both system-wide and application-specific preferences:
https://www.howtogeek.com/346093/what-is-cfprefsd-and-why-is-it-running-on-my-mac/https://www.howtogeek.com/346093/what-is-cfprefsd-and-why-is-it-running-on-my-mac/


https://stackoverflow.com/a/29053594
 ps -ax -Ouser | grep [c]fprefsd
  144 root               ??  Ss     0:10.71 /usr/sbin/cfprefsd daemon
  263 _locationd         ??  S      0:00.07 /usr/sbin/cfprefsd agent
  272 _cmiodalassistants   ??  S      0:00.07 /usr/sbin/cfprefsd agent
  393 meuthu             ??  S      0:08.44 /usr/sbin/cfprefsd agent
  669 _softwareupdate    ??  S      0:00.04 /usr/sbin/cfprefsd agent

--------------------

idx16 tcp4 10.1.144.21:50228<->ec2-35-174-127-31.compute-1.amazonaws.com:443      idx-16   Established        6124 B
udp4 on vpn
protocols
lo0 https://datacadamia.com/network/loopback
lo0 inet6 

ipv6 - https://dnschecker.org/ipv6-whois-lookup.php


----
Blocks:
IPV6 configured "Off"
WPA3 Personal Enabled

--------------------
ProtonVPN IPv6
https://protonvpn.com/support/prevent-ipv6-vpn-leaks/

ProtonVPN shows disabling IPV6 - link-local rather than "Off" - https://protonvpn.com/support/how-to-disable-ipv6-protocol-on-macos/

[Note: Top VPN providers offer IPv6 leak protection implemented in their applications, but it is safer to disable IPv6 from your machine. Disabling IPv6 at the system level guarantees no leaks are possible.

For your own safety, we recommend that you disable the IPv6 any time you set up a manual VPN connection on your Mac, no matter the chosen VPN protocol (IKEv2, L2TP over IPSec, OpenVPN).]


A null route or black hole route is a network route (routing table entry) that goes nowhere. Matching packets are dropped (ignored) rather than forwarded, acting as a kind of very limited firewall. The act of using null routes is often called blackhole filtering. 

Black hole filtering refers specifically to dropping packets at the routing level, usually using a routing protocol to implement the filtering on several routers at once, often dynamically to respond quickly to distributed denial-of-service attacks.

Remote Triggered Black Hole Filtering (RTBH) is a technique that provides the ability to drop undesirable traffic before it enters a protected network.[4] The Internet Exchange (IX) provider usually acquires this technology to help its members or participants to filter such attack [5]

Null routes are typically configured with a special route flag; for example, the standard iproute2 command ip route allows to set route types unreachable, blackhole, prohibit which discard packets. Alternatively, a null route can be implemented by forwarding packets to an illegal IP address such as 0.0.0.0, or the loopback address.

PROTONVPN IPSec IKEv2
IKEv2 is the VPN protocol officially supported on all Apple devices (Mac computers, iPhones, and iPads), but the way that Apple implements VPN connections is badly flawed. 

IPSec
Internet Protocol Security (IPSec) is a flexible protocol suite that provides a framework for securing VPN connections. Crucially, it:
Sets up the key exchange between your device and the VPN server. 
Provides authentication to verify the source of data packets and ensure they haven’t been tampered with during transit.
Encrypts and decrypts data sent over the VPN connection
As a framework rather than a complete solution itself, IPSec supports multiple protocols and encryption standards to perform these functions.

IKEv2
IKE is used to set up a security association (SA) for IPSec when connecting your device and the VPN server. When IPSec is used with IKEv1, it’s often referred to simply as IPSec.

on apple implementation:
IKEv2 is the VPN protocol officially supported on all Apple devices (Mac computers, iPhones, and iPads), but the way that Apple implements VPN connections is badly flawed. 
However, IPSec has no known weaknesses when implemented with IKEv2 (Apple’s implementation of IKEv2 is problematic, but the problem lies with Apple, not IKEv2/IPSec itself).  
IKEv2 continues to be widely supported because it’s the VPN protocol officially supported on Apple devices. But as we’ve already mentioned, Apple’s implementation of IKEv2 is best avoided.


--------------------
NETSTAT -F INET6 (vpn enabled)
Active Internet connections
Proto Recv-Q Send-Q  Local Address          Foreign Address        (state)    
tcp6       0      0  fd54:20a4:d33b:b.51366 2606:4700:4400::.https ESTABLISHED
tcp6       0      0  fe80::aede:48ff:.49156 fe80::aede:48ff:.52032 ESTABLISHED
tcp6       0      0  fe80::aede:48ff:.49152 fe80::aede:48ff:.61000 ESTABLISHED
udp46      0      0  *.*                    *.*                               
udp6       0      0  *.mdns                 *.*                               
Active Multipath Internet connections
Proto/ID  Flags      Local Address          Foreign Address        (state)    

NETSTAT -F INET
Active Internet connections
Proto Recv-Q Send-Q  Local Address          Foreign Address        (state)    
tcp4       0      0  10.1.27.2.51101        17.57.144.10.5223      ESTABLISHED
udp4       0      0  10.1.27.2.59376        104.18.141.67.https               
udp4       0      0  192.168.1.174.ipsec-ms unn-143-244-44-1.ipsec         
--------------------
#NETTOP M ROUTE IPV6 CONNECTION

255.255.255.255 -> ipsec0                                                                  0 B             0 B       0 B       0 B       0 B           0
255.255.255.255 -> en0                                                                     0 B             0 B       0 B       0 B       0 B           0
localhost -> lo0                                                                           0 B             0 B       0 B       0 B       0 B           0
fd54:20a4:d33b:b10c::/64 -> ipsec0 -> fe80::aede:48ff:fe00:1122%ipsec0                   481 B           659 B       0 B       0 B       0 B           0
  fd54:20a4:d33b:b10c:8a6:0:1:1                                                          481 B           659 B       0 B       0 B       0 B           0
fd54:20a4:d33b:b10c:8a6:0:1:1a00 -> lo0                                                    0 B             0 B       0 B       0 B       0 B           0
fe80::/64 -> lo0 -> fe80::1%lo0                                                            0 B             0 B       0 B       0 B       0 B           0
localhost -> lo0                                                                          22 KiB           0 B       0 B       0 B       0 B           0


 fd addresses are Unique Local Addresses. More common are Link Local Addresses which start with fe80

--------------------
#IFCONFIG (VPN CONNECTED) LO0 / IPSEC0

lo0: flags=8049<UP,LOOPBACK,RUNNING,MULTICAST> mtu 16384
	options=1203<RXCSUM,TXCSUM,TXSTATUS,SW_TIMESTAMP>
	inet 127.0.0.1 netmask 0xff000000 
	inet6 ::1 prefixlen 128 
	inet6 fe80::1%lo0 prefixlen 64 scopeid 0x1 
	nd6 options=201<PERFORMNUD,DAD>


ipsec0: flags=8051<UP,POINTOPOINT,RUNNING,MULTICAST> mtu 1280
	options=6463<RXCSUM,TXCSUM,TSO4,TSO6,CHANNEL_IO,PARTIAL_CSUM,ZEROINVERT_CSUM>
	inet 10.1.27.2 --> 10.1.27.2 netmask 0xffff0000 
	inet6 fe80::aede:48ff:fe00:1122%ipsec0 prefixlen 64 scopeid 0x10 
	inet6 fd54:20a4:d33b:b10c:8a6:0:1:1a00 prefixlen 64 
	nd6 options=201<PERFORMNUD,DAD>

---
example: (https://knowledgebase.paloaltonetworks.com/KCSArticleDetail?id=kA14u000000HCAOCA4&lang=en_US%E2%80%A9)
A "utun" is a virtual interface created by an application on macOS endpoints to interact with the system. 

Ifconfig output while using system extensions)
utun2: flags=8051<UP,POINTOPOINT,RUNNING,MULTICAST> mtu 1400
    inet 10.85.40.243 --> 10.85.40.243 netmask 0xffffffff 
    inet6 fe80::3af9:d3ff:fe69:1732%utun2 prefixlen 64 scopeid 0xd 
    inet6 fdae:f7e0:9f37:64::146 prefixlen 128 
    nd6 options=201<PERFORMNUD,DAD>
 [ Note: Please be aware that any installed application has the ability to create a utun interface on your macOS device and this is not isolated to the GlobalProtect product. ]
 

--------------------
#ROUTE MONITOR

route monitor (on vpn enablement): 
    RTM_NEWADDR: address being added to iface: len 96, metric 0, flags:<CLONING>
sockaddrs: <NETMASK,IFP,IFA,BRD>
 ffff:ffff:ffff:ffff:: ipsec0 fd54:20a4:d33b:b10c:8a6:0:1:1a00 default

got message of size 140 on Fri Apr  5 19:12:30 2024
RTM_ADD: Add Route: len 140, pid: 0, seq 0, errno 0, flags:<UP,HOST,LLINFO,LOCAL>
locks:  inits: 
sockaddrs: <DST,GATEWAY>
 fd54:20a4:d33b:b10c:8a6:0:1:1a00 



--------------------

[ Note: sudo route -n flush : will delete all routes but will add back inet6 address. Need to run: sudo route -n flush -inet6 ] 

route monitor after  sudo route -n flush -inet6:

 got message of size 200 on Fri Apr  5 19:21:22 2024
RTM_DELETE: Delete Route: len 200, pid: 4334, seq 0, errno 0, flags:<GATEWAY,DONE,PRCLONING,CONDEMNED,GLOBAL>
locks:  inits: 
sockaddrs: <DST,GATEWAY,NETMASK,IFP,IFA>
 :: fd54:20a4:d33b:b10c:: default ipsec0 fe80::aede:48ff:fe00:1122%ipsec0

got message of size 224 on Fri Apr  5 19:21:22 2024
RTM_DELETE: Delete Route: len 224, pid: 4334, seq 1, errno 0, ifscope 13, flags:<GATEWAY,DONE,PRCLONING,IFSCOPE,CONDEMNED,GLOBAL>
locks:  inits: 
sockaddrs: <DST,GATEWAY,NETMASK,IFP,IFA>
 :: fe80::%utun0 (255) ffff ffff ffff 0 0 0 0 0 0 0 0 0 0 utun0 fe80::d4fa:100c:1905:b9d8%utun0

got message of size 224 on Fri Apr  5 19:21:22 2024
RTM_DELETE: Delete Route: len 224, pid: 4334, seq 2, errno 0, ifscope 14, flags:<GATEWAY,DONE,PRCLONING,IFSCOPE,CONDEMNED,GLOBAL>
locks:  inits: 
sockaddrs: <DST,GATEWAY,NETMASK,IFP,IFA>
 :: fe80::%utun1 (255) ffff ffff ffff 0 0 0 0 0 0 0 0 0 0 utun1 fe80::5861:983a:e547:bb2a%utun1

got message of size 224 on Fri Apr  5 19:21:22 2024
RTM_DELETE: Delete Route: len 224, pid: 4334, seq 3, errno 0, ifscope 15, flags:<GATEWAY,DONE,PRCLONING,IFSCOPE,CONDEMNED,GLOBAL>
locks:  inits: 
sockaddrs: <DST,GATEWAY,NETMASK,IFP,IFA>
 :: fe80::%utun2 (255) ffff ffff ffff 0 0 0 0 0 0 0 0 0 0 utun2 fe80::ce81:b1c:bd2c:69e%utun2

got message of size 176 on Fri Apr  5 19:21:22 2024
RTM_DELETE: Delete Route: len 176, pid: 4334, seq 4, errno 3, ifscope 16, flags:<UP,GATEWAY,HOST,WASCLONED,PROTO3,IFSCOPE,GLOBAL>
locks:  inits: 
sockaddrs: <DST,GATEWAY,IFA>
 2001:4860:4860::8888 fd54:20a4:d33b:b10c:: fe80::aede:48ff:fe00:1122%ipsec0

--- 
#2

got message of size 200 on Fri Apr  5 23:26:52 2024
RTM_DELETE: Delete Route: len 200, pid: 7177, seq 0, errno 0, flags:<GATEWAY,DONE,PRCLONING,CONDEMNED,GLOBAL>
locks:  inits: 
sockaddrs: <DST,GATEWAY,NETMASK,IFP,IFA>
 :: fd54:20a4:d33b:b10c:: default ipsec0 fe80::aede:48ff:fe00:1122%ipsec0

got message of size 176 on Fri Apr  5 23:26:52 2024
RTM_DELETE: Delete Route: len 176, pid: 7177, seq 1, errno 3, ifscope 16, flags:<UP,GATEWAY,HOST,WASCLONED,PROTO3,IFSCOPE,GLOBAL>
locks:  inits: 
sockaddrs: <DST,GATEWAY,IFA>
 2001:4860:4860::8888 fd54:20a4:d33b:b10c:: fe80::aede:48ff:fe00:1122%ipsec0

-------
#ROUTE FLUSH INET6

route -n flush -inet6 (vpn enabled):
::                   fd54:20a4:d33b:b10c: done
::                   fe80::%utun0         done
::                   fe80::%utun1         done
::                   fe80::%utun2         done
route: write to routing socket: No such process
got only -1 for rlen


-------
#NETTOP -M ROUTE AFTER FLUSH INET6

nettop -m route [after sudo route -n flush -inet6]: 

default -> ipsec0                                                                       4054 KiB         480 KiB  7027 B       0 B    4770 B
  10.1.0.1                                                                                84 KiB          41 KiB     0 B       0 B       0 B
  server-13-225-142-113.lax3.r.cloudfront.net                                            700 KiB          27 KiB  2469 B       0 B       0 B
  17.57.144.90                                                                          9088 B         10089 B       0 B       0 B       0 B
  ec2-34-208-1-255.us-west-2.compute.amazonaws.com                                      8871 B          4721 B       0 B       0 B       0 B
  104.21.20.191                                                                           25 KiB          21 KiB     0 B       0 B     479 B
  146.75.106.214                                                                         151 KiB          46 KiB  1220 B       0 B     128 B
  185.159.159.148                                                                      10185 B          2241 B       0 B       0 B       0 B
  stackoverflow.com                                                                     7605 B          6346 B       0 B       0 B       0 B
default -> en0 -> 192.168.1.1                                                            252 B             0 B       0 B       0 B       0 B
10.1.131.102 -> ipsec0                                                                     0 B             0 B       0 B       0 B       0 B
unn-37-19-221-197.datapacket.com -> en0                                                 5883 KiB        1018 KiB     0 B       0 B       0 B
127/8 -> lo0 -> 127.0.0.1                                                                  0 B             0 B       0 B       0 B       0 B
localhost -> lo0                                                                         123 KiB          98 KiB     0 B       0 B       0 B
169.254/16 -> en0                                                                          0 B             0 B       0 B       0 B       0 B
192.168.1/24 -> en0                                                                      173 KiB         929 B    1448 B       0 B       0 B
192.168.1.1 -> en0                                                                        91 KiB          47 KiB     0 B       0 B       0 B
  192.168.1.1 -> ac:b6:87:32:c0:c0                                                        86 KiB          45 KiB     0 B       0 B       0 B
192.168.1.174 -> en0                                                                      12 KiB           0 B       0 B       0 B       0 B
224/4 -> ipsec0                                                                            0 B          1685 B       0 B       0 B       0 B
224/4 -> en0                                                                               0 B             0 B       0 B       0 B       0 B
255.255.255.255 -> ipsec0                                                                  0 B             0 B       0 B       0 B       0 B
255.255.255.255 -> en0                                                                     0 B             0 B       0 B       0 B       0 B
localhost -> lo0                                                                           0 B             0 B       0 B       0 B       0 B
fd54:20a4:d33b:b10c::/64 -> ipsec0 -> fe80::aede:48ff:fe00:1122%ipsec0                     0 B             0 B       0 B       0 B       0 B
fd54:20a4:d33b:b10c:8bb:0:1:8257 -> lo0                                                    0 B             0 B       0 B       0 B       0 B
fe80::/64 -> lo0 -> fe80::1%lo0                                                            0 B             0 B       0 B       0 B       0 B
localhost -> lo0                                                                          27 KiB           0 B       0 B       0 B       0 B
fe80::/64 -> en5                                                                         341 KiB         373 KiB     0 B       0 B       0 B
  fe80::aede:48ff:fe33:4455%en5 -> ac:de:48:33:44:55                                     341 KiB         373 KiB     0 B       0 B       0 B
fe80::aede:48ff:fe00:1122%en5 -> lo0                                                       0 B             0 B       0 B       0 B       0 B
fe80::/64 -> utun0 -> fe80::d4fa:100c:1905:b9d8%utun0                                      0 B             0 B       0 B       0 B       0 B
fe80::d4fa:100c:1905:b9d8%utun0 -> lo0                                                     0 B             0 B       0 B       0 B       0 B
fe80::/64 -> utun1 -> fe80::5861:983a:e547:bb2a%utun1                                      0 B             0 B       0 B       0 B       0 B
fe80::5861:983a:e547:bb2a%utun1 -> lo0                                                     0 B             0 B       0 B       0 B       0 B
fe80::/64 -> utun2 -> fe80::ce81:b1c:bd2c:69e%utun2                                        0 B             0 B       0 B       0 B       0 B
fe80::ce81:b1c:bd2c:69e%utun2 -> lo0                                                       0 B             0 B       0 B       0 B       0 B
fe80::/64 -> ipsec0 -> fe80::aede:48ff:fe00:1122%ipsec0                                    0 B             0 B       0 B       0 B       0 B
fe80::aede:48ff:fe00:1122%ipsec0 -> lo0                                                    0 B             0 B       0 B       0 B       0 B
ff00::/8 -> lo0 -> ::1                                                                     0 B             0 B       0 B       0 B       0 B
ff00::/8 -> en5                                                                            0 B             0 B       0 B       0 B       0 B
ff00::/8 -> utun0 -> fe80::d4fa:100c:1905:b9d8%utun0                                       0 B             0 B       0 B       0 B       0 B
ff00::/8 -> utun1 -> fe80::5861:983a:e547:bb2a%utun1                                       0 B             0 B       0 B       0 B       0 B
ff00::/8 -> utun2 -> fe80::ce81:b1c:bd2c:69e%utun2                                         0 B             0 B       0 B       0 B       0 B
ff00::/8 -> ipsec0 -> fe80::aede:48ff:fe00:1122%ipsec0                                     0 B             0 B       0 B       0 B       0 B
ff01::/32 -> lo0 -> ::1                                                                    0 B             0 B       0 B       0 B       0 B
ff01::/32 -> en5                                                                           0 B             0 B       0 B       0 B       0 B
ff01::/32 -> utun0 -> fe80::d4fa:100c:1905:b9d8%utun0                                      0 B             0 B       0 B       0 B       0 B
ff01::/32 -> utun1 -> fe80::5861:983a:e547:bb2a%utun1                                      0 B             0 B       0 B       0 B       0 B
ff01::/32 -> utun2 -> fe80::ce81:b1c:bd2c:69e%utun2                                        0 B             0 B       0 B       0 B       0 B
ff01::/32 -> ipsec0 -> fe80::aede:48ff:fe00:1122%ipsec0                                    0 B             0 B       0 B       0 B       0 B
ff02::/32 -> lo0 -> ::1                                                                    0 B             0 B       0 B       0 B       0 B
ff02::/32 -> en5                                                                           0 B             0 B       0 B       0 B       0 B
ff02::/32 -> utun0 -> fe80::d4fa:100c:1905:b9d8%utun0                                      0 B             0 B       0 B       0 B       0 B
ff02::/32 -> utun1 -> fe80::5861:983a:e547:bb2a%utun1                                      0 B             0 B       0 B       0 B       0 B
ff02::/32 -> utun2 -> fe80::ce81:b1c:bd2c:69e%utun2                                        0 B             0 B       0 B       0 B       0 B
ff02::/32 -> ipsec0 -> fe80::aede:48ff:fe00:1122%ipsec0                                    0 B             0 B       0 B       0 B       0 B




-------
#NETTOP -M ROUTE BEFORE FLUSH INET6 OR NORMAL FLUSH 

nettop -m route (without route flush inet6, or normal route flush):

default -> ipsec0                                                                        649 KiB         135 KiB   714 B    3449 B       0 B          37
  10.1.0.1                                                                                39 KiB          18 KiB     0 B       0 B       0 B           0
  17.57.144.10                                                                          9854 B          8666 B       0 B       0 B       0 B           2
  17.188.143.187                                                                        8884 B          1508 B       0 B       0 B       0 B           1
  usnyc3-ntp-002.aaplimg.com                                                              76 B            76 B       0 B       0 B       0 B           0
  usqas2-ntp-002.aaplimg.com                                                             152 B           152 B       0 B       0 B       0 B           0
  server-18-173-132-80.jfk52.r.cloudfront.net                                            285 KiB          11 KiB     0 B    3449 B       0 B           1
  server-18-238-55-23.jfk52.r.cloudfront.net                                               0 B             0 B       0 B       0 B       0 B           0
  server-18-238-55-48.jfk52.r.cloudfront.net                                               0 B             0 B       0 B       0 B       0 B           0
  server-18-238-55-74.jfk52.r.cloudfront.net                                               0 B             0 B       0 B       0 B       0 B           0
  server-18-238-55-109.jfk52.r.cloudfront.net                                              0 B             0 B       0 B       0 B       0 B           0
  ec2-34-218-171-232.us-west-2.compute.amazonaws.com                                    5769 B          3193 B       0 B       0 B       0 B           1
  ec2-35-167-22-52.us-west-2.compute.amazonaws.com                                        23 KiB          15 KiB   156 B       0 B       0 B           6
  ec2-44-237-123-146.us-west-2.compute.amazonaws.com                                    5745 B          3109 B       0 B       0 B       0 B           1
  ec2-52-89-98-151.us-west-2.compute.amazonaws.com                                        12 KiB        7594 B     108 B       0 B       0 B           3
  52.149.246.39                                                                         7173 B          3289 B       0 B       0 B       0 B           1
  ec2-54-200-49-94.us-west-2.compute.amazonaws.com                                        14 KiB        5397 B       0 B       0 B       0 B           2
  104.21.20.191                                                                         1504 B           919 B       0 B       0 B       0 B           2
  104.21.30.14                                                                           130 KiB          12 KiB     0 B       0 B       0 B           0
  151.101.2.137                                                                            0 B             0 B       0 B       0 B       0 B           0
  151.101.66.137                                                                           0 B             0 B       0 B       0 B       0 B           0
  151.101.130.137                                                                          0 B             0 B       0 B       0 B       0 B           0
  151.101.194.137                                                                          0 B             0 B       0 B       0 B       0 B           0
  151.101.209.229                                                                         12 KiB        2403 B       0 B       0 B       0 B           0
  151.101.210.214                                                                         18 KiB        7051 B       0 B       0 B       0 B           1
  172.67.150.50                                                                            0 B             0 B       0 B       0 B       0 B           0
  172.67.194.22                                                                           11 KiB       10000 B       0 B       0 B       0 B          12
  185.159.159.1                                                                          574 B           907 B       0 B       0 B       0 B           1
  185.159.159.148                                                                       9088 B          2314 B     389 B       0 B       0 B           1
  192.229.211.108                                                                       1868 B          1238 B       0 B       0 B       0 B           1
  stackoverflow.com                                                                     7757 B          6839 B      61 B       0 B       0 B           0
  199.232.38.214                                                                          41 KiB          13 KiB     0 B       0 B       0 B           1
default -> en0 -> 192.168.1.1                                                            200 B           180 B       0 B       0 B       0 B           0
  17.57.147.4                                                                            128 B           180 B       0 B       0 B       0 B           0
10.1.27.2 -> ipsec0                                                                        0 B             0 B       0 B       0 B       0 B           0
127/8 -> lo0 -> 127.0.0.1                                                                  0 B             0 B       0 B       0 B       0 B           0
localhost -> lo0                                                                         119 KiB          97 KiB     0 B       0 B       0 B          74
unn-143-244-44-166.datapacket.com -> en0                                                1037 KiB         273 KiB     0 B       0 B       0 B           0
169.254/16 -> en0                                                                          0 B             0 B       0 B       0 B       0 B           0
192.168.1/24 -> en0                                                                       13 KiB         929 B    1448 B       0 B       0 B           2
  192.168.1.188 -> 3a:a7:9e:57:2a:fe                                                     120 B             0 B       0 B       0 B       0 B           0
192.168.1.1 -> en0                                                                        85 KiB          44 KiB     0 B       0 B       0 B           0
  192.168.1.1 -> ac:b6:87:32:c0:c0                                                        80 KiB          42 KiB     0 B       0 B       0 B           0
192.168.1.174 -> en0                                                                    8673 B             0 B       0 B       0 B       0 B           0
224/4 -> ipsec0                                                                            0 B          1133 B       0 B       0 B       0 B           0
  mdns.mcast.net                                                                           0 B          1133 B       0 B       0 B       0 B           0
224/4 -> en0                                                                               0 B             0 B       0 B       0 B       0 B           0
255.255.255.255 -> ipsec0                                                                  0 B             0 B       0 B       0 B       0 B           0
255.255.255.255 -> en0                                                                     0 B             0 B       0 B       0 B       0 B           0
default -> ipsec0 -> fd54:20a4:d33b:b10c::                                               240 KiB          20 KiB     0 B       0 B       0 B           7
  dns.google                                                                               0 B             0 B       0 B       0 B       0 B           0
  g2600-141b-5000-0589-0000-0000-0000-02a1.deploy.static.akam                             37 KiB        3754 B       0 B       0 B       0 B           1
  g2600-141b-5000-0593-0000-0000-0000-02a1.deploy.static.akam                           7659 B          1309 B       0 B       0 B       0 B           1
  g2600-141b-5000-0595-0000-0000-0000-02a1.deploy.static.akam                              0 B             0 B       0 B       0 B       0 B           0
  g2600-141b-5000-05a5-0000-0000-0000-02a1.deploy.static.akam                              0 B             0 B       0 B       0 B       0 B           0
  g2600-141b-5000-05ab-0000-0000-0000-02a1.deploy.static.akam                              0 B             0 B       0 B       0 B       0 B           0
  g2600-141b-5000-0689-0000-0000-0000-1aca.deploy.static.akam                              0 B             0 B       0 B       0 B       0 B           0
  g2600-141b-5000-06a4-0000-0000-0000-1aca.deploy.static.akam                              0 B             0 B       0 B       0 B       0 B           0
  2600:9000:211c:1200:15:85fe:56c0:93a1                                                    0 B             0 B       0 B       0 B       0 B           0
  2600:9000:211c:1400:15:85fe:56c0:93a1                                                    0 B             0 B       0 B       0 B       0 B           0
  2600:9000:211c:1600:15:85fe:56c0:93a1                                                    0 B             0 B       0 B       0 B       0 B           0
  2600:9000:211c:3600:15:85fe:56c0:93a1                                                    0 B             0 B       0 B       0 B       0 B           0
  2600:9000:211c:5e00:15:85fe:56c0:93a1                                                    0 B             0 B       0 B       0 B       0 B           0
2600:9000:211c:f600:15:85fe:56c0:93a1                                                   13 KiB        2604 B       0 B       0 B       0 B           1
  2606:4700:3030::ac43:9632                                                                0 B             0 B       0 B       0 B       0 B           0
  2606:4700:3033::6815:14bf                                                                0 B             0 B       0 B       0 B       0 B           0
  2606:4700:3034::ac43:c216                                                                0 B             0 B       0 B       0 B       0 B           0
  2606:4700:3035::6815:1e0e                                                                0 B             0 B       0 B       0 B       0 B           0
  2620:149:149:1022::21                                                                    0 B             0 B       0 B       0 B       0 B           0
  2620:149:149:1028::a                                                                     0 B             0 B       0 B       0 B       0 B           0
  2620:149:149:1029::8                                                                     0 B             0 B       0 B       0 B       0 B           0
  2620:149:149:102a::7                                                                     0 B             0 B       0 B       0 B       0 B           0
  2620:149:149:102d::a                                                                     0 B             0 B       0 B       0 B       0 B           0
  2620:149:149:1030::a                                                                     0 B             0 B       0 B       0 B       0 B           0
  2620:149:149:1031::8                                                                     0 B             0 B       0 B       0 B       0 B           0
  2620:149:149:1031::a                                                                     0 B             0 B       0 B       0 B       0 B           0
  usqas2-ntp-002.aaplimg.com                                                               0 B             0 B       0 B       0 B       0 B           0
  usnyc3-ntp-001.aaplimg.com                                                               0 B             0 B       0 B       0 B       0 B           0
  usnyc3-ntp-004.aaplimg.com                                                               0 B             0 B       0 B       0 B       0 B           0
  2620:149:a41:280::2:1                                                                    0 B             0 B       0 B       0 B       0 B           0
  2620:149:a41:280::2:2                                                                    0 B             0 B       0 B       0 B       0 B           0
  2620:149:a41:280::2:4                                                                    0 B             0 B       0 B       0 B       0 B           0
  2620:149:a41:280::2:5                                                                    0 B             0 B       0 B       0 B       0 B           0
  2620:149:a41:280::2:6                                                                    0 B             0 B       0 B       0 B       0 B           0
  2a04:4e42::649                                                                         114 KiB        7141 B       0 B       0 B       0 B           2
  2a04:4e42:46::485                                                                       69 KiB        6217 B       0 B       0 B       0 B           2
  2a04:4e42:200::649                                                                       0 B             0 B       0 B       0 B       0 B           0
  2a04:4e42:400::649                                                                       0 B             0 B       0 B       0 B       0 B           0
  2a04:4e42:600::649                                                                       0 B             0 B       0 B       0 B       0 B           0
  2a07:b941:f00::1                                                                         0 B             0 B       0 B       0 B       0 B           0
  2600:9000:20ed:a600:12:bd7a:5b00:93a1                                                 3018 B          3417 B       0 B       0 B     726 B           1
  2600:9000:20ed:b800:12:bd7a:5b00:93a1                                                    0 B             0 B       0 B       0 B       0 B           0
  2600:9000:20ed:f400:12:bd7a:5b00:93a1                                                    0 B             0 B       0 B       0 B       0 B           0
  2600:9000:20ed:400:12:bd7a:5b00:93a1                                                     0 B             0 B       0 B       0 B       0 B           0
  2600:9000:20ed:2200:12:bd7a:5b00:93a1                                                    0 B             0 B       0 B       0 B       0 B           0
  2600:9000:20ed:6200:12:bd7a:5b00:93a1                                                    0 B             0 B       0 B       0 B       0 B           0
  2600:9000:20ed:9000:12:bd7a:5b00:93a1                                                    0 B             0 B       0 B       0 B       0 B           0
  2600:9000:20ed:9e00:12:bd7a:5b00:93a1                                                    0 B             0 B       0 B       0 B       0 B           0
localhost -> lo0                                                                           0 B             0 B       0 B       0 B       0 B           0
fd54:20a4:d33b:b10c::/64 -> ipsec0 -> fe80::aede:48ff:fe00:1122%ipsec0                   481 B           659 B       0 B       0 B       0 B           0
  fd54:20a4:d33b:b10c:8a6:0:1:1                                                          481 B           659 B       0 B       0 B       0 B           0
fd54:20a4:d33b:b10c:8a6:0:1:1a00 -> lo0                                                    0 B             0 B       0 B       0 B       0 B           0
fe80::/64 -> lo0 -> fe80::1%lo0                                                            0 B             0 B       0 B       0 B       0 B           0
localhost -> lo0                                                                          23 KiB           0 B       0 B       0 B       0 B           0
fe80::/64 -> en5                                                                         335 KiB         368 KiB     0 B       0 B       0 B          30
  fe80::aede:48ff:fe33:4455%en5 -> ac:de:48:33:44:55                                     335 KiB         368 KiB     0 B       0 B       0 B          30
fe80::aede:48ff:fe00:1122%en5 -> lo0                                                       0 B             0 B       0 B       0 B       0 B           0
fe80::/64 -> utun0 -> fe80::d4fa:100c:1905:b9d8%utun0                                      0 B             0 B       0 B       0 B       0 B           0
fe80::d4fa:100c:1905:b9d8%utun0 -> lo0                                                     0 B             0 B       0 B       0 B       0 B           0
fe80::/64 -> utun1 -> fe80::5861:983a:e547:bb2a%utun1                                      0 B             0 B       0 B       0 B       0 B           0
fe80::5861:983a:e547:bb2a%utun1 -> lo0                                                     0 B             0 B       0 B       0 B       0 B           0
fe80::/64 -> utun2 -> fe80::ce81:b1c:bd2c:69e%utun2                                        0 B             0 B       0 B       0 B       0 B           0
fe80::ce81:b1c:bd2c:69e%utun2 -> lo0                                                       0 B             0 B       0 B       0 B       0 B           0
fe80::/64 -> ipsec0 -> fe80::aede:48ff:fe00:1122%ipsec0                                    0 B             0 B       0 B       0 B       0 B           0
fe80::aede:48ff:fe00:1122%ipsec0 -> lo0                                                    0 B             0 B       0 B       0 B       0 B           0
ff00::/8 -> lo0 -> ::1                                                                     0 B             0 B       0 B       0 B       0 B           0
ff00::/8 -> en5                                                                            0 B             0 B       0 B       0 B       0 B           0
ff00::/8 -> utun0 -> fe80::d4fa:100c:1905:b9d8%utun0                                       0 B             0 B       0 B       0 B       0 B           0
ff00::/8 -> utun1 -> fe80::5861:983a:e547:bb2a%utun1                                       0 B             0 B       0 B       0 B       0 B           0
ff00::/8 -> utun2 -> fe80::ce81:b1c:bd2c:69e%utun2                                         0 B             0 B       0 B       0 B       0 B           0
ff00::/8 -> ipsec0 -> fe80::aede:48ff:fe00:1122%ipsec0                                     0 B             0 B       0 B       0 B       0 B           0
ff01::/32 -> lo0 -> ::1                                                                    0 B             0 B       0 B       0 B       0 B           0
ff01::/32 -> en5                                                                           0 B             0 B       0 B       0 B       0 B           0
ff01::/32 -> utun0 -> fe80::d4fa:100c:1905:b9d8%utun0                                      0 B             0 B       0 B       0 B       0 B           0
ff01::/32 -> utun1 -> fe80::5861:983a:e547:bb2a%utun1                                      0 B             0 B       0 B       0 B       0 B           0
ff01::/32 -> utun2 -> fe80::ce81:b1c:bd2c:69e%utun2                                        0 B             0 B       0 B       0 B       0 B           0
ff01::/32 -> ipsec0 -> fe80::aede:48ff:fe00:1122%ipsec0                                    0 B             0 B       0 B       0 B       0 B           0
ff02::/32 -> lo0 -> ::1                                                                    0 B             0 B       0 B       0 B       0 B           0
ff02::/32 -> en5                                                                           0 B             0 B       0 B       0 B       0 B           0
ff02::/32 -> utun0 -> fe80::d4fa:100c:1905:b9d8%utun0                                      0 B             0 B       0 B       0 B       0 B           0
ff02::/32 -> utun1 -> fe80::5861:983a:e547:bb2a%utun1                                      0 B             0 B       0 B       0 B       0 B           0
ff02::/32 -> utun2 -> fe80::ce81:b1c:bd2c:69e%utun2                                        0 B             0 B       0 B       0 B       0 B           0
ff01::/32 -> ipsec0 -> fe80::aede:48ff:fe00:1122%ipsec0                                    0 B             0 B       0 B       0 B       0 B           0
ff02::/32 -> lo0 -> ::1                                                                    0 B             0 B       0 B       0 B       0 B           0
ff02::/32 -> en5                                                                           0 B             0 B       0 B       0 B       0 B           0
ff02::/32 -> utun0 -> fe80::d4fa:100c:1905:b9d8%utun0                                      0 B             0 B       0 B       0 B       0 B           0
ff02::/32 -> utun1 -> fe80::5861:983a:e547:bb2a%utun1                                      0 B             0 B       0 B       0 B       0 B           0
ff02::/32 -> utun2 -> fe80::ce81:b1c:bd2c:69e%utun2                                        0 B             0 B       0 B       0 B       0 B           0
ff02::/32 -> ipsec0 -> fe80::aede:48ff:fe00:1122%ipsec0                                    0 B             0 B       0 B       0 B       0 B           0

                                                              0 B             0 B       0 B       0 B       0 B           0



---


$ netstat -n -s -I ipsec0

ip6 on ipsec0:

1112 total input datagrams

0 datagram with invalid header received

0 datagram exceeded MTU received

0 datagram with no route received

0 datagram with invalid dst received

0 datagram with unknown proto received

0 truncated datagram received

0 input datagram discarded

1112 datagrams delivered to an upper layer protocol

0 datagram forwarded to this interface

1029 datagrams sent from an upper layer protocol

0 total discarded output datagram

0 output datagram fragmented

0 output datagram failed on fragment

0 output datagram succeeded on fragment

0 incoming datagram fragmented

0 datagram reassembled

0 atomic fragments received

0 datagram failed on reassembling

0 multicast datagram received

7 multicast datagrams sent

0 ICMPv6 packet received for unreachable destination

0 address expiry event reported

0 prefix expiry event reported

0 default router expiry event reported

ipsec0     0     fe80::aede: fe80:10::aede:48f        0     -        0     -     -

ipsec0     0     10.1/16       10.1.118.200           0     -        0     -     -

ipsec0     0     fd54:20a4:d fd54:20a4:d33b:b1        0     -        0     -     -

icmp6 on ipsec0:

0 total input message

0 total input error message

0 input destination unreachable error

0 input administratively prohibited error

0 input time exceeded error

0 input parameter problem error

0 input packet too big error

0 input echo request

0 input echo reply

0 input router solicitation

0 input router advertisement

0 input neighbor solicitation

0 input neighbor advertisement

0 input redirect

0 input MLD query

0 input MLD report

0 input MLD done

0 total output message

0 total output error message

0 output destination unreachable error

0 output administratively prohibited error

0 output time exceeded error

0 output parameter problem error

0 output packet too big error

0 output echo request

0 output echo reply

0 output router solicitation

0 output router advertisement

0 output neighbor solicitation

0 output neighbor advertisement

0 output redirect

0 output MLD query

0 output MLD report

0 output MLD done

ipsec0     0     fe80::aede: fe80:10::aede:48f        0     -        0     -     -

ipsec0     0     10.1/16       10.1.118.200           0     -        0     -     -

ipsec0     0     fd54:20a4:d fd54:20a4:d33b:b1        0     -        0     -     -