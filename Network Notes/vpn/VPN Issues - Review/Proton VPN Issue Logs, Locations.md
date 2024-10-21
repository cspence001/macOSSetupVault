

#### protonvpn container caches
/Users/meuthu/Library/Containers/ch.protonvpn.mac/Data/Library/Caches/ch.protonvpn.mac/*
/Users/meuthu/Library/Containers/ch.protonvpn.mac/Data/Library/Caches/ch.protonvpn.mac/fsCachedData 
/Users/meuthu/Library/Containers/ch.protonvpn.mac/Data/Library/Application Support/database.sqlite
/Users/meuthu/Library/Containers/ch.protonvpn.mac/Data/Library/HTTPStorages/ch.protonvpn.mac/
/Users/meuthu/Library/Containers/ch.protonvpn.mac/Data/Library/Cookies/Cookies.binarycookies 

### protonvpn logs
/Users/meuthu/Library/Containers/ch.protonvpn.mac/Data/Library/Logs/

### review helpd after startup 
~/Library/Caches/com.apple.helpd/

defr /Library/SystemExtensions/ADCF9C5F-4552-4CC1-B5E5-A2F6F693598E/ch.protonvpn.mac.WireGuard-Extension.systemextension/Contents/Resources/LocalFeatureFlags_LocalFeatureFlags.bundle/Contents/Resources/LocalFeatureFlags.plist 

defr /Library/SystemExtensions/ADCF9C5F-4552-4CC1-B5E5-A2F6F693598E/ch.protonvpn.mac.WireGuard-Extension.systemextension/Contents/Info.plist 
{
    AppIdentifierPrefix = "J6S6Q257EK.";
    BuildMachineOSBuild = 22G513;
    CFBundleDevelopmentRegion = en;
    CFBundleDisplayName = "ProtonVPN WireGuard";
    CFBundleExecutable = "ch.protonvpn.mac.WireGuard-Extension";
    CFBundleIdentifier = "ch.protonvpn.mac.WireGuard-Extension";
    CFBundleInfoDictionaryVersion = "6.0";
    CFBundleName = "ch.protonvpn.mac.WireGuard-Extension";
    CFBundlePackageType = SYSX;
    CFBundleShortVersionString = "4.3.0";
    CFBundleSupportedPlatforms =     (
        MacOSX
    );
    CFBundleVersion = 2404171112;
    NSSystemExtensionUsageDescription = "";
    NetworkExtension =     {
        NEMachServiceName = "J6S6Q257EK.group.ch.protonvpn.mac.WireGuard-Extension";
        NEProviderClasses =         {
            "com.apple.networkextension.packet-tunnel" = "ch_protonvpn_mac_WireGuard_Extension.PacketTunnelProvider";
        };
    };
    TeamIdentifierPrefix = "J6S6Q257EK.";
}

---
cat /Library/SystemExtensions/7D0C8F5E-E49D-4924-9BF-16DDA98511C3/ch.protonvpn.mac.OpenVPN-Extension.systemextension/Contents/MacOS/ch.protonvpn.mac.OpenVPN-Extension 

<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>com.apple.application-identifier</key>
	<string>J6S6Q257EK.ch.protonvpn.mac.OpenVPN-Extension</string>
	<key>com.apple.developer.networking.networkextension</key>
	<array>
		<string>packet-tunnel-provider-systemextension</string>
	</array>
	<key>com.apple.developer.team-identifier</key>
	<string>J6S6Q257EK</string>
	<key>com.apple.security.app-sandbox</key>
	<true/>
	<key>com.apple.security.application-groups</key>
	<array>
		<string>J6S6Q257EK.group.ch.protonvpn.mac</string>
	</array>
	<key>com.apple.security.network.client</key>
	<true/>
	<key>com.apple.security.network.server</key>
	<true/>
	<key>com.apple.security.temporary-exception.files.absolute-path.read-write</key>
	<array>
		<string>/Library/Keychains/</string>
		<string>/private/var/db/mds/system/</string>
	</array>
	<key>keychain-access-groups</key>
	<array>
		<string>J6S6Q257EK.ch.protonvpn.macos</string>
	</array>
</dict>
</plist>

---
cat /Library/SystemExtensions/7D0C8F5E-E49D-4924-89BF-16DDA98511C3/ch.protonvpn.mac.OpenVPN-Extension.systemextension/Contents/_CodeSignature/CodeResources 

(FrameworksøSharedFrameworksøPlugInsøPlug-insøXPCServicesøHelpersøMacOSøLibrary/(AutomatorøSpotlightøLoginItems))/</key>

##### Uninstall 

```sh
systemextensionsctl list 
systemextensionsctl uninstall J6S6Q257EK ch.protonvpn.mac.WireGuard-Extension
systemextensionsctl uninstall J6S6Q257EK ch.protonvpn.mac.OpenVPN-Extension

At this time, this tool cannot be used if System Integrity Protection is enabled.
This limitation will be removed in the near future.
Please remember to re-enable System Integrity Protection!
```

`/Library/SystemExtensions/db.plist`
```sh
{
    bootUUID = "7871ABF2-F319-49C9-B9FD-884E994F3DF5";
    developerMode = 0;
    extensionPolicies =     (
    );
    extensions =     (
                {
            additionalLaunchdPlistEntries = {length = 821, bytes = 0x62706c69 73743030 d4010203 04050607 ... 00000000 0000027d };
            bundleVersion =             {
                CFBundleShortVersionString = "4.3.0";
                CFBundleVersion = 2404171112;
            };
            categories =             (
                "com.apple.system_extension.network_extension"
            );
            container =             {
                bundlePath = "/Applications/ProtonVPN.app";
            };
            identifier = "ch.protonvpn.mac.OpenVPN-Extension";
            originPath = "/Applications/ProtonVPN.app/Contents/Library/SystemExtensions/ch.protonvpn.mac.OpenVPN-Extension.systemextension";
            references =             (
            );
            stagedBundleURL =             {
                relative = "file:///Library/SystemExtensions/50FECB81-5E79-44EF-B57F-225878395F85/ch.protonvpn.mac.OpenVPN-Extension.systemextension/";
            };
            stagedCdhashes =             {
                316c1f0ade134eeed65bc026c7e449c7658fc382 =                 {
                    cpusubtype = 3;
                    cputype = 16777223;
                };
                c6210f78b1d3e33b21142dd81367a6502b0f60f0 =                 {
                    cpusubtype = 0;
                    cputype = 16777228;
                };
            };
            state = "activated_enabled";
            teamID = J6S6Q257EK;
            uniqueID = "50FECB81-5E79-44EF-B57F-225878395F85";
        },
                {
            additionalLaunchdPlistEntries = {length = 825, bytes = 0x62706c69 73743030 d4010203 04050607 ... 00000000 00000281 };
            bundleVersion =             {
                CFBundleShortVersionString = "5.5";
                CFBundleVersion = 6273;
            };
            categories =             (
                "com.apple.system_extension.network_extension"
            );
            container = "$null";
            identifier = "at.obdev.littlesnitch.networkextension";
            originPath = "/Applications/Little Snitch.app/Contents/Library/SystemExtensions/at.obdev.littlesnitch.networkextension.systemextension";
            references =             (
            );
            stagedBundleURL =             {
                relative = "file:///Library/SystemExtensions/33593376-322C-47B5-BF99-9DE1ACE68DAD/at.obdev.littlesnitch.networkextension.systemextension/";
            };
            stagedCdhashes =             {
                191878b25be9436ebfc3e14648f4584901eab657 =                 {
                    cpusubtype = 0;
                    cputype = 16777228;
                };
                27d374587e4c0ea1103d1b80b305751568ebae2b =                 {
                    cpusubtype = 3;
                    cputype = 16777223;
                };
            };
            state = "activated_enabled";
            teamID = MLZF7K7B5R;
            uniqueID = "33593376-322C-47B5-BF99-9DE1ACE68DAD";
        },
                {
            additionalLaunchdPlistEntries = {length = 827, bytes = 0x62706c69 73743030 d4010203 04050607 ... 00000000 00000283 };
            bundleVersion =             {
                CFBundleShortVersionString = "4.4.0";
                CFBundleVersion = 2408011056;
            };
            categories =             (
                "com.apple.system_extension.network_extension"
            );
            container =             {
                bundlePath = "/Applications/ProtonVPN.app";
            };
            identifier = "ch.protonvpn.mac.WireGuard-Extension";
            originPath = "/Applications/ProtonVPN.app/Contents/Library/SystemExtensions/ch.protonvpn.mac.WireGuard-Extension.systemextension";
            references =             (
                                {
                    appIdentifier = "ch.protonvpn.mac";
                    appRef = "file:///.file/id=6571367.35902053/";
                    teamID = J6S6Q257EK;
                }
            );
            stagedBundleURL =             {
                relative = "file:///Library/SystemExtensions/40710991-59D6-454F-9949-F1E69E8241BC/ch.protonvpn.mac.WireGuard-Extension.systemextension/";
            };
            stagedCdhashes =             {
                855aab2ef2dc8b23aa1aa799ac73063ce8345b6d =                 {
                    cpusubtype = 3;
                    cputype = 16777223;
                };
                bd262b3db3290ba5f7b8f04080ae9ce67d83b566 =                 {
                    cpusubtype = 0;
                    cputype = 16777228;
                };
            };
            state = "activated_enabled";
            teamID = J6S6Q257EK;
            uniqueID = "40710991-59D6-454F-9949-F1E69E8241BC";
        },
                {
            additionalLaunchdPlistEntries = {length = 754, bytes = 0x62706c69 73743030 d4010203 04050607 ... 00000000 00000250 };
            bundleVersion =             {
                CFBundleShortVersionString = "30.0.2";
                CFBundleVersion = 7160081382;
            };
            categories =             (
                "com.apple.system_extension.cmio"
            );
            container =             {
                bundlePath = "/Applications/OBS.app";
            };
            identifier = "com.obsproject.obs-studio.mac-camera-extension";
            originPath = "/Applications/OBS.app/Contents/Library/SystemExtensions/com.obsproject.obs-studio.mac-camera-extension.systemextension";
            references =             (
                                {
                    appIdentifier = "com.obsproject.obs-studio";
                    appRef = "file:///.file/id=6571367.22154169/";
                    teamID = 2MMRE5MTB8;
                }
            );
            stagedBundleURL =             {
                relative = "file:///Library/SystemExtensions/FAB4E7D1-9509-4093-804F-AD986CA92A8E/com.obsproject.obs-studio.mac-camera-extension.systemextension/";
            };
            stagedCdhashes =             {
                4a731dc428b318c0d376df191aec52b5de6abefe =                 {
                    cpusubtype = 3;
                    cputype = 16777223;
                };
            };
            state = "activated_enabled";
            teamID = 2MMRE5MTB8;
            uniqueID = "FAB4E7D1-9509-4093-804F-AD986CA92A8E";
        }
    );
    version = 1;
}
```