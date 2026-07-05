Now we'll try to create communication between our attacker (Kali) and target (Windows) machine. We'll  do it using pure PowerShell code, without obfuscation. But before doing so, we have to disable Defender to enable the code execution. I'm not describing commands or process for this. I also noted the timeframe of these events for filtering them easily while analyzing logs later.

<img src="Assets/Step_6_PS.png" style="border: 2px solid green; padding: 10px;border-radius:20px;">
<center>I just copied this code from revshell.com</center>

<img src="Assets/step_6_revshell.png" style="border: 2px solid green; padding: 10px;border-radius:20px;">
<center>We got a reverse shell effortlessly. I also ran whoami and hostname command in this shell</center>

