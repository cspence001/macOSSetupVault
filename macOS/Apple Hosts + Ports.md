
#### Apple Host Domains BlackList
https://github.com/cedws/apple-telemetry/blob/master/blacklist

#### Apple Service/Host Domains

Apple makes extensive use of Akamai and AWS CloudFront CDNs, so not all addresses will resolve to an Apple 17.0.0.0 address.

**General Domains**
- `aaplimg.com`: Apple's in-house CDN
- `captive.apple.com`: Used for captive network testing (also used by `attwifi.apple.com`)
- `configuration.apple.com`: Alias for `configuration.apple.com.edgekey.net`, resolving to various Akamai IPs

**Activation and Delivery**
- `albert.apple.com`: Used for activation
- `appldnld.apple.com`: iOS firmware delivery
- `buy.itunes.com`: Purchase and account validation

**Search and Analytics**
- `ax.itunes.apple.com`: Apple search
- `metrics.apple.com`: Apple analytics

**CDNs and Caching**
- `*.cdn-apple.com`: Apple CDN network
- `mzstatic.com`: Apple in-house CDN
- `lcdn-locator.apple.com`: Likely used with Caching Service
- `lcdn-registration.apple.com`: Caching Service registration

**Certificates and Validation**
- `*.digicert.com`: Used for certificate validation (`crl3.digicert.com`, `crl4.digicert.com`, and `ocsp.digicert.com` are used for Apple Business Manager and Apple School Manager)
- `evintl-ocsp.verisign.com`: Used for certificate validation
- `evsecure-ocsp.verisign.com`: Used for certificate validation
- `ocsp.apple.com`: Used to validate certificates
- `*.symcb.com`: Used for certificate validation
- `*.symcd.com`: Used for certificate validation

**iTunes and Updates**
- `phobos.apple.com`: iTunes
- `su.itunes.apple.com`: App updates
- `swcdnlocator.apple.com`: Related to Apple Mac Software Update
- `swscan.apple.com`: Front end to Apple Mac Software Update
- `swdist.apple.com`: Related to Apple Mac Software Update

**Push Notifications**
- `push.apple.com`: Resolves to various Apple 17.0.0.0 IPs. Used for APNS (Includes `gateway.push.apple.com`, `feedback.push.apple.com`, and `courier.push.apple.com`). For push notification services, `api.development.push.apple.com:443` and `api.push.apple.com:443` are the HTTP/2 compatible sites.

**Specific Domains**
- `gs.apple.com`: Alias for `gs.apple.com.akadns.net`, resolving to various Apple 17.0.0.0 IPs. Used for:
  - iOS signature validation
  - Destination of the personalization manifest from T1 or T2-equipped Macs
  - LaunchServices contacts to verify app stapling ticket revocation state
  - Validate update servers used by the Caching service
- `gsp*.apple.com` & `gsp*-ssl.apple.com`: Used with geolocation services
- `ig.apple.com`: Used with Touchbar Macs
- `iprofiles.apple.com`: Used by Apple Business Manager and Apple School Manager. Initial service URL/endpoint for a DEP device to discover whether Apple has an MDM configured for the device
- `littlebuddy.apple.com`: Used by Setup Assistant
- `skl.apple.com`: Used with Touchbar Macs

**iCloud and System Daemons**
- `bird`: iCloud documents daemon. `brctl` is the binary used to interact with it.
- `cloudconfigurationd`: The Device Enrollment client daemon, which communicates with the DEP API and retrieves Device Enrollment profiles. Can connect to `iprofiles.apple.com` and `suconfig.apple.com`.
- `storeaccountd`: Can connect to `phobos.apple.com`, `ax.init.itunes.apple.com`, and `play.itunes.apple.com`
- `SubmitDiagInfo`: Can connect to `radarsubmissions.apple.com`

**Other Services**
- `Safari`: Can connect to `extensions.apple.com` and `plugins.apple.com`
- `SpotlightNetHelper`: Can connect to `cloudfront.net`, `init.itunes.apple.com`, `api-glb-chi.smoot.apple.com`, and `api.smoot.apple.com`
- `panic.apple.com`: Associated with error reporting

Reference: [cheatsheets macOS](https://www.zoocoup.org/exhibits/cheatsheets/macos.html)

---
### Overview of Domain Usage 
https://support.apple.com/en-us/101555

**Apple Products on Enterprise Networks**
**Published Date:** July 29, 2024

This document is intended for enterprise and education network administrators. Apple products require access to the internet hosts listed below for various services. Network connections to these hosts are initiated by the device, not by hosts operated by Apple. Apple services will fail any connection that uses HTTPS Interception (SSL Inspection). If the HTTPS traffic traverses a web proxy, disable HTTPS Interception for the hosts listed in this document.

---
#### Device Setup

Apple devices need access to the following hosts during setup, or when installing, updating, or restoring the operating system.

| **Hosts**                 | **Ports**       | **Protocol** | **OS**                           | **Description**                            | **Supports Proxies** |
|---------------------------|-----------------|--------------|----------------------------------|--------------------------------------------|----------------------|
| albert.apple.com          | 443             | TCP          | iOS, iPadOS, tvOS, macOS, visionOS | Device activation                          | Yes                  |
| captive.apple.com         | 443, 80         | TCP          | iOS, iPadOS, tvOS, macOS, visionOS | Internet connectivity validation            | Yes                  |
| gs.apple.com              | 443             | TCP          | iOS, iPadOS, tvOS, macOS, visionOS |                                            | Yes                  |
| humb.apple.com            | 443             | TCP          | iOS, iPadOS, tvOS, macOS, visionOS |                                            | Yes                  |
| static.ips.apple.com      | 443, 80         | TCP          | iOS, iPadOS, tvOS, macOS, visionOS |                                            | Yes                  |
| sq-device.apple.com       | 443             | TCP          | iOS, iPadOS, visionOS             | eSIM activation                             | —                    |
| tbsc.apple.com            | 443             | TCP          | iOS, iPadOS, tvOS, macOS, visionOS |                                            | Yes                  |
| time-ios.apple.com        | 123             | UDP          | iOS, iPadOS, tvOS, visionOS       | Used by devices to set their date and time | —                    |
| time.apple.com            | 123             | UDP          | iOS, iPadOS, tvOS, macOS, visionOS | Used by devices to set their date and time | —                    |
| time-macos.apple.com      | 123             | UDP          | macOS only                        | Used by devices to set their date and time | —                    |

---

#### Device Management

Apple devices enrolled in MDM need access to the following hosts and domains.

| **Hosts**                        | **Ports**           | **Protocol** | **OS**                           | **Description**                                                  | **Supports Proxies** |
|----------------------------------|---------------------|--------------|----------------------------------|------------------------------------------------------------------|----------------------|
| *.push.apple.com                 | 443, 80, 5223, 2197 | TCP          | iOS, iPadOS, tvOS, macOS, visionOS | Push notifications                                                | Learn more about APNs and proxies. |
| deviceenrollment.apple.com       | 443                 | TCP          | iOS, iPadOS, tvOS, macOS          | DEP provisional enrollment                                        | —                    |
| deviceservices-external.apple.com | 443                 | TCP          | iOS, iPadOS, tvOS, macOS          |                                                                  | —                    |
| gdmf.apple.com                   | 443                 | TCP          | iOS, iPadOS, tvOS, macOS          | Used by an MDM server to identify available software updates      | Yes                  |
| identity.apple.com               | 443                 | TCP          | iOS, iPadOS, tvOS, macOS, visionOS | APNs certificate request portal                                    | Yes                  |
| iprofiles.apple.com              | 443                 | TCP          | iOS, iPadOS, tvOS, macOS          | Hosts enrollment profiles for Apple School Manager or Apple Business Manager | Yes                  |
| mdmenrollment.apple.com          | 443                 | TCP          | iOS, iPadOS, tvOS, macOS          | MDM servers to upload enrollment profiles and look up devices and accounts | Yes                  |
| setup.icloud.com                 | 443                 | TCP          | iOS, iPadOS                       | Required to log in with a Managed Apple ID on Shared iPad          | —                    |
| vpp.itunes.apple.com             | 443                 | TCP          | iOS, iPadOS, tvOS, macOS, visionOS | MDM servers to perform operations related to Apps and Books       | Yes                  |
| *.appattest.apple.com            | 443                 | TCP          | iOS, iPadOS, tvOS, macOS, visionOS | Managed device attestation                                        | —                    |

---

#### Apple Business Manager and Apple School Manager

Administrators and managers need access to the following hosts and domains.

| **Hosts**                        | **Ports** | **Protocol** | **Description**             | **Supports Proxies** |
|----------------------------------|-----------|--------------|-----------------------------|----------------------|
| *.business.apple.com             | 443, 80   | TCP          | Apple Business Manager      | —                    |
| *.school.apple.com               | 443, 80   | TCP          | Apple School Manager        | —                    |
| appleid.cdn-apple.com            | 443       | TCP          | Login authentication        | Yes                  |
| idmsa.apple.com                  | 443       | TCP          | Login authentication        | Yes                  |
| *.itunes.apple.com               | 443, 80   | TCP          | Apps and Books              | Yes                  |
| *.mzstatic.com                   | 443       | TCP          | Apps and Books              | —                    |
| api.ent.apple.com                | 443       | TCP          | Apps and Books (ABM)        | —                    |
| api.edu.apple.com                | 443       | TCP          | Apps and Books (ASM)        | —                    |
| statici.icloud.com               | 443       | TCP          | Device icons                | —                    |
| *.vertexsmb.com                  | 443       | TCP          | Validating tax-exempt status| —                    |
| www.apple.com                    | 443       | TCP          | Fonts for certain languages | —                    |
| upload.appleschoolcontent.com    | 22        | SSH          | SFTP uploads                | Yes                  |

---

#### Apple Business Essentials Device Management

Administrators and devices managed by Apple Business Essentials need access to the following hosts and domains.

| **Hosts**                        | **Ports** | **Protocol** | **Description**             | **Supports Proxies** |
|----------------------------------|-----------|--------------|-----------------------------|----------------------|
| axm-adm-enroll.apple.com         | 443       | TCP          | DEP enrollment server       | —                    |
| axm-adm-mdm.apple.com            | 443       | TCP          | MDM server                  | —                    |
| axm-adm-scep.apple.com           | 443       | TCP          | SCEP server                 | —                    |
| axm-app.apple.com                | 443       | TCP          | View and manage apps        | —                    |
| *.apple-mapkit.com               | 443       | TCP          | View the location of devices| —                    |
| icons.axm-usercontent-apple.com  | 443       | TCP          | Custom Package icons        | —                    |

---

#### Classroom and Schoolwork

Student and Teacher devices using the Classroom or Schoolwork apps need access to the following hosts.

| **Hosts**                        | **Ports** | **Protocol** | **OS**       | **Description**                    | **Supports Proxies** |
|----------------------------------|-----------|--------------|--------------|------------------------------------|----------------------|
| s.mzstatic.com                   | 443       | TCP          | iPadOS, macOS | Classroom and Schoolwork device verification | —                    |
| play.itunes.apple.com           | 443       | TCP          | iPadOS, macOS | Classroom and Schoolwork device verification | —                    |
| ws-ee-maidsvc.icloud.com        | 443       | TCP          | iPadOS, macOS | Classroom and Schoolwork class roster service | —                    |
| ws.school.apple.com              | 443       | TCP          | iPadOS, macOS | Classroom and Schoolwork class roster service | —                    |
| pg-bootstrap.itunes.apple.com    | 443       | TCP          | iPadOS        | Schoolwork handout service         | —                    |
| cls-iosclient.itunes.apple.com   | 443       | TCP          | iPadOS        | Schoolwork handout service         | —                    |
| cls-ingest.itunes.apple.com      | 443       | TCP          | iPadOS        | Schoolwork handout service         | —                    |

---

#### Software Updates

Apple devices need access to the following hosts when installing, restoring, and updating iOS, iPadOS, macOS, watchOS, tvOS, and visionOS.

**Hosts**                     | **Ports** | **Protocol** | **OS**                       | **Description** | **Supports Proxies**  
----------------------------- | --------- | ------------ | ----------------------------- | --------------- | --------------------  
appldnld.apple.com            | 80        | TCP          | iOS, iPadOS, watchOS, visionOS |                 | Yes                  
swcdn.apple.com               | 443       | TCP          | iOS, iPadOS, watchOS, visionOS |                 | Yes                  
updates.cdn-apple.com         | 80, 443   | TCP          | iOS, iPadOS, watchOS, visionOS |                 | Yes                  
swscan.apple.com              | 80, 443   | TCP          | iOS, iPadOS, watchOS, visionOS |                 | Yes                  
updates.apple.com             | 80, 443   | TCP          | iOS, iPadOS, watchOS, visionOS |                 | Yes                  
swcdn-ssl.apple.com           | 443       | TCP          | iOS, iPadOS, macOS, watchOS    |                 | Yes                  

---

**Support**

For support, refer to [Apple Support](https://support.apple.com/) for the latest information and troubleshooting tips.

**Additional Resources**

- [Apple IT Support](https://support.apple.com/enterprise)
- [Apple Security Updates](https://support.apple.com/en-us/HT201222)
