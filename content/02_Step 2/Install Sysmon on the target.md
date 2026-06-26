
Download it from the Sysinternals site, unzip it, and open CMD as Admin. You may install it with the default config, but it is better to use a special config known as SwiftOnSecurity, so download SwiftOnSecurity, and install Sysmon with its config. In my system, Sysmon is in the Downloads folder; you should keep the config file in the same location.

`sysmon.exe -accepteula -i sysmonconfig-export.xml

To check whether Sysmon is collecting logs, open Event Viewer, then go to Applications and Services Logs > Microsoft > Windows > Sysmon > Operational. You can also clear them by right-clicking on Operational and choosing Clear Log. You may want to control the log size. Open Properties by right-clicking on Operational, and control it from there.
