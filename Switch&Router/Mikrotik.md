# Mikrotik Ruters
To configure logging on Mikrotik devices, follow these steps in the **Winbox** program:

1. Navigate to **System > Logging**.
2. In the **Rules** tab, select the following **Severity levels** for sending logs to the SIEM:
   - `critical`
   - `error`
   - `info`
   - `warning`
3. In the **Action** tab, enter the following information for the **Syslog Server**:
   - **Remote Address**: Enter the IP address of the **SIEM** machine.
   - **Remote Port**: Enter the port for Mikrotik logs, which is `optional port`.
   - Check the **BSD Syslog** option.
   - **Syslog Facility**: Select `local0`.
   - **Syslog Severity**: Select `info`.
