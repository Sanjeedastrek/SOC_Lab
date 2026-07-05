Lets create an SPL and save it for a smoother experience. Notably, my indexer is following my local time. Use:  

```
index="main" earliest=06/16/2026:12:33:00 latest=06/16/2026:12:34:59
```

Save it as a report (I named it as report1), which is the standard way to save a search result in Splunk. Then, open Reports under Search & Reporting, select the correct report & click it to see results.

<img src="Assets/Step_7_Splunk.png" style="border: 2px solid green; padding: 10px;border-radius:20px;">**
<center>In total, 16 rows of info retrieved</center>

Now, create a KQL query and export the output as CSV. Use UTC time to match Sentinel's timezone standard.:

```
Event
| where TimeGenerated >= datetime(2026-06-16T17:33:00Z) 
  and TimeGenerated <= datetime(2026-06-16T17:34:59Z)
```
  

**<img src="Assets/Step_7_Sentinel.png" style="border: 2px solid green; padding: 10px;border-radius:20px;">
<center>Like Splunk, Sentinel also retrieved 16 rows of info</center>

#Splunk #Sentinel