Search Data Collection Rule (DCR) in the search space, & create a new DCR. I named the rule as AMA_rule1 (AMA stands for Azure Monitor Agent)

  During this, mention basics (region, name etc), resources (in this case, Azure Arc), collect and deliver (Microsoft-Windows-Sysmon/Operational as log name under Data Source, and the previously created  sentinel workspace as destination). Then review & create.

>[!NOTE]
> We don't need endpoint. A Data Collection Endpoint (DCE) is primarily used if your VM needs to send custom logs via an API, or if your VM is isolated and needs a specific private link setup to talk to Azure. Because we are using the standard Azure Monitor Agent to collect standard Windows, the agent can send the data directly to your Log Analytics workspace without needing a custom endpoint.

At last, the DCR was created successfully. Remember, the rule is found under Monitor:

<img src="Assets/Step_5_dcr.png" style="border: 2px solid green; padding: 10px;border-radius:20px;">
