
The most important event observed was Event ID 3 (Network Connection). In both Splunk and Sentinel, there was a clear malicious network connection from powershell.exe (source IP was 192.168.56.131) to the Kali machine at IP 192.168.56.130 on port 4444. This is the reverse shell callback and serves as the strongest evidence of the attack in this lab. 

Watch for SourceIp and DestinationIp under the EventData column in Sentinel's output
```
<DataItem Type="System.XmlData" time="2026-06-16T17:34:02.8870742Z" sourceHealthServiceId="1cc3bd45-c9c2-4588-8db8-f6295de382d4"><EventData xmlns="http://schemas.microsoft.com/win/2004/08/events/event"><Data Name="RuleName">-</Data><Data Name="UtcTime">2026-06-16 17:34:01.113</Data><Data Name="ProcessGuid">{22d8bf06-7f61-6a31-b302-000000001500}</Data><Data Name="ProcessId">9948</Data><Data Name="Image">C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe</Data><Data Name="User">Win11_SoC\6543s</Data><Data Name="Protocol">tcp</Data><Data Name="Initiated">true</Data><Data Name="SourceIsIpv6">false</Data><Data Name="SourceIp">192.168.56.131</Data><Data Name="SourceHostname">Win11_SoC</Data><Data Name="SourcePort">64134</Data><Data Name="SourcePortName">-</Data><Data Name="DestinationIsIpv6">false</Data><Data Name="DestinationIp">192.168.56.130</Data><Data Name="DestinationHostname">-</Data><Data Name="DestinationPort">4444</Data><Data Name="DestinationPortName">-</Data></EventData></DataItem>
```

Event ID 1 (Process Creation) was also significant but secondary. Multiple powershell.exe processes were created, with the most relevant one being the PowerShell instance that later initiated the outbound connection. Additionally, whoami.exe and hostname.exe were executed from PowerShell, indicating post-exploitation enumeration activity by the attacker.

**<img src="Assets/Step_7_event_1.png" style="border: 2px solid green; padding: 10px;border-radius:20px;">**

Overall, the main focus is on the Event ID 3 network connection as it clearly demonstrates Command and Control behaviour. Event ID 1 provides useful supporting context showing how the shell was executed.

