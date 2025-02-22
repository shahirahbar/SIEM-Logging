## Kaspersky Configuration

As shown in the figure below, in **Configure notifications and event export**, select the **Configure export to SIEM systems** option to send logs.

**Note:** In the **Advanced** section, you must select **Advanced** or **Total** (F) for sending logs (logs are not sent for basic configurations).

In this section, perform the following steps:

- ✔ Check the **Automatically export events to SIEM system database** option.
- In the **SIEM system** section, select **Splunk (CEF format)**.
- In the **SIEM system server address** section, enter the **IP address** of the NBA server.
- In the **SIEM system server port** section, enter the **port 5526** used by Kaspersky.
- Finally, select **UDP** as the protocol and click **OK**.
