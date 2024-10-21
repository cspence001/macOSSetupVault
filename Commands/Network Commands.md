##### Basic Commands 

1. **`ping`**:
   - Use this command to check if the IP address is reachable and measure response times.
   ```bash
   ping <IP_ADDRESS>
   ```
   Replace `<IP_ADDRESS>` with the IP address you want to check. For example:
   ```bash
   ping 8.8.8.8
   ```

2. **`traceroute`**:
   - This command shows the path packets take to reach the IP address, which can help diagnose network routing issues.
   ```bash
   traceroute <IP_ADDRESS>
   ```
   For example:
   ```bash
   traceroute 8.8.8.8
   ```

3. **`whois`**:
   - This command provides registration information about the IP address, including the organization that owns it.
   ```bash
   whois <IP_ADDRESS>
   ```
   For example:
   ```bash
   whois 34.128.128.0
   ```

4. **`nslookup`**:
   - This command queries DNS to get the domain name associated with an IP address.
   ```bash
   nslookup <IP_ADDRESS>
   ```
   For example:
   ```bash
   nslookup 34.128.128.0
   ```

5. **`dig`**:
   - This command provides detailed DNS information, including reverse DNS lookups.
   ```bash
   dig -x <IP_ADDRESS>
   ```
   For example:
   ```bash
   dig -x 34.128.128.0
   ```

---

#### Process Viewer

- **htop**
  Visit [htop Stack Overflow](https://stackoverflow.com/questions/8334433/how-to-examine-processes-in-os-xs-terminal)