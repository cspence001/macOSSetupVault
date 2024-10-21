### Investigating Process Invocation on macOS:

To determine what invoked a process, such as `rpcsvchost`:

1. **Check Process Hierarchy:**
   - Use `ps` to find the Parent Process ID (PPID) and the invoking process:
     ```bash
     ps -ef | grep rpcsvchost # get PID, PPID
     ps -o ppid= -p <PID_of_rpcsvchost> 
     ps -p <PPID>
     ```

2. **Check Command-line Arguments:**
   - Find the arguments with which `rpcsvchost` was invoked:
     ```bash
     ps -o args= -p <PID_of_rpcsvchost>
     ```

3. **Inspect Launch Daemons/Agents:**
   - Check if a `launchctl` job triggered the process:
     ```bash
     launchctl list | grep rpcsvchost
     ```
For more on Launch Services, such as Process Info and Services Status, See [[launchctl]]

4. **Inspect Process Details:**
- To get detailed information about the process, including its environment and attributes, use:
```sh
	sudo launchctl procinfo <PID_of_rpcsvchost>
```
 
5. **Analyze System Logs:**
   - Use the `log` command to search for logs related to `rpcsvchost`:
     ```bash
     log show --predicate 'eventMessage contains "rpcsvchost"' --info
     log show --predicate 'eventMessage contains "rpcsvchost"' --info --start "2024-09-06 12:00:00" --end "2024-09-06 23:59:59"
     ```
######  Output:
 ```sh
 $ log show --predicate 'eventMessage contains "rpcsvchost"' --info --start "2024-09-06 12:00:00" --end "2024-09-06 23:59:59"

Filtering the log data using "composedMessage CONTAINS "rpcsvchost""

Skipping debug messages, pass --debug to include.

Timestamp                       Thread     Type        Activity             PID    TTL  

2024-09-06 12:19:30.875462-0400 0xd6da     Default     0x0                  1      0    launchd: [system/com.apple.msrpc.netlogon [4331]:] Successfully spawned rpcsvchost[4331] because ipc (socket)

2024-09-06 13:24:02.485245-0400 0x1381b    Default     0x0                  1      0    launchd: [system/com.apple.msrpc.netlogon [6215]:] Successfully spawned rpcsvchost[6215] because ipc (socket)

2024-09-06 13:30:22.644828-0400 0x17bd5    Default     0x0                  0      0    kernel: (AppleMobileFileIntegrity) macOSTaskPolicy: (com.apple.xpc.launchctl) may not get the task control port of (rpcsvchost) (pid: 6215): (rpcsvchost) is hardened, (rpcsvchost) doesn't have get-task-allow, (com.apple.xpc.launchctl) is not a declared debugger(com.apple.xpc.launchctl) is not a declared read-only debugger

2024-09-06 13:58:28.123263-0400 0xb82e     Default     0x0                  1      0    launchd: [system/com.apple.msrpc.netlogon [7337]:] Successfully spawned rpcsvchost[7337] because ipc (socket)

--------------------------------------------------------------------------------------------------------------------

Log      - Default:          4, Info:                0, Debug:             0, Error:          0, Fault:          0

Activity - Create:           0, Transition:          0, Actions:           0
```
The logs show that `rpcsvchost` was spawned by the `launchd` process due to an Inter-Process Communication (IPC) request, specifically related to the `msrpc.netlogon` service. 

- **12:19:30**: `rpcsvchost` was spawned with PID 4331 due to an IPC (socket) request by `launchd` through `system/com.apple.msrpc.netlogon`.
- **13:24:02**: `rpcsvchost` was spawned again with PID 6215, also triggered by `launchd` through `system/com.apple.msrpc.netlogon`.
- **13:30:22**: The kernel denied `com.apple.xpc.launchctl` from obtaining the task control port of `rpcsvchost[6215]`, indicating that `rpcsvchost` is a hardened process without the `get-task-allow` entitlement.
- **13:58:28**: Another instance of `rpcsvchost` was spawned with PID 7337, again initiated by an IPC (socket) request by `launchd`.

From this, it appears that the `rpcsvchost` processes are related to handling RPC (Remote Procedure Call) requests for the `msrpc.netlogon` service. The spawns are likely associated with network or domain-related activities, such as authentication or communications with a domain controller.

To further investigate, you could look into the `msrpc.netlogon` service's configuration or the network activity around those times.

---

### Examining Process and Service Configuration on macOS:

To check the configuration of the `msrpc` (Microsoft Remote Procedure Call) service on macOS, particularly related to the `netlogon` service that is invoking `rpcsvchost`, you can do the following:

#### 1. Examine the Launch Daemon Configuration
   - The `msrpc.netlogon` service is managed by `launchd`, which loads its configuration from a **plist** file located in either `/System/Library/LaunchDaemons/` or `/Library/LaunchDaemons/`. You can inspect the plist to see the configuration details.
   - The system logs from the output above shows `system/com.apple.msrpc.netlogon` 

   Run the following command to locate the relevant plist:
   ```bash
   sudo find /System/Library/LaunchDaemons/ -name "*msrpc*"
   ```
   Alternative:
```sh
sudo find /System/Library/LaunchDaemons /Library/LaunchDaemons /Library/LaunchAgents /System/Library/LaunchAgents -name "*msrpc*"
```

Output:
```sh
/System/Library/LaunchDaemons//com.apple.msrpc.netlogon.plist
/System/Library/LaunchDaemons//com.apple.msrpc.lsarpc.plist
/System/Library/LaunchDaemons//com.apple.msrpc.mdssvc.plist
/System/Library/LaunchDaemons//com.apple.msrpc.wkssvc.plist
/System/Library/LaunchDaemons//com.apple.msrpc.echosvc.plist
/System/Library/LaunchDaemons//com.apple.msrpc.srvsvc.plist
```

   Once you find the corresponding plist (for example, `/System/Library/LaunchDaemons/com.apple.msrpc.netlogon.plist`), you can inspect its contents with:
   ```bash
   sudo defaults read /System/Library/LaunchDaemons/com.apple.msrpc.netlogon.plist
   ```
   This will show the configuration, including how the service is started, its `ProgramArguments`, and any conditions for invocation.
   ```sh
   {
    EnableTransactions = 0;
    EnvironmentVariables =     {
        "RPC_DEBUG" = 0;
    };
    Label = "com.apple.msrpc.netlogon";
    ProgramArguments =     (
        "/usr/libexec/rpcsvchost",
        "-launchd",
        "netlogon.bundle"
    );
    Sockets =     {
        ncalrpc =         {
            Bonjour = 0;
            SockPathMode = 438;
            SockPathName = "/var/rpc/ncalrpc/NETLOGON";
        };
    };
}
```

The configuration for the `com.apple.msrpc.netlogon` service in the plist file reveals the following key details:
##### Configuration Breakdown:

1. **Label**: 
   - `"com.apple.msrpc.netlogon"`: This is the identifier for the `netlogon` service managed by `launchd`.
2. **Program Arguments**:
   - `"/usr/libexec/rpcsvchost", "-launchd", "netlogon.bundle"`: 
     - This indicates that `rpcsvchost` is executed by `launchd`, and it is responsible for loading the `netlogon.bundle` service.
     - The `netlogon.bundle` plugin is being loaded by `rpcsvchost`, which handles the Remote Procedure Calls (RPCs) related to the Netlogon service.
3. **Enable Transactions**: 
   - `0`: This means that transactions are disabled for this service. It likely doesn't require transactional behavior during IPC communications.
4. **Environment Variables**:
   - `"RPC_DEBUG" = 0`: 
     - RPC debugging is turned off. This keeps the service running without verbose logging.
5. **Sockets**:
   - `ncalrpc`: 
     - This section defines the socket used by the `netlogon` service for communication.
     - **`SockPathName`**: `"/var/rpc/ncalrpc/NETLOGON"`: This is the socket path where the service listens for RPC requests.
     - **`SockPathMode`**: `438`: This sets the file permission mode to `0666` (read and write permissions for all users).
     - **`Bonjour = 0`**: This indicates that the service does not advertise its presence over Bonjour, meaning it is not discoverable via local network discovery protocols.

The `netlogon` service is configured to use the `rpcsvchost` binary to load the `netlogon.bundle` and listen for RPC requests on a local socket: `/var/rpc/ncalrpc/NETLOGON`. The service is minimalistic, with debugging disabled and no advertising over Bonjour.

#### 2. Examine `rpcsvchost` Configurations
   - Since `rpcsvchost` loads RPC services as plugins, you can check if any specific RPC services are being loaded via configuration. Look for files related to `msrpc` or `netlogon` in:
     - `/System/Library/LaunchDaemons/`
     - `/Library/LaunchDaemons/`

   Also, inspect the loaded modules or plugins that might relate to the `netlogon` service.

#### 3. Network Configuration
   - RPC services like `netlogon` are network-related, particularly involved in domain authentication. To investigate further, check for network settings related to domain controllers, Active Directory, or any other network service using RPC.

   Look into:
   - **Directory Services** (dsconfigad or dscl commands): If the Mac is connected to a domain, the `netlogon` service might be triggered during domain logins or authentications.
     ```bash
     sudo dsconfigad -show
     ```

   - **Network Services Configuration**: Inspect any associated network configurations in `/etc` or via system preferences related to domains or directory services.

#### 4. Check Running Services
   Use the following command to list the `launchd` services, including those related to `msrpc` or `netlogon`:
   ```bash
   launchctl list | grep msrpc
   ```
   This can help identify the current status and whether the service is running as expected.

By looking into these configurations, you can gather more details about how `msrpc` and `netlogon` are configured and why they may be invoking `rpcsvchost`.

```sh
log show --predicate 'eventMessage contains "com.apple.iCloudNotificationAgent"' --info --start "2024-09-06 12:00:00" --end "2024-09-06 23:59:59"
```

