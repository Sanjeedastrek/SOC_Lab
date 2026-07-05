Let's classify detected attack behaviour using MITRE ATT&CK to standardize findings and improve reporting.

Defense Impairment 
In this lab, we observed the tactic of Defense Impairment (TA0112) by turning off Windows Defender’s real-time protection before running the payload. This behavior maps to MITRE ATT&CK technique T1685: Disable or Modify Tools. By turning off Defender, we bypassed the system’s built-in security controls and execute the malicious PS script without being blocked or detected by Windows Defender, leading us to the next tactic Execution.

Execution
Running a malicious PS script on the Windows system to establish a reverse connection. aligns with MITRE ATT&CK tactic Execution (TA0002), technique Command and Scripting Interpreter (T1059), and sub-technique Command and Scripting Interpreter: PowerShell (T1059/001). PowerShell was used as the command-line interpreter to execute the code that initiated the connection back to the attacker’s machine, leading us to the next tactic Command and Control.

Command and Control
The compromised Windows system made an outbound network connection to the attacker’s Kali Linux machine on port 4444,  corresponding to MITRE ATT&CK technique Command and Control (TA0011), technique Application Layer Protocol (T1071) and sub-technique Application Layer Protocol: Web Protocols (T1071/001). The PS process used a standard TCP connection to communicate with the attacker’s server, allowing remote control of the target system.

Because we've access to the system as owners, we can't add the Initial Access tactic. However, in real life this tactic could be the first place where MITRE mapping would make sense. Not every technique has been broken down into sub-techniques because some methods are straightforward or broad enough that a single classification is enough.

#MITRE