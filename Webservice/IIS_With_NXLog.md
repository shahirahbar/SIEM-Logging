# IIS Log Configuration with NXLog
## NXLog Agent Configuration
The NXLog Agent is used to send logs generated by the IIS service to the Log Receiver machine. This agent is designed for log forwarding and is available as an open-source tool for both Linux and Windows.
### Steps to Configure IIS Logging
1. Log in to the system with an account that has **Administrator** privileges.
2. Navigate to **Logging** in the IIS Management console.
3. Click on **Select Fields** and check all the fields. Ensure that the log format is set to **W3C**.
---
### Adding Custom Fields
In the **Select Fields** window, click on **Add Custom Field** and add the following three fields:

1. X-Forwarded-for
2. Content-Type
3. https
Click **OK** to save the changes. At this point, the IIS log configuration is complete. The next section explains how to send these logs using NXLog.

---
### NXLog Installation and Configuration

To forward logs to the Log collector machine using NXLog, follow these steps:

1. **Install NXLog**: Download and install the NXLog Community Edition (`nxlog-ce-2.10.2150`) using the default settings.
2. **Replace Configuration File**: change the `nxlog.conf` file located at `C:\Program Files\nxlog\conf\nxlog.conf` with:
```
define ROOT C:\Program Files\nxlog
define LOGFILE %ROOT%\data\nxlog.log

define OUTPUT_DESTINATION_ADDRESS <IP Address>
define OUTPUT_DESTINATION_PORT <port>

Moduledir %ROOT%\modules
CacheDir %ROOT%\data
Pidfile %ROOT%\data\nxlog.pid
SpoolDir %ROOT%\data
LogFile %ROOT%\data\nxlog.log

#######################################################################
####             NXlog Internal log Retention/Rotation            #####
####     		    (LOGFILE %ROOT%\data\nxlog.log)			      #####
#######################################################################

<Extension _fileop>
    Module xm_fileop
	
    #Check the log file size every hour and rotate if larger than 20 MB
    <Schedule>
        Every 1 hour
        <Exec>
            if (file_exists('%LOGFILE%') and file_size('%LOGFILE%') >= 20M)
                file_cycle('%LOGFILE%', 8);
        </Exec>
    </Schedule>
	
    #Rotate log file every week on Sunday at midnight
    <Schedule>
        When    @weekly
        Exec    if file_exists('%LOGFILE%') file_cycle('%LOGFILE%', 8);
    </Schedule>
</Extension>

#######################################################################
####             NXlog Internal log Retention/Rotation            #####
####     		    (LOGFILE %ROOT%\data\nxlog.log)			      #####
#######################################################################

#######################################################################
####                          IIS-NXLOG                           #####
####     Uncomment the following lines for IIS log forwarding     #####
#######################################################################

#Windows IIS events log:
<Input IIS_Logs>
    Module    im_file
    File    "C:\\inetpub\\logs\\LogFiles\\W3SVC1868132350\\u_ex*"
    SavePos  TRUE
   
    Exec if $raw_event =~/^#/ drop();\
       else\
       {\
       }
</Input>

#Output internal iis nxlog messages:
<Output out_alienvault_iis_nxlog>
    Module      om_udp
    Host        %OUTPUT_DESTINATION_ADDRESS%
    Port        %OUTPUT_DESTINATION_PORT%
    Exec    $Hostname = hostname_fqdn();
    Exec        $raw_event =$Hostname + ' IIS-NXLOG ' + $raw_event;
</Output>

#Route for iis nxlog logs:
<Route route_iis_nxlog>
    Path        IIS_Logs => out_alienvault_iis_nxlog
</Route>
#######################################################################
####                         /IIS-NXLOG                           #####
#######################################################################
```

3. **Configure Log Destination**: Open the `mylog.conf` file and specify the destination IP address and port (`optional port`) for the logs.

---
### Additional NXLog Configuration

- Ensure that the path for reading IIS logs is correctly set in the NXLog configuration file.
- If multiple websites are hosted on the server, multiple `input` and `output` configurations may be required. Refer to the `multi-input-nxlog.conf` file for guidance.
- After configuring NXLog, start the service using the following command in the Command Prompt (with Administrator privileges):

  ```bash
  net start nxlog
    ```
* The output should indicate that the service has started successfully. If not, there may be an issue with the configuration.
* Check the logs generated by NXLog in the C:\Program Files\nxlog\data\ directory for troubleshooting purposes.
---
### Verifying Log Transmission
To ensure that logs are being sent correctly, use tcpdump on the Log collector machine with the destination port <optional port>/UDP and the source IP address of the IIS server:
```
tcpdump -n -i any dst port <optional port> and host <IIS_server_IP> -A
```
Replace <IIS_server_IP> with the IP address of the IIS server.

---
### Retention and Rotation Settings
To configure log retention and rotation, add the following section to the nxlog.conf file:

```
<Extension _fileop>
  Module xm_fileop
  #Check the log file size every hour and rotate if larger than 20 MB
  <Schedule>
    Every 1 hour
    <Exec>
      if (file_exists('%LOGFILE%') and file_size('%LOGFILE%') >= 20M)
      file_cycle('%LOGFILE%', 8);
    </Exec>
  </Schedule>
  #Rotate log file every week on Sunday at midnight
  <Schedule>
    When @weekly
    Exec if file_exists('%LOGFILE%') file_cycle('%LOGFILE%', 8);
  </Schedule>
</Extension>
```
This configuration will:
* Check the log file size every hour and rotate it if it exceeds 20 MB.
* Rotate the log file every Sunday at midnight.
* Keep a maximum of 8 rotated log files.
---
## Installation Checklist
1. Log in to the system with an account that has Administrator privileges.
2. Ensure that the date, time, and timezone are correctly set on the operating system. If necessary, sync the system with an internal NTP server or manually set the date and time based on the timezone: Asia/Tehran.
3. Enable Logging in the IIS service as described in this document.
4. Verify that logs are being generated in the C:\inetpub\logs\LogFiles directory.
5. Install the NXLog software.
6. Configure NXLog as described in this document (including the Retention/Rotation section).
7. Start the NXLog service using the Command Prompt with Administrator privileges:
```
net start nxlog
```

8. Check the logs generated by the NXLog service in the C:\Program Files\nxlog\data\ directory to ensure the service is functioning correctly.
9. Troubleshoot any errors related to the NXLog service using the logs generated in the previous step.
10. Run the tcpdump command on the Log collector machine with the destination port 5527/UDP to ensure that IIS logs are being received:
```
tcpdump -n -i any dst port <optional port> and host <IIS_server_IP> -A
```
Replace <IIS_server_IP> with the IP address of the IIS server.

11. If the IIS server is sending logs but the Log collector machine is not receiving them, it may indicate a routing issue or a blocked connection by an intermediary firewall.







