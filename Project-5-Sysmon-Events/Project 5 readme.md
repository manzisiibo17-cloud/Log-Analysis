**Project 5 — Windows Sysmon Event Analysis**



This project documents my setup and analysis of Windows Sysmon logs for security monitoring. Each section includes screenshots showing the log filtering and event details.



**What Each Sysmon Event ID Represents**



Event ID 1 — Process Creation

Logs every time a process starts. Includes parent process, command line, hashes, etc. Extremely valuable for detecting malware execution,  and suspicious command lines.



Event ID 3 — Network Connection

Logs outbound TCP connections. Used to identify unknown IP communication, C2 traffic, lateral movement, or beaconing behavior.



Event ID 11 — File Creation

Logs when a file is created or overwritten. Useful for spotting malware drops, script deployment, or persistence files in unusual directories.



Event ID 7 — Image Load

Logs when a process loads a DLL or executable image. Critical for catching DLL injection, unsigned DLL loads, or suspicious side-loading behavior.



**Why Event IDs 11 and 7 Showed 0 Events**



Event ID 11 (File Creation)

Sysmon only logs file creations that match rules in the configuration file.

The SwiftOnSecurity config reduces noise, so normal user-level file creations (like saving screenshots, text files, documents) often do not generate Event ID 11 logs unless they occur in high-risk directories (e.g., system folders, temp folders used by malware).



Event ID 7 (Image Load)

Image load logging depends on driver behavior and config rules.

Sysmon will only log DLL loads for:



\- unsigned DLLs



\- DLLs from suspicious directories



\- DLLs matching specific rules



During this project, no DLLs matching these conditions were loaded, so Event ID 7 showed 0 events, which is normal on a clean system.



**Screenshots**

Screenshot 0 — Sysmon Installation



Command used: sysmon -accepteula -i sysmonconfig-export.xml



Confirms Sysmon installed with the configuration file.



Image: images/screenshot0.png



**Exercise 1 — Verify Sysmon Installation**

Screenshot 1 — Sysmon Operational Log Visible



Event Viewer → Applications and Services Logs → Microsoft → Windows → Sysmon → Operational



Confirms Sysmon is generating logs.



Image: images/screenshot1.png



**Exercise 2 — Process Creation (Event ID 1)**

Screenshot 2 — Filter Applied for Event ID 1



Shows filtering for Sysmon Event ID 1.



Image: images/screenshot2.png



Screenshot 3 — Event ID 1 Details



Displays process creation details, including process name, parent process, and command line.



Image: images/screenshot3.png



**Exercise 3 — Network Connections (Event ID 3)**

Screenshot 4 — Filter Applied for Event ID 3



Shows filtering for Sysmon Event ID 3.



Image: images/screenshot4.png



Screenshot 5 — Event ID 3 Details



Displays source/destination IP + process responsible for the connection.



Image: images/screenshot5.png



**Exercise 4 — File Creation (Event ID 11)**

Screenshot 6 — Event ID 11 (0 Events)



No file creation events recorded (expected with the SwiftOnSecurity config).



Screenshot still included to document proper filtering.



Image: images/screenshot6.png



**Exercise 5 — Image Load (Event ID 7)**

Screenshot 7 — Event ID 7 (0 Events)



No matching DLL loads occurred.



Screenshot included to properly document the exercise.



Image: images/screenshot7.png



**Completion**



&nbsp;	This project demonstrates:



&nbsp;		- Installing and configuring Sysmon



&nbsp;		- Navigating Sysmon logs



&nbsp;		- Filtering and analyzing critical Sysmon events



&nbsp;		- Understanding when and why certain events do not appear



&nbsp;		- Documenting all findings clearly for a SOC portfolio

