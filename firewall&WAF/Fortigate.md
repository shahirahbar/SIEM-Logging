## FortiGate Configuration

FortiGate can be configured using both the graphical interface and the CLI environment.

### Graphical Interface Configuration  
To configure via the graphical interface, go to **Log & Report > Log Config > Log Settings**.  
Select **Send Logs to Syslog** and enter the information related to the **Syslog Server**. You can also select the desired event categories.

### CLI Configuration  
To configure via the CLI, enter the following commands:

```bash
config log syslogd setting
set status enable
set facility [By Standard local7]
set csv [enable | disable]
set port <optional port>
set reliable disable [Activate TCP-514 or UDP-514 which means UDP is default]
set server [IP address of the SIEM]
set source-ip [Source IP of FortiGate; By Standard 0.0.0.0]
end
