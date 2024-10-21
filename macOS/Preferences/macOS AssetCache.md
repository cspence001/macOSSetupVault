
---

## AssetCache on macOS

Content caching in macOS enhances the speed of downloading software from Apple and data stored in iCloud by storing previously downloaded content locally on a Mac. This allows other Apple devices to access the saved content without needing to go online. Supported content types can be found in Apple's documentation.

The service works on networks using network address translation (NAT) or publicly routable IP addresses, and it can also support tethered devices via Apple Configurator. Apple devices automatically connect to nearby content caches through a lookup service, which ensures seamless access without additional configuration. However, due to privacy considerations, detailed information on individual client requests is not available, though aggregate statistics can be queried to assess performance.

**Important:** For optimal performance, deploy content caching on a Mac with a single wired Ethernet connection as its only connection to the network, as using Wi-Fi may impact speed.

**How Content Caching Works**
When content caching is enabled on a Mac, it stores copies of downloadable content for clients on the local network. This includes support for tethered iPhone and iPad devices. You can specify IP address ranges for clients that the cache will serve, and you can choose to make this content exclusive to those clients.

0c:8e:29:15:3a:e5

**Options for Content Caching**

| Option                                         | Description                                                                                                                                           |
|------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Cache content for**                         | Selects which devices this computer will cache content for:                                                                                         |
| - Devices using the same public IP address    | Caches content for devices sharing the same public IP address as the Mac.                                                                           |
| - Devices using the same local networks        | Caches content for devices on the same local networks as the Mac.                                                                                   |
| - Devices using custom local networks          | Caches content for devices on specified local networks.                                                                                              |
| - Devices using custom local networks with fallback | Caches content for specified local networks and devices sharing the same public IP address when their preferred cache is unavailable.                 |
| **My local networks**                          | Describes your network configuration:                                                                                                                |
| - Use one public IP address                    | Automatically discovers a single public IP address.                                                                                                  |
| - Use custom public IP addresses               | Allows specification of specific or multiple public IP addresses; requires additional DNS configuration.                                             |
| **DNS configuration**                          | Used to generate a DNS TXT record configuration, available only when using custom public IP addresses.                                               |

**Example Scenario**
When the first client downloads a macOS update, the content cache saves a copy. Subsequent clients can then download the update from the cache rather than the App Store, significantly speeding up the process due to the faster local network.

![A diagram showing a single subnet content cache.](https://help.apple.com/assets/66A14EF79678AA800E0B677D/66A14EFA9678AA800E0B6783/en_US/4dc985e49af3f9c753b0e40ee6d27f9b.png)

By default, content caching is limited to a specific subnet, but you can set it to provide content caching for:
- All combinations of subnets of the local network that share a common public IP address
- Any combination of subnets of publicly accessible IP addresses (with additional DNS settings being required)

**How subnets and caches interact**
In a network with multiple subnets sharing the same public IP address, all subnets can utilize a single content cache.

![A diagram showing content caching with multiple subnets.](https://help.apple.com/assets/66A14EF79678AA800E0B677D/66A14EFA9678AA800E0B6783/en_US/211a6395aa29d44f8eefb76e2059ff41.png)

When multiple content caches are present, they become peers and can share cached software. If a requested item isn't found on one cache, it checks its peers first. If still unavailable, the item is downloaded from a configured parent cache or directly from Apple. Clients automatically connect to the appropriate content cache when multiple options are available.

**Note**: If enabled, user iCloud data is stored on only one content cache and isn't replicated across peers or parents. Devices maintain a preference for their specific content cache for iCloud data as long as possible.

**Cached Content Storage**
Cached content is typically stored on the startup volume, but an alternate location can be specified. When the allocated storage limit is reached or available space is low, the cache automatically deletes the least recently used content to make room for new requests.

---

### References
- **Intro to Content Caching**: [Apple Developer Documentation](https://support.apple.com/guide/deployment/intro-to-content-caching-depde72e125f/1/web/1.0)
- **Advanced Content Caching Settings**: [Apple Developer Documentation](https://support.apple.com/guide/deployment/advanced-content-caching-settings-depc8f669b20/web)
- **Content Caching**: [Apple Developer Documentation](https://developer.apple.com/documentation/devicemanagement/contentcaching)
- **Configuration Profile Reference**: [Apple Developer Documentation](https://developer.apple.com/business/documentation/Configuration-Profile-Reference.pdf)

### AssetCache Commands
- **Basic Management**:
  ```bash
  /usr/bin/AssetCacheManagerUtil AllowPersonalCaching false
  /usr/bin/AssetCacheManagerUtil AllowSharedCaching false
  /usr/bin/AssetCacheManagerUtil AllowTetheredCaching false
  /usr/bin/AssetCacheManagerUtil deactivate
  ```

- **Cache Management**:
  ```bash
  /usr/bin/AssetCacheManagerUtil flushCache
  /usr/bin/AssetCacheManagerUtil flushPersonalCache
  /usr/bin/AssetCacheManagerUtil flushSharedCache
  /usr/bin/AssetCacheManagerUtil status
  /usr/bin/AssetCacheManagerUtil settings
  ```

### Preference Settings
#### Viewing Current Preferences
```bash
defaults read /Library/Preferences/com.apple.AssetCache.plist
```

#### Modifying Preferences
```bash
sudo defaults write /Library/Preferences/com.apple.AssetCache.plist Activated 0
sudo defaults write /Library/Preferences/com.apple.AssetCache.plist AllowCacheDelete 1
sudo defaults write /Library/Preferences/com.apple.AssetCache.plist AllowPersonalCaching 0
sudo defaults write /Library/Preferences/com.apple.AssetCache.plist AllowSharedCaching 0
sudo defaults write /Library/Preferences/com.apple.AssetCache.plist AllowWirelessPortable 0
sudo defaults write /Library/Preferences/com.apple.AssetCache.plist CacheLimit 0
sudo defaults write /Library/Preferences/com.apple.AssetCache.plist ListenWithPeersAndParents 0
sudo defaults write /Library/Preferences/com.apple.AssetCache.plist LogClientIdentity 1
```

#### Network Restrictions
```bash
# Restrict Content To Local Network
sudo defaults write /Library/Preferences/com.apple.AssetCache.plist LocalSubnetsOnly 0

# Restrict Peers To Local Network
sudo defaults write /Library/Preferences/com.apple.AssetCache.plist PeerLocalSubnetsOnly 0
sudo defaults write /Library/Preferences/com.apple.AssetCache.plist Port 443
```

### Key Preferences Explained
- **AllowPersonalCaching**: Caches user’s iCloud data; changes may take time to reflect.
- **AllowSharedCaching**: Caches non-iCloud content (apps, updates); changes may take time.
- **AllowCacheDelete**: Permits automatic cache purging for disk space.
- **AllowWirelessPortable**: Enables caching on Wi-Fi only devices.
- **CacheSize**: Maximum size of the cache in bytes.
- **CacheLimit**: Max percentage of disk space for caching.
- **DataPath**: Directory for cached content storage.
- **MaxCachingClientCount**: Maximum number of simultaneous clients accessing the cache.
- **ListenRanges**: IP address ranges for incoming connections.
- **PeerLocalSubnets**: Local subnets for content sharing.

### Defined Settings
- **Content Caching Profile**: [Profile Overview](https://docs.omnissa.com/bundle/macOS-Device-ManagementVSaaS/page/ProfilesOverview.html)

| Setting                               | Description                                                                                                                                                                                                                                                                                                                  |
| ------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Activate Content Caching              | Automatically activates the content cache when possible and prevents users from disabling it.                                                                                                                                                                                                                                |
| Cache size limit (bytes)              | The maximum amount of disk space (in bytes) that will be used for the content cache. A value of 0 means unlimited disk space.                                                                                                                                                                                                |
| Cache iCloud data                     | Caches the user’s iCloud data, such as documents and pictures. Clients may take some time (hours or days) to react to changes to this setting; it doesn’t have an immediate effect. Either “Cache iCloud data” or “Cache non-iCloud data,” or both, must be enabled.                                                         |
| Cache non-iCloud data                 | Caches non-iCloud content, such as apps and software updates. Clients may take some time (hours or days) to react to changes to this setting; it doesn’t have an immediate effect. Either “Cache iCloud data” or “Cache non-iCloud data,” or both, must be enabled.                                                          |
| Allow tethered caching                | Allows sharing cached content and internet connection sharing with iOS devices connected using USB.                                                                                                                                                                                                                          |
| Automatically enable tethered caching | Automatically enables Internet connection sharing and sharing cached content when possible for iOS devices connected using USB.                                                                                                                                                                                              |
| Port                                  | The TCP port on which the content cache accepts requests for uploads or downloads. If set to 0, a random available port is picked.                                                                                                                                                                                           |
| Data Path                             | The path to the directory used to store cached content. Changing this setting manually doesn’t automatically move cached content from the old location to the new one. To move content automatically, use the Sharing preference’s Content Caching pane. Must end with `/Library/Application Support/Apple/AssetCache/Data`. |
| Enable client logging                 | Logs the IP address and port of clients that request content.                                                                                                                                                                                                                                                                |
| Allow cache delete                    | Allows the system to purge content from the cache automatically when it needs disk space for other apps.                                                                                                                                                                                                                     |
| Display alerts                        | Content Caching can display alerts as system notifications.                                                                                                                                                                                                                                                                  |
| Keep awake                            | Prevents the computer from sleeping while Content Caching is enabled.                                                                                                                                                                                                                                                        |
| Parents                               | IP addresses of other content caches that this cache should download from or upload to, instead of downloading from or uploading to Apple directly. If all parent content caches become unavailable, the content cache downloads from or uploads to Apple directly until a parent becomes available again.                   |
| Parent selection policy               | The policy to implement when choosing among more than one configured parent content cache:                                                                                                                                                                                                                                   |
|                                       | - **first-available**: Always use the first available parent in the Parents list.                                                                                                                                                                                                                                            |
|                                       | - **url-path-hash**: Hash the path part of the requested URL to consistently use the same parent for the same URL.                                                                                                                                                                                                           |
|                                       | - **random**: Choose a parent at random for load balancing.                                                                                                                                                                                                                                                                  |
|                                       | - **round-robin**: Rotate through the parents in order for load balancing.                                                                                                                                                                                                                                                   |
|                                       | - **sticky-available**: Use the first available parent until it becomes unavailable, then advance to the next one.                                                                                                                                                                                                           |
| Public Ranges                         | Ranges of public IP addresses that the cloud servers should use for matching clients to content caches.                                                                                                                                                                                                                      |
| Listen with peers and parents         | If enabled, the content cache provides content to clients in any specified Listen Ranges, Peer Listen Ranges, or Parents. If disabled, it only uses the Listen Ranges.                                                                                                                                                       |
| Local subnets only                    | Content cache offers content to clients only on the same immediate local network.                                                                                                                                                                                                                                            |
| Listen Ranges only                    | Content cache offers content to clients in the specified ranges only.                                                                                                                                                                                                                                                        |
| Listen Ranges                         | Ranges of client IP addresses that are served by content caching.                                                                                                                                                                                                                                                            |
| Peer local subnets only               | The content cache only peers with other caches on the same immediate local network, not with those using the same public IP.                                                                                                                                                                                                 |
| Peer Listen Ranges                    | Ranges of peer IP addresses that the content cache responds to.                                                                                                                                                                                                                                                              |
| Peer Filter Ranges                    | Ranges of peer IP addresses that the content cache uses to filter its list of peers to query for content.                                                                                                                                                                                                                    |

### Example Configurations
- **Server Load Configuration**: [GitHub Gist](https://gist.github.com/weldon/53b65cbb06efba9cbf6e8e195c63a48f)

### Additional Resources
- **Asset Caching Commands & Descriptions**: [GitHub Gist](https://gist.github.com/ntrogers/f9e900ad3912966f6260d02e4beabfd0)
- **Asset Cache Preferences (mobileconfig)**: [ProfileCreator](https://github.com/ProfileCreator/ProfileManifests/blob/master/Manifests/ManifestsApple/com.apple.AssetCache.managed.plist)

### Security Compliance
- **Ensure Content Caching Is Disabled**: [Tenable Audit](https://www.tenable.com/audits/items/CIS_Apple_macOS_13.0_Ventura_v2.0.0_L2.audit:d9eba7d89569a8d29dae22707b13881a)
  ```bash
  /usr/bin/sudo /usr/bin/AssetCacheManagerUtil deactivate
  ```

#### Profile Configuration
- **Payload Type**: `com.apple.applicationaccess`
- **Key to Include**: `allowContentCaching`
- **Key Value**: `<false/>`
