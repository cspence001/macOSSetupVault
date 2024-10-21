


![[Screenshot 2024-05-23 at 19.08.04.png]]
vpn prompt annoyance
I set my login keychain to lock after 5 minutes of inactivity 
I keep getting prompts from ProtonVPN that ProtonVPN needs my password to access my login keychain  
I noticed after entering my password the the protonVPN "vpnCredentials" item (Kind: application password, Account: vpnCredentials, Where: ProtonVPN) in my login keychain was updated.


Wireguard through device IP [[Allow LAN (ProtonVPN)]]
ch.protonvpn.mac
ch.protonvpn.mac.WireGuard-Extension

vpn with macos firewall
vpn local enabled/disabled? 


protonvpn plist
~/Library/Preferences/ch.protonvpn.mac.plist

	AVDesktopPlaybackControlsControllerShowsDurationInsteadOfTimeRemainingDefaultsKey = 1;
    AutoConnect = 1;
    "AutoConnect_cspence001" = "st_f";
    ConnectOnDemand = 1;
    DynamicBugReport = {length = 5141, bytes = 0x7b226361 7465676f 72696573 223a5b7b ... 673f227d 5d7d5d7d };
    EarlyAccess = 0;
    FeatureFlags = {length = 72, bytes = 0x7b227365 72766572 52656672 65736822 ... 69656c64 223a317d };
    IntentionallyDisconnected = 0;
    LastAppVersion = "4.1.6";
    LastConnectionRequest = {length = 165, bytes = 0x62706c69 73743030 d5010203 04050609 ... 00000000 00000077 };
    LastIkeConnection = {length = 1981, bytes = 0x62706c69 73743030 d4010203 04050708 ... 00000000 00000747 };
    LastPreparingServer = {length = 1542, bytes = 0x7b226964 223a226a 6466575a 6d624158 ... 65223a22 5553227d };
    LastUserAccountPlancspence001 = free;
    LastWireguardConnection = {length = 1967, bytes = 0x7b227365 72766572 223a7b22 6964223a ... 332c3132 32345d7d };
    LaunchedBefore = 1;
    MaintenanceServerRefreshIntereval = 40;
    NSInitialToolTipDelay = 500;
    "NSWindow Frame Main Window" = "1330 397 363 628 0 0 1680 1025 ";
    "NSWindow Frame SUStatusFrame" = "695 857 400 135 0 0 1680 1025 ";
    "NSWindow Frame SUUpdateAlert" = "530 491 620 398 0 0 1680 1025 ";
    NetShieldcspence001 = 0;
    OpenVpnConfig = {length = 79, bytes = 0x7b226465 6661756c 74546370 506f7274 ... 342c3530 36305d7d };
    RatingSettings = {length = 183, bytes = 0x7b226461 79734c61 73745265 76696577 ... 74696f6e 223a307d };
    RememberLoginAfterUpdate = 1;
    SUFeedURL = "https://protonvpn.com/download/macos-update3.xml";
    SUHasLaunchedBefore = 1;
    SULastCheckTime = "2024-02-28 22:20:23 +0000";
    SUUpdateGroupIdentifier = 2947549416;
    SUUpdateRelaunchingMarker = 1;
    ServerChangeConfig = {length = 105, bytes = 0x7b226368 616e6765 53657276 65724c6f ... 6e647322 3a39307d };
    SmartProtocolConfig = {length = 87, bytes = 0x7b226f70 656e5650 4e223a66 616c7365 ... 53223a74 7275657d };
    StartMinimized = 0;
    StartOnBoot = 1;
    SystemNotifications = 1;
    TSKVendorIdentifier = "87CC3412-E2C8-4722-A5E1-1256E4B53F98";
    TelemetryCrashReportscspence001 = false;
    TelemetryUsageDatacspence001 = false;
    UnprotectedNetwork = 1;
    UserAccountCreationDate = 1654231240;
    UserIp = "97.146.113.252";
    UserLocation = {length = 63, bytes = 0x7b226970 223a2239 372e3134 362e3131 ... 72656c65 7373227d };
    VpnAcceleratorEnabledcspence001 = {length = 4, bytes = 0x226f6e22};
    Welcomed = 1;
    WireguardConfig = {length = 96, bytes = 0x7b226465 6661756c 74556470 506f7274 ... 223a5b34 34335d7d };
    age = "1709158831.477413";
    "announcements_cspence001" = {length = 2, bytes = 0x5b5d};
    excludeLocalNetworkscspence001 = {length = 5, bytes = 0x226f666622};
    isSubsequentLaunch = 1;
    partnerTypes = {length = 2, bytes = 0x5b5d};
    "protoncore.featureflag" = {length = 2244, bytes = 0x62706c69 73743030 d2010203 7d505f10 ... 00000000 000006c4 };
    "protoncore.featureflag.userId" = "QuPF_YmhsYN15pE1zQh91uXYqOG6LtHNbSQOJgT1hN_oU3eKjMT_34W15eVFoDyUUvOWMIHXpfe7Idv0hr-ldg==";
    serverCacheVersion = 1;
    servers = {length = 3861944, bytes = 0x62706c69 73743030 d4000100 02000300 ... 00000000 00373060 };
    smartProtocol = 1;
    streamingResourcesUrl = "https://protonvpn.com/download/resources/";
    streamingServices = {length = 4848, bytes = 0x7b224252 223a7b22 32223a5b 7b226e61 ... 706e6722 7d5d7d7d };
    userRole = {length = 1, bytes = 0x30};



**IPV6 and LAN:** I'm using the free version of Proton VPN on my macOS (IKEv2) and since the last couple of updates (the last month) I started noticing a ipv6 address being used after I've disabled it system wide (networksetup -setv6off Wi-Fi). My solution to this thus far is flushing the inet6 routes after the vpn is activated (route -n flush -inet6) but I don't see why this system setting is being overridden in this way.

Also curious if this has anything to do with my inability to toggle "Allow LAN connections" in the free version, which has been a recent change within the last 2 months.

**Keychain Prompt:**
I started getting prompts (last 2 months) that ProtonVPN needs my password to access my login keychain  (1-2x/day). At one point I set my login keychain to lock after 5 minutes of inactivity and then I started getting these every 5 minutes. I've already contacted support about this and their solution was to just not lock my keychain. This doesn't seem like a secure solution..

* I noticed after entering my password the the protonVPN "vpnCredentials" item in my login keychain was updated.
* ProtonVPN Root CA exists in my login keychain but displays that the root certificate is not trusted.

Curious if anyone else gets these prompts and/or has their ProtonVPN Root CA appearing as such?

**Firewall:**
Does this affect my VPN connection? I have the following firewall settings in the native system settings:
EnableFirewall true
EnableStealthMode true
AllowSignedApp false
AllowSigned false
BlockAllIncoming true

**Other settings:**

awdl0 -off

**etc/hosts:**
[127.0.0.1](http://127.0.0.1/) `localhost`
[255.255.255.255](http://255.255.255.255/) `broadcasthost`
`::1 localhost`
`fe80::1%lo0 localhost`

**mDNSResponder:**

* NoMulticastAdvertisements true

**Wi-Fi:**

* RequireAdminForIBSS true
* RequireAdminToTurnAirPortOnOff true
* RequireAdminForAirPortNetworkChange true

**Asset Caching:**

* AllowPersonalCaching false
* AllowSharedCaching false

**Network Browser Settings:**

* BrowseAllInterfaces false
* EnableODiskBrowsing false
* ODSSupported false

Any information appreciated.