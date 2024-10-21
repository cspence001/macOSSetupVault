
TO-DO:
Review and add other packetfilter notes, scripts, and configurations from `~/folder/macsetup/_packetfilter/`

#### pf Overview

In addition to ALF, MacOS also provides a packet level filter called `pf`. This is an in-kernel facility, but may be controlled from user mode through the `/dev/pf[m]` character devices, and the `pfctl` (8) command, as well as tiles in `/etc`, notably `pf.conf` (5). This is the main config file, which loads the ruleset on startup, and may redirect/include other files (usually, in `/etc/pf.anchors`) as well. pf is borrowed from BSD, and is similar in some respects to Linux's netfilter mechanism (the foundation of its `iptables` firewall).

The operation of pf is explained in more detail in Volume II; Note, that this level of firewalling filters on a per-packet basis, rather than ALF's, which "sees much higher into the OSI stack at the application level. This brings advantages (e.g. packet-level dropping, NAT, masquerading and more), and disadvantages (inability to reassemble packets).
[ref](https://newosxbook.com/files/moxii3/AppendixA.pdf)

---

## pfctl Commands

- **Enable and Load Rules:**
  ```bash
  sudo pfctl -e -f ~/pf_rules.conf
  ```

- **Create pflog0 Interface:**
  ```bash
  sudo ifconfig pflog0 create
  ```

- **Monitor Traffic with `tcpdump`:**
  ```bash
  sudo tcpdump -n -e -ttt -i pflog0
  sudo tcpdump -ni pflog0
  ```

---
##### [pf.conf](https://github.com/drduh/config/blob/master/pf/pf.conf)

The `pf.conf` file you provided is a configuration file for Packet Filter (PF), a firewall tool used on macOS and BSD systems. PF is responsible for controlling network traffic based on the rules defined in this configuration file. Here's a detailed explanation of the key components of this configuration:

### Overview of `pf.conf`

1. **Interface Definitions**
   ```bash
   ext = "em0"
   red = "em1"
   blue = "em2"
   green = "em3"
   wifi = "athn0"
   vpn = "tun0"
   ```
   - These lines define network interfaces used in the PF rules. They assign names to interfaces, making the configuration more readable and manageable.

2. **Miscellaneous Settings**
   ```bash
   ntp_server = "192.168.1.1"
   routers = "{ 10.8.1.1, 172.16.1.1, 192.168.1.1 }"
   table <blocklist> persist file "/etc/pf/blocklist"
   table <martians> persist file "/etc/pf/martians"
   table <private> persist file "/etc/pf/private"
   ```
   - `ntp_server`: Defines the NTP server IP address for time synchronization.
   - `routers`: A list of router IP addresses.
   - `table <blocklist>`: Defines a PF table for blocked IP addresses, loading the list from `/etc/pf/blocklist`. [/pf/blocklist](https://github.com/drduh/config/blob/master/pf/blocklist)
   - `table <martians>`: Defines a PF table for "martian" IP addresses (addresses that should not be routed on the internet).  [/pf/martians](https://github.com/drduh/config/blob/master/pf/martians)
   - `table <private>`: Defines a PF table for private IP address ranges. [/pf/private](https://github.com/drduh/config/blob/master/pf/private)

3. **Global Settings**
   ```bash
   set block-policy drop
   set state-policy if-bound
   set debug info
   set loginterface $ext
   set optimization normal
   set limit { states 30000, table-entries 250000 }
   set skip on lo0
   ```
   - `block-policy drop`: Specifies that the default action for blocked traffic is to drop packets.
   - `state-policy if-bound`: States are bound to interfaces.
   - `debug info`: Enables logging of debug information.
   - `loginterface $ext`: Specifies which interface PF should log traffic on.
   - `optimization normal`: Sets the optimization level for PF.
   - `limit { states 30000, table-entries 250000 }`: Sets limits for the number of states and table entries PF can handle.
   - `set skip on lo0`: Skips rules on the loopback interface (`lo0`).

4. **Basic Filtering Rules**
   ```bash
   block quick from any to lo0:network
   block log quick from <blocklist> to any
   block log quick from any to <blocklist>
   antispoof quick for { $ext $red $blue $green $wifi }
   match in all scrub (no-df random-id max-mss 1440)
   match out on egress inet from !(egress:network) to any nat-to (egress:0)
   ```
   - `block quick from any to lo0:network`: Blocks any traffic to the loopback network.
   - `block log quick from <blocklist> to any`: Blocks and logs traffic from IPs listed in the `blocklist` table.
   - `block log quick from any to <blocklist>`: Blocks and logs traffic to IPs listed in the `blocklist` table.
   - `antispoof quick for { $ext $red $blue $green $wifi }`: Prevents IP spoofing for specified interfaces.
   - `match in all scrub (no-df random-id max-mss 1440)`: Scrubs incoming packets by clearing the DF bit, randomizing IDs, and setting maximum segment size.
   - `match out on egress inet from !(egress:network) to any nat-to (egress:0)`: NATs outgoing traffic on the egress interface.

5. **Specific Rules**
   ```bash
   pass out quick proto udp from $ext to $ntp_server port 123
   block in log quick from no-route to any
   block in log inet from any to localhost
   block in log quick on egress from <martians> to any
   block return out log quick on egress from any to <martians>
   block log all
   pass in quick on { $red $blue $green $wifi $vpn } inet keep state
   pass out quick on $ext proto { tcp, udp, icmp } from $ext to any keep state
   ```
   - `pass out quick proto udp from $ext to $ntp_server port 123`: Allows outgoing NTP traffic to the defined NTP server.
   - `block in log quick from no-route to any`: Blocks and logs traffic from IPs that have no route.
   - `block in log inet from any to localhost`: Blocks and logs incoming traffic to localhost.
   - `block in log quick on egress from <martians> to any`: Blocks and logs traffic from "martian" IP addresses on the egress interface.
   - `block return out log quick on egress from any to <martians>`: Blocks and logs traffic to "martian" IP addresses on the egress interface.
   - `block log all`: Blocks all traffic and logs it.
   - `pass in quick on { $red $blue $green $wifi $vpn } inet keep state`: Allows incoming traffic on specified interfaces and keeps state.
   - `pass out quick on $ext proto { tcp, udp, icmp } from $ext to any keep state`: Allows outgoing TCP, UDP, and ICMP traffic on the external interface and keeps state.

6. **Custom Rules**
   ```bash
   private:
   # https://github.com/drduh/config/blob/master/pf/private
   #0.0.0.0/8
   #100.64.0.0/10
   #127.0.0.0/8
   10.0.0.0/8
   172.16.0.0/12
   192.168.0.0/16

   martians:
   # https://github.com/drduh/config/blob/master/pf/martians
   # https://en.wikipedia.org/wiki/Reserved_IP_addresses
   #10.0.0.0/8
   #172.16.0.0/12
   0.0.0.0/8
   100.64.0.0/10
   127.0.0.0/8
   169.254.0.0/16
   192.0.0.0/24
   192.0.2.0/24
   192.168.0.0/16
   233.252.0.0/24
   192.88.99.0/24
   198.18.0.0/15
   198.51.100.0/24
   203.0.113.0/24
   224.0.0.0/3
   224.0.0.0/4
   240.0.0.0/4
   ```
   - **`private` table**: Contains private IP address ranges.
   - **`martians` table**: Contains reserved or "martian" IP addresses that should not appear on the internet.
   - **`blocklist` table**: Contains blocked IP addresses.

### Summary

- **Tables**: Defined for blocklists, private IPs, and martian IPs. These tables are used for filtering traffic based on IP address ranges.
- **Rules**: Include blocking and logging traffic, allowing specific types of traffic, and applying NAT. 
- **Optimization and Limits**: Configured to handle a reasonable number of states and table entries.
- **Customizable**: There are placeholders for custom rules and configurations specific to the user’s network setup.

This configuration provides a comprehensive firewall setup, balancing security and usability, and includes provisions for logging and monitoring network traffic.

---


##### [pf-blocklist.sh](https://github.com/drduh/config/blob/master/scripts/pf-blocklist.sh)
This Bash script is designed to update a blocklist used by the Packet Filter (PF) on macOS or BSD systems. PF is a firewall tool that can filter network traffic and block access based on various criteria. Here's a detailed breakdown of what the script does:

### Script Breakdown

1. **Shebang and Metadata**
   ```bash
   #!/usr/bin/env bash
   # https://github.com/drduh/config/blob/master/scripts/pf-blocklist.sh
   ```
   This line specifies the script should be run with Bash and provides a URL to the script’s source or documentation.

2. **Configuration Variables**
   ```bash
   dns=1.1.1.1
   custom=pf-custom.$(date +%F)
   threats=pf-threats.$(date +%F)
   zones=pf-zones.$(date +%F)
   blocklist=pf-blocklist.$(date +%F)
   ```
   - `dns`: The DNS server to use for testing (Cloudflare’s DNS in this case).
   - `custom`, `threats`, `zones`, `blocklist`: Variables for filenames that include the current date.

3. **Permissions Check**
   ```bash
   doas whoami >/dev/null || exit 1
   ```
   This checks if the script is running with superuser privileges using `doas`. If not, the script exits with status 1.

4. **Display Current Rules**
   ```bash
   printf "Current rules: "
   doas pfctl -t blocklist -T show | wc -l
   ```
   This displays the current number of rules in the `blocklist` table.

5. **User Confirmation**
   ```bash
   action=""
   while [[ -z "${action}" ]] ;
     do read -n 1 -p "Continue? (y/n) " action
   done
   printf "\n"
   ```
   Prompts the user to confirm whether they want to proceed with updating the blocklist.

6. **Update Blocklist**
   ```bash
   if [[ "${action}" =~ ^([yY])$ ]] ; then
     rm $custom $threats $zones $blocklist 2>/dev/null
     touch $custom $threats $zones
   ```
   If the user confirms, it removes any existing blocklist files and creates new empty ones.

7. **Fetch Threat IPs**
   ```bash
   printf "Checking threats ..."
   curl -sq \
     "https://pgl.yoyo.org/adservers/iplist.php?ipformat=&showintro=0&mimetype=plaintext" \
     ...
     "https://isc.sans.edu/api/threatlist/shodan/shodan.txt" | \
   grep -Ev "^192\.168\.|^10\.|172\.16\.|127\.0\.0\.0|0\.0\.0\.0|^#|#$" | \
   grep -E "^[0-9]" >> $threats
   wc -l $threats
   ```
   Fetches IP addresses from various threat intelligence sources, filters out private and invalid addresses, and appends the results to the `$threats` file.

8. **Fetch ASN IP Ranges**
   ```bash
   printf "Checking asns ..."
   for asn in $(find ../asns -type f) ; do
     printf "# $asn\n" >> $custom
     for nb in $(grep -v "^#" $asn) ; do
       printf " $nb"
       whois -h whois.radb.net !g$nb | tr " " "\n" | \
         grep -Eo "([0-9]{1,3}\.){3}[0-9]{1,3}\/[0-9]+" >> $custom
     done
   done
   wc -l $custom
   ```
   Iterates over files in the `../asns` directory, performs a WHOIS lookup for each Autonomous System Number (ASN), and extracts IP ranges. These are appended to the `$custom` file.

9. **Fetch Country IP Ranges**
   ```bash
   printf "Checking zones ..."
   for zone in $(grep -v "^#" ../zones | sed "s/\ \ \#.*//g") ; do
     printf " $zone"
     curl -sq \
       "https://www.ipdeny.com/ipblocks/data/countries/$zone.zone" >> $zones
   done
   wc -l $zones
   ```
   Fetches IP blocks assigned to countries from the `../zones` directory, appends the data to the `$zones` file.

10. **Combine and Apply Blocklist**
    ```bash
    sort $custom $threats $zones | uniq > $blocklist
    wc -l $blocklist

    if [[ ! -s $blocklist ]] ; then
      printf "Error: empty blocklist\n" ; exit 1
    fi

    doas cp -v /etc/pf/blocklist /etc/pf/blocklist.$(date +%F) && \
      doas cp -v ./$blocklist /etc/pf/blocklist
    doas pfctl -f /etc/pf.conf
    printf "\nnew rules: "
    doas pfctl -t blocklist -T show | wc -l
    ```
    Combines and sorts the IP addresses from the `custom`, `threats`, and `zones` files into the final `$blocklist` file. If the blocklist is not empty, it replaces the existing blocklist file and reloads PF configuration.

11. **Testing Blocked Sites (if not updating)**
    ```bash
    else
      printf "\ntesting blocked sites ...\n"
      for ws in $(/bin/ls ../asns) ; do
        printf "\n$ws.com: "
        curl -v \
          https://$(dig a $ws.com @$dns +short|head -n1) 2>&1 | \
            grep "Permission denied" || printf "BLOCK FAILED"
      done
    fi
    ```
    If the user chooses not to update the blocklist, this section tests whether sites associated with the ASNs in `../asns` are correctly blocked by the current PF rules.

### Summary

- The script updates the PF blocklist on a macOS or BSD system by combining IP addresses from various threat intelligence sources, ASN assignments, and country-specific IP blocks.
- It requires elevated privileges to modify system files and update the firewall rules.
- It includes a prompt for user confirmation before making changes and provides a way to test whether the blocklist is working if the update is skipped.

This script is useful for maintaining a dynamic blocklist to protect against known threats and unwanted traffic.
