### <center> Incident Response </center>
Recently I helped with an incident response and had to spend a few days digging through windows event logs, so I thought it would be helpful to capture some of what I learned.  The purpose of this repository is to show some different types of events and what logs show these events.  Collecting these logs across an enterprise is a different dicsussion that will not be covered.


#### SMB Brute Force Login
By default windows security log does not log failed login attempts, so we need to look else where to find these events.  The Microsoft-Windows-SMBServer/Security log will show these failed attempts with event id 551.</br>
Here is a view of the log after running a Hydra brute force smb attempt.</br>
![alt text](https://github.com/defendthehoneypot/incidentresponse/blob/master/images/smbserver-security-log-list.png "SMBServer Security Log List")</br>
</br>
Here is the individual log that shows the attacking IP.</br>
![alt text](https://github.com/defendthehoneypot/incidentresponse/blob/master/images/smbserver-security-log.png "SMBServer Security Log")</br>
</br>
#### RDP Brute Force Login
There is not a specific event that will show the failed login event, but the Microsoft-Windows-TerminalServices-RemoteConnectionManager/Operational does show a bunch of received connection attempts with event id 261.</br>
Here is a view of the log after running a Crackmapexec brute force rdp attempt.</br>

