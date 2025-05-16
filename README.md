# FortiGate firewall troubleshooting guide
based on the commands and topics you shared. This covers general health checks, VPN issues, performance, HA status, sessions, interfaces, logging, and backups.
### 1. Check Basic System Status
* Verify FortiGate software version, operation mode, system time, and general health.
```shell
get sys status
```
![image](https://github.com/user-attachments/assets/f87cdfd6-874b-4975-ae2b-41bb1ee0eee7)

### Check output for:

* Firmware version and patch level

* Operation Mode (NAT or Transparent)

* HA mode (Standalone or Cluster)

* System time (important for VPNs, logs)

* Serial number, hostname, etc.

### 2. Check Firewall Traffic Statistics
* Understand the traffic mix (DNS, email, TCP, UDP, etc.) to see if traffic is flowing normally.
```shell
get system performance firewall statistics
```
![image](https://github.com/user-attachments/assets/feba55ba-d4d7-4fa0-b015-1be3376483bc)

### 3. Check CPU and Memory Usage
* Check overall CPU, memory, uptime.
```shell
get system performance status
```
![image](https://github.com/user-attachments/assets/9fc36ca6-f4d2-4943-9419-d8288d222ca2)

* If CPU is high, find top CPU consumers:
```shell
get system performance top
```
![image](https://github.com/user-attachments/assets/86ed80a8-22cd-4d9d-84e9-b460ad32267e)

### 4. Check High Availability (HA) Status
* Check cluster status if HA is configured:
```shell
get sys ha status
```
* View full HA config:

```shell
show full-configuration system ha
```
* Diagnose HA detailed status:
```shell
diagnose sys ha status
```
Look for issues like secondary unit being offline.

### 5. Check Firewall Session Table
* Review max sessions vs used sessions:
```shell
diag sys session full-stat
```
![image](https://github.com/user-attachments/assets/bf68c230-54db-48b2-8ae1-c193153a90f7)

* List active sessions (use with caution on busy systems):
```shell
diag sys session list
```
* Filter sessions by source IP (useful for troubleshooting specific hosts):
* 
```shell
diagnose sys session filter src <IP_address>
diag sys session list
```
### 6. Check Interface Status and Details
Check physical interface states (speed, duplex, link status):
```shell
get system interface physical
```
* Check detailed info of specific interface (e.g., internal):
```shell
diagnose hardware deviceinfo nic internal
```
### 7. Check ARP Table
* Verify ARP entries to confirm layer 2 reachability:
```shell
get router info routing-table all
```
* Search for specific routes (e.g., to a particular IP):
```shell
get router info routing-table details <IP_address>
```
### 9. VPN Troubleshooting
* Manually bring up/down VPN tunnels:
```shell
diag vpn tunnel up <phase2-name> <phase1-name>
diag vpn tunnel down <phase2-name> <phase1-name>
```
* Check VPN tunnel status:
  Check if Security Associations (SAs) exist (if none, tunnel is down):
  
```shell
diagnose vpn tunnel list name <tunnel_name>
diagnose vpn tunnel dumpsa
diagnose vpn tunnel stat
```
* For active tunnels, verify:

    * VPN peers

    * Traffic counters (encrypted packets)

    * Encryption method

    * SPI values
* Compare packet counters over time to ensure traffic is flowing:
```shell
diagnose vpn ipsec status
```

