# The importance of logging in SOC

One of the main steps in setting up a SOC is implementing SIEM, and the most critical and important step in implementing SIEM is receiving logs correctly with prioritization.

Many people have a misconception about logging, which is that all logs must be received from the very beginning of starting a SOC, and otherwise, the service cannot be initiated. However, in my opinion, this is a misconception. Although it would be very beneficial to have all logs from day one, we all know that this is not feasible for various reasons. Therefore, I always recommend that if it is not possible to receive all logs at once (which will always be the case), logging should be done in stages, and logs should be received based on their priority and importance in the SOC. My suggestion in this regard is to categorize the required logs into three stages, and when 90% of the logs in stage 1 are received, the SOC service can be initiated. This proposed categorization is as follows. I have also tried to include logging instructions for equipment in this Repo.

## Stage 1:

This stage includes important and security-related assets, and by having their logs, most general incidents in the organization can be observed. In my opinion, the following items must be logged before the SOC is initiated.

- SPAN
- Security tools:
  - [Firewall](https://github.com/shahirahbar/SIEM-Logging/tree/main/firewall%26WAF)
  - WAF
  - [Antivirus](https://github.com/shahirahbar/SIEM-Logging/tree/main/AV%26EDR)
  - EDR
- Active directory
- Publish applications (only OS)
- Critical company services:
  - OS
  - [Service](https://github.com/shahirahbar/SIEM-Logging/tree/main/Webservice)
  - DB

**note:** The term **Critical Company Services** in this step refers to assets whose **CIA** is between 4 and 5.

**note:** If **Threat Modeling** has been done for the organization, priority should be given to the tools specified in the threat model.

## Stage 2:

The items in this stage should be logged within the first 2 months of setting up the SOC and are essential for the maturity of stage 1.

- [Network devices](https://github.com/shahirahbar/SIEM-Logging/tree/main/Switch%26Router)
- All operation systems
- High company services:
  - OS
  - [Service](https://github.com/shahirahbar/SIEM-Logging/tree/main/Webservice)
  - DB

The term **High Company Services** in this step refers to assets whose **CIA** is between 4 and 3.

## Stage 3:

**note:** This stage should also be completed by the end of the first 4 months of setup.

- All databases
- Network services
- Medium company services:
  - OS
  - Service

**note:** The term **Medium Company Services** in this step refers to assets whose **CIA** is less than 3.
