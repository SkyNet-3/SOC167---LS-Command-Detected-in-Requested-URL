<h1>SOC167 - LS Command Detected in Requested URL</h1>


<h2>Description</h2>
This lab reviews a possible CMDi payload alert triggered by	SOC167 - LS Command Detected in Requested URL. The HTTP request seems to contain a modified version of the OS command "ls" in the URL parameter. This OS command is typically harmless but commonly used by attackers as a probing technique to determine whether a web application is vulnerable to command injections. Based on the information in the alert, the input "?s=" is most likely what triggered the security rule.At first glance, this could potentially be a false positive, but further analysis is needed to determine whether any real malicious activity occurred against the web application.
<br />


<h2> Utilities Used</h2>

- <b>LetsDefend SIEM</b> 
- <b>LetsDefend Email Security</b>
- <b>LetsDefend EDR</b>
- <b>VirusTotal</b>

<h2>Environments Used </h2>

- <b>Windows 10</b> (22H2)

<h2>Alert walk-through:</h2>

<p align="center">
Go to investigation channel in Monitoring to get the context of the alert: <br/>
<img src="https://i.imgur.com/XT01Vkd.jpeg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
After taking ownership of the case, very simply look at the rule name and examine the direction of the traffic thats occuring between both devices:  <br/>
<img src="https://i.imgur.com/JWl6L2J.jpeg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
The specific pattern/signature that the rule was detecting seems to be "?s", which looks very close to the "ls" command that triggered the alert. There are also several outbound HTTPS requests being to Letsdefend.io made from an Internal IP address that belongs to ElliotPRD : <br/>
<img src="https://i.imgur.com/PDzfE8J.jpeg" height="80%" width="80%" alt="Disk Sanitization Steps"/><img src="https://i.imgur.com/wRFSJbu.jpeg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Cloudflare is the owner. Traffic isnt coming from outside. It is a Pool addresses and its a web hosting cloud provider. IP address has a positive rating by security vendors on VT. Traffic is coming from company network, its internal outbound traffic. Hostname: ElliotPRD The owner is webadmin15 (admin account) last login: Feb, 27, 2022, 12:00 AM: <br/>
<img src="https://i.imgur.com/AcLpWk1.jpeg" height="50%" width="50%" alt="Disk Sanitization Steps"/><img src="https://i.imgur.com/3daIksc.jpeg" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/YWiK3dB.jpeg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Time to examine the supposed CMDi payload embedded in the URL parameter, as well as all fields within the HTTP request:  <br/>
<img src="https://i.imgur.com/RjCFTPV.jpeg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
After taking a closer look at each request, they dont appear to look malicious. Just normal web request being made to the server  <br/>
<img src="https://i.imgur.com/jLo3FeV.jpeg" height="50%" width="50%" alt="Disk Sanitization Steps"/><img src="https://i.imgur.com/BMwIiFT.jpeg" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/kZs3AP2.jpeg" height="50%" width="50%" alt="Disk Sanitization Steps"/><img src="https://i.imgur.com/ZUGv06s.jpeg" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<br />
<br />
The traffic was not malicious because it did not contain a CMDi payload specified in the alert:  <br/>
<img src="https://i.imgur.com/sCujGob.jpeg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
There was different traffic being sent to the same server between 12:01 AM to 12:37 AM by the same user as seen earlier in the Log Management and EDR:  <br/>
<img src="https://i.imgur.com/DXuHnhP.jpeg" height="50%" width="50%" alt="Disk Sanitization Steps"/><img src="https://i.imgur.com/wRFSJbu.jpeg" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/RsgNsi4.jpeg" height="50%" width="50%" alt="Disk Sanitization Steps"/><img src="https://i.imgur.com/yPNPHo8.jpeg" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<br />
<br />
Again, none of The other traffic created by the user were malicious either:  <br/>
<img src="https://i.imgur.com/sCujGob.jpeg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
write your analyst notes from the case. click confirm and youre finished! Next closing the case..:  <br/>
<img src="https://i.imgur.com/Ko3O89O.jpeg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Lastly, the alert triggered was a False Positive because the security system incorrectly identified this as an ls payloads injection being delivered to the web server :  <br/>
<img src="https://i.imgur.com/WmQCfs4.jpeg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
</p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
