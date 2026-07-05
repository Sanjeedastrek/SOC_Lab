
1. Check if agent processes are running: `Get-Process *agent*`
2. If running, check overall agent status: 
```
& "$env:ProgramW6432\AzureConnectedMachineAgent\azcmagent.exe" show
```
    
& = execute the following command
$env:ProgramW6432 = Program Files directory path (64-bit)
\AzureConnectedMachineAgent\azcmagent.exe = the Azure agent executable
show = display the agent's status and configuration

So it is, "Run the Azure agent program and show me its status." or just simply use this if you know the full path:  
```
& "C:\Program Files\AzureConnectedMachineAgent\azcmagent.exe" show 
```

  I got the following output:

<img src="Assets/Step_5_azcmagent.png" style="border: 2px solid red; padding: 10px;border-radius:20px">

3. Look at the output for "Dependent Service Status" — if any service shows "stopped," that's the problem.
4. Restart the stopped service(s).
5. Verify logs are coming to the workspace. Sentinel uses UTC timezone, which is different from ours. When comparing logs across systems with different timezones, always calculate the offset carefully.
   
**<img src="Assets/Step_5_Sentiel_Logs.png" style="border: 2px solid green; padding: 10px;border-radius:20px;"><center>This shows where logs are expected to be. Event is the name of the table</center>

#Sentinel