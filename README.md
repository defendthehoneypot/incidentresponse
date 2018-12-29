### <center> Incident Response </center>
Recently I helped with an incident response and had to spend a few days digging through windows event logs, so I thought it would be helpful to capture some of what I learned.  The purpose of this repository is to show some different types of events and what logs show these events.  These events were collected from a Windows 10 LTSB, non-domain joined system.


#### SMB Brute Force Login
By default windows security log does not record failed login attempts, so we need to look else where to find these events.  Domain Controllers do capture these events.  The Microsoft-Windows-SMBServer/Security log will show these failed attempts with event id 551.</br>
Here is a view of the log after running a Hydra brute force smb attempt.</br>
![](https://github.com/defendthehoneypot/incidentresponse/blob/master/images/smbserver-security-log-list.png "SMBServer Security Log List")</br>
</br>
Here is the individual log that shows the attacking IP.</br>
![](https://github.com/defendthehoneypot/incidentresponse/blob/master/images/smbserver-security-log.png "SMBServer Security Log")</br>
</br>
#### RDP Brute Force Login
There is not a specific event that will show the failed login event, but the Microsoft-Windows-TerminalServices-RemoteConnectionManager/Operational does show a bunch of received connection attempts with event id 261.</br>
Here is a view of the log after running a [Crackmapexec](https://github.com/byt3bl33d3r/CrackMapExec/wiki/Using-Credentials) brute force rdp attempt.</br>
![](https://github.com/defendthehoneypot/incidentresponse/blob/master/images/terminal-services-remote-connection-manager-list.png "TerminalServices RemoteConnection Manager Log")</br>
Here is the individual log.</br>
![](https://github.com/defendthehoneypot/incidentresponse/blob/master/images/terminal-services-remote-connection-manager.png "TerminalServices RemoteConnection Manager Log")</br>
</br>
Additionally, you can look at the Security log for event id 4624 as an anonymous login.  This event will show the connecting IP.  It will be immediately followed by event id 4634, account logoff.</br>
![](https://github.com/defendthehoneypot/incidentresponse/blob/master/images/security-4624-anonymous.png)</br>
</br>
Once we see these RDP connection attempts stop, look for successful logins in the Security log using event id 4624.  Another option is to look in the Microsoft-Windows-TerminalServices-LocalSessionManager/Operational log for a Remote Desktop Services: Session logon succeeded event id 21.</br>
![](https://github.com/defendthehoneypot/incidentresponse/blob/master/images/terminal-services-localsessionmanager.png "Terminal Services Local Session Manager")</br>
</br>
#### Commands run on system
Now we need to determine what commands where run on the system.  Lets check what was executed using "Run".  Search HKEY_USERS for runMRU.</br>
![](https://github.com/defendthehoneypot/incidentresponse/blob/master/images/registry-runmru.png "runMRU")</br>
</br>
Something else that can be determined is what programs were launched by searching in the registry.  HKEY_USERS\SID of User\SOFTWARE\Microsoft\Windows NT\CurrentVersion\AppCompatFlags\Compatibility Assistant\Store</br>
![](https://github.com/defendthehoneypot/incidentresponse/blob/master/images/programs-run.png "Programs Run")</br>
</br>
While there is no command line log, there is one for Powershell, but really only provides useful information if logging is turned on.  About the only event you can obtain is Powershell being started.  Microsoft-Windows-PowerShell/Operational event id 40961 will show when the Powershell console is launched.</br>
![](https://github.com/defendthehoneypot/incidentresponse/blob/master/images/powershell-startup.png "PowerShell Startup")</br>
</br>
#### Installation of programs
Look for installation of windows features using the Setup log with event id 9.</br>
![](https://github.com/defendthehoneypot/incidentresponse/blob/master/images/install-features-tftp.png "Install Features")</br>
</br>
#### Download files
Downloading of files can be accomplished several different ways.  One method is using bitsadmin.  This event can be found in Microsoft-Windows-Bits-Client/Operational event id 59 start download event.</br>
![](https://github.com/defendthehoneypot/incidentresponse/blob/master/images/bitsadmin-download.png "Bitsadmin Download")</br>
</br>
