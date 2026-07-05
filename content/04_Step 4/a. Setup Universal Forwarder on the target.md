
Download from the official site & install the forwarder in the target VM using the same account you used for downloading Splunk Enterprise.

After installation is complete, check whether the forwarder is installed. In order to do that, go to C:\Program Files\SplunkUniversalForwarder\bin, open CMD as Administrator and run splunk status. If you can see the process ID, you know that it is running.

>[!NOTE] 
>During this installation I came across 2 choices, using deployment server or using receiving indexer. Receiving Indexer: The forwarder sends data directly to a specific indexer using its IP address and port (default 9997). This is a simple, straightforward setup, best for when you have just one or a few forwarders. Configuration stays local on the forwarder itself.
>Deployment Server: A centralized tool that pushes configurations out to multiple universal forwarders automatically. All the forwarders pull their configs from this one server. Best for managing many forwarders.

#UniversalForwarder #Splunk

