In Splunk, inputs.conf and outputs.conf are the ==core configuration files used to ingest and route data==. The `inputs.conf` file defines what data to collect and how to label it, while `outputs.conf` defines where to send that collected data (usually to a Splunk Indexer or heavy forwarder). 

Use the following code in inputs.conf file under "C:\Program Files\SplunkUniversalForwarder\etc\system\local" folder:

```[WinEventLog://Microsoft-Windows-Sysmon/Operational]
disabled = false
index = main
sourcetype = XmlWinEventLog:Microsoft-Windows-Sysmon/Operational
renderXml = true
current_only = 0
```

Paste it in outputs.conf in the same location:

```[tcpout] 
defaultGroup = splunk_indexer 
[tcpout:splunk_indexer] 
server = 192.168.56.1:9997
[tcpout-server://192.168.56.1:9997]
```
#Microsft #inputs.conf #outputs.conf