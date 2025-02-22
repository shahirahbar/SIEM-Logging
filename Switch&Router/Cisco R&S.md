# Cisco Configuration Guide

To ensure proper configuration, the data required for various network layers must be collected. Cisco communication equipment should be configured to log necessary data for SOC (Security Operations Center). This includes successful and unsuccessful login attempts, CPU usage spikes or drops, and policy violations such as Port Security Violation. Logs should be categorized and sent to a central log management system for services such as Permit and Deny access policies.

| Level | Level Name    | Explanation                                      |
|-------|---------------|--------------------------------------------------|
| 0     | Emergency     | The system may be unusable.                     |
| 1     | Alert         | Immediate action may be required.               |
| 2     | Critical      | A critical event took place.                    |
| 3     | Error         | The router experienced an error.                |
| 4     | Warning       | A condition might warrant attention.            |
| 5     | Notification  | A normal but significant condition occurred.    |
| 6     | Informational | A normal event occurred.                        |
| 7     | Debugging     | The output is a result of a debug command.      |

It is important to note that in Cisco devices, logging levels range from 0 to 7. However, Level 7 (Debugging) is not typically used for security analysis due to the high volume of logs it generates. 

**Note:** The logging level for Cisco devices is set to "information."

The following configuration example demonstrates how to generate and send the aforementioned logs to a central log management system:

```plaintext
en
conf t
no logging host <Old Syslog Server IP>
login on-failure log
login on-success log
logging trap 7
logging source-interface vlan <VLAN Management-ID>
logging host <SIEM> transport udp port <Optional port>
ntp logging
logging userinfo
ip ssh logging events
snmp-server enable traps cpu threshold
process cpu threshold type total rising 80 interval 5 falling 40 interval 5
process cpu statistics limit entry-percentage 60 size 300
snmp-server enable traps syslog
logging on
archive
log config
logging enable
notify syslog contenttype plaintext
logging trap 7
hidekeys
do wr
```
**Note:** You can identify the <Old Syslog Server IP> by using the show running-config command.

**Important:** While executing these commands, you may encounter errors indicating that certain IOS versions do not support specific commands. For example, some versions may not support the transport udp port <Optional port> command and may default to port 514.
