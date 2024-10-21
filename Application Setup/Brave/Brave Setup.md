### Brave Browser Configuration Notes

brave://settings
#### Privacy and Security Settings
##### Privacy and security
Delete browsing data > On exit
	Browsing History
	Download History
	Cookies and other site data
	Cached Images and files

Security
	Standard Protection
	Use Secure DNS
	Select DNS Provider > OS default
	
Site and Shields Settings
	Content >
		Block Cookies > Block 3rd Party Cookies
		Pop-ups and redirects > Don't allow sites to send pop-ups or use redirects
	Additional Content Settings  
		On Device Site Data > Delete data sites have saved to your device when you close all windows
	Automatically remove permissions from unused sites (Enabled)

**Privacy and Security** > **WebRTC IP handling policy > Disable non-proxied UDP**
- **WebRTC IP Handling Policy:**
  - **Disable Non-Proxied UDP**: Ensure WebRTC uses TCP to contact peers or servers unless the proxy server supports UDP. This setting prevents exposing local addresses.
  - **WebRTC Overview**: WebRTC allows for direct connections for features like video chat, which can reveal your IP address. Brave provides settings to manage this but typically, default settings are sufficient.
  - **Policies:**
    - **Default**: WebRTC can enumerate and bind all interfaces, discovering public interfaces.
    - **Default Public And Private Interfaces**: WebRTC uses the default HTTP route, exposing the default private address.
    - **Default Public Interface Only**: WebRTC uses only the default HTTP route, avoiding exposure of local addresses.
    - **Disable Non-Proxied UDP**: WebRTC uses only TCP unless the proxy supports UDP, hiding local addresses.
Use Google services for push messaging (Disabled)
Auto-redirect AMP pages (Enabled)
Auto-redirect tracking URLs (Enabled)
Prevent sites from fingerprinting me based on my language preferences (Enabled)
Send a "Do Not Track" request with your browsing traffic (Disabled)
###### Tor windows
Private window with Tor (Enabled)
Only resolve .onion addresses in Tor windows (Enabled)
Volunteer to help others connect to the Tor network (Disabled)
Use Bridges (Disabled)
###### Data collection
Allow privacy-preserving product analytics (P3A) (Disabled)
Automatically send daily usage ping to Brave (Disabled)
Automatically send diagnostic reports (Disabled)

##### Shields 
Show the number of blocked items on the Shields icon (Enabled)
Trackers & ads blocking > Standard      
Upgrade connections to HTTPS > Strict      
Block scripts (Disabled)
Block fingerprinting (Enabled)
Block cookies > Block third-party cookies       
Forget me when I close this site (Enabled)

Content filtering (See [[Brave Filters]])
Filter lists
	EasyList Cookie
	Fanboy's Annoyances + uBO Annoyances
	AdGuard URL Tracking Protection Filters
	Brave Experimental Adblock Rules

Create Custom Filters 
	See [[Brave Filters]]
###### Social media blocking
Allow Facebook logins and embedded posts > Disabled
Allow X (previously Twitter) embedded tweets > Disabled
Allow LinkedIn embedded posts > Disabled

---

brave://flags
#### Flags and Experimental Features
- **Enable Localhost Access Permission Prompt**: Enabled. Displays a prompt for permission when a site tries to connect to localhost (127.0.0.1).
- **Local Data Collection for Notification Ad Timing (brave-federated)**: Disabled.
- **Anonymize Local IPs Exposed by WebRTC**: Enabled. Conceals local IPs with mDNS hostnames across multiple platforms.
- **Allow Invalid Certificates for Resources Loaded from Localhost**: Disabled.
- **WebTransport Developer Mode**: Disabled.
- **Enable SpeedReader**: Enabled.
- **Reading Mode**: Enabled.
- **Brave AI Chat**: Enabled.
- **Brave AI Chat History**: Disabled.
- **Tab Groups Save and Sync**: Enabled.
- **Experimental QUIC Protocol**: Disabled.
- **Playlists**: Enabled.
- **Playlists FakeUA**: Enabled.
- **Enable Multi-State Option for Memory Saver Mode**: Enabled.
  - **Location**: Brave Settings > System > Memory > Timer.
- **Enable IPFS**: Disabled.
- **Treat 'Brave Experimental Adblock Rules' as a Default List Source**: Enabled.
- **Enable Debug Logging for Scriptlet Injections**: Enabled. Useful for viewing Twitch blocking and other scriptlet-related logs.
- **Enable Extension Network Blocking**: Enabled. Blocks network requests initiated by extensions.

---

#### Network and Security Internals
- **brave://net-internals/**
  - **brave://net-export/**: For exporting network logs.
  - **Netlog Viewer**: [Netlog Viewer](https://netlog-viewer.appspot.com/#import)
- **brave://net-internals/#hsts**
  - **About HSTS**: [Chromium HSTS](https://www.chromium.org/hsts/)
  - **Current Rules for Brave**: [HSTS Rules](https://source.chromium.org/chromium/chromium/src/+/main:net/http/transport_security_state_static.json)
  - **Additional Information**:
    - [Suggestion for localhost address issues](https://community.brave.com/t/cannot-use-http-localhost-8080/184736/6)
    - [Potential permanent fix](https://stackoverflow.com/questions/38968510/how-to-permanently-exclude-localhost-from-hsts-list-in-google-chrome)
  - **Delete Domain Security Policies**: Manage or delete policies related to localhost.
  - **Alternative Flag**: [Allow Insecure Localhost](chrome://flags/#allow-insecure-localhost)

---

#### Profile Internals
- **brave://profile-internals/**
  - **List of Profiles**: Shows profiles and corresponding IDs.

---
#### More Brave Directories
* **brave://about**

---

