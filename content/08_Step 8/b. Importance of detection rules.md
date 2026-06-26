
We analyzed a few logs to determine the MITRE mapping, but in the real world, you deal with thousands of such logs, and it's impossible to analyze them one by one. Generally, SOC people depend on detection rules to signal them of a possible malicious attack.

According to Microsoft:

>[!NOTES]
>After setting up Microsoft Sentinel to collect data from all over your organization, you need to constantly dig through all that data to detect security threats to your environment. To accomplish this task, Microsoft Sentinel provides threat detection rules that run regularly, querying the collected data and analyzing it to discover threats. These rules come in a few different flavors and are collectively known as analytics rules.
These rules generate alerts, containing information about the events detected, such as the entities (users, devices, addresses, and other items) involved. Alerts are aggregated and correlated into incidents—case files—that you can assign and investigate to learn the full extent of the detected threat and respond accordingly. You can also build predetermined, automated responses into the rules' own configuration.

  In this project, we'll not go into importing detection rules from Sentinel/Splunk or creating them from scratch. They are available on GitHub, and can be used with or without tweaking to match specific needs.
