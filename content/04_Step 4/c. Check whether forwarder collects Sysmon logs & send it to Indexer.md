Faced a whole lot of difficulties in getting logs in Splunk Enterprise.

**<img src="Assets/Step_4_null_splunk.png" style="border: 2px solid green; padding: 10px;border-radius:20px;">**
<center>After forwarder and config files setup, still no result in Splunk</center>
  
Communication between the Windows target VM and host (where we'll analyze logs):
Sysmon -> inputs.conf -> Universal Forwarder -> outputs.conf -> Indexer:9997

***Troubleshooting steps***

1. Is sysmon enabled & working? (Yes, I found current logs in event viewer.)
2. Is splunkd service running as it determines whether splunk enterprise can receive logs?(`tasklist | findstr splunk`)
3.  Splunkd has receiving port configured? (in enterprise select settings or gear data > forwarding and receivng > receive data > configure receiving) 
4. Is Forward-server active/running in VM?  (in splunkuniversalforwarder\bin folder cmd: `splunk list forward-server`)
    
	4 a. If forwarder runs, check whether forwarder can reach the host/port where we want logs to forward to: (run in ps: `test-netconnection 192.168.56.1 -port 9997`)

	It reveals several things: 
	ping works?  port reachable?
	If ping works but tcptestcsucceeded is false, it means the host is reachable but either a firewall is blocking the port or the port is not open/active to receive connections.

	4 b. Is the firewall blocking? 
	Open Defender with advanced security > inbound rules > under local port check if 9997 appears.
	
	4 c. It is Allow or Block in profiles?
	open Defender advanced security properties to see different profile settings. In my case, all of domain/private/public/ipsec profile's inbound connections is block. So, let's create an inbound rule to outrule this. I created a rule named Splunk Forwarder in my Windows host machine. 
	
5. Is forwarder collecting sysmon logs?
    
	5 a. is forwarder's inputs.conf is properly configured to collect logs?

6.  Is forwarder sending logs to Enterprise?  
	6 a. Does the forwarder know the IP & port to where it should send logs?(check outputs.conf)

	6 b. Does Enterprise know which port to use to listen to to retrieve those logs?(check forwarding and receiving in Enterrpise)  
	6 c. Can the target VM access the host VM? Do they have a shared adapter? (powershell test-netconnection)

	6 d. is forwarder's outputs.conf is properly configured to collect logs from inputs.conf?       

      6 e. Is the communication between inputs.config & outputs.config are seamless?

7. Can Enterprise search logs? (index="main" reveals anything is Search & Reporting?)

***After Everything checking, at last found the real issue

  Although the user in the VM is under Administrative group, the splunkforwarder service is not running as that same user, rather it is a virtual service account: user NT Service\SplunkForwarder 

  Let's check splunkd.log using this command and then follow the output: 

`Get-Content "C:\Program Files\SplunkUniversalForwarder\var\log\splunk\splunkd.log" | Select-String -Pattern "permission|denied|access|sysmon" -CaseSensitive

 ```**

06-04-2026 12:33:44.334 -0400 ERROR ExecProcessor [1412 ExecProcessor]

- message from ""C:\Program

Files\SplunkUniversalForwarder\bin\splunk-winevtlog.exe"" splunk-winevtlog -

WinEventLogChannel::subscribeToEvtChannel: Could not subscribe to

Windows Event Log channel

'Microsoft-Windows-Sysmon/Operational'

06-04-2026 12:33:44.334 -0400 ERROR ExecProcessor [1412 ExecProcessor]

- message from ""C:\Program

Files\SplunkUniversalForwarder\bin\splunk-winevtlog.exe""

splunk-winevtlog - WinEventLogChannel::init: Init

failed, unable to subscribe to Windows Event Log channel

'Microsoft-Windows-Sysmon/Operational': errorCode=5

06-04-2026 12:53:18.760 -0400 INFO  WatchedFile [2280 tailreader0] -

File too small to check seekcrc,

probably truncated.  Will re-read entire file='C:\Program

Files\SplunkUniversalForwarder\var\log\splunk\splunkd_ui_access.log'.

06-04-2026 12:53:18.763 -0400 INFO  WatchedFile [2280 tailreader0] -

File too small to check seekcrc,

probably truncated.  Will re-read entire file='C:\Program

Files\SplunkUniversalForwarder\var\log\splunk\splunkd_access.log'.

 ``` 
 

The summary is "NT Service\SplunkForwarder" is a virtual service account that does not have permission to read the Sysmon log. 

Switching the Splunk Forwarder service to run under the LocalSystem account grants the service full access to all resources on that machine, including the Sysmon event logs. We're going to change service account to be NT AUTHORITY/SYSTEM:  
  
 1. Get the service object via WMI

```
$service = Get-CimInstance -ClassName Win32_Service -Filter "Name='SplunkForwarder'"
```

 2. Change the Splunk Forwarder service account to LocalSystem (Password must be $null for
LocalSystem)

```
Invoke-CimMethod -InputObject $service -MethodName Change -Arguments
@{ StartName = "LocalSystem"; StartPassword = $null }
```
 
3. Restart the service to apply changes in C:\Program Files\SplunkUniversalForwarder\bin
` .\splunk restart`

  But, the best practice is to grant only the permissions needed, in this case, permission to read the specific Sysmon event log channel, rather than giving the service total control. 
  
After giving the Forwarder sufficient privileges, it was able to read the Sysmon logs and successfully forward them to Splunk Enterprise:

**<img src="Assets/Step_4_Splunk.png" style="border: 2px solid green; padding: 10px;border-radius:20px;">
<center>This is how event logs appear in Splunk search as xml</center>

