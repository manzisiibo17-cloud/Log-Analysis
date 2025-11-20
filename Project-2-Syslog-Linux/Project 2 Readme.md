**Project 2 – Introduction to Syslog Analysis on Linux Systems**



This project demonstrates the fundamentals of syslog analysis on Linux systems.

Syslog is the primary logging system used by Linux to record system messages, authentication events, kernel activity, and more.

Throughout this project, I practiced navigating log directories, filtering log data, analyzing authentication activity, and summarizing log patterns.



**Exercise 1: Understanding Syslog Configuration**



File viewed:



sudo nano /etc/rsyslog.conf





**What I did:**



Opened the rsyslog configuration file.



Reviewed modules such as:



imuxsock (local system logging)



imklog (kernel message logging)



Observed optional UDP/TCP syslog server configurations (commented out).



What I learned:



rsyslog loads modules to receive messages from the kernel, system processes, and remote sources.



Logging rules are generally stored in /etc/rsyslog.d/50-default.conf.



Facilities and severity levels are defined here and determine where logs are written (e.g., /var/log/syslog, /var/log/auth.log).



**Exercise 2: Accessing Syslog Files**



Commands used:



cd /var/log

ls -l

less syslog





What I did:



Navigated to the main log directory.



Listed system log files such as:



syslog



auth.log



kern.log



dpkg.log



apache2/



journal/



Opened syslog using less to view system messages such as boot events, detected hardware, systemd logs, etc.



What I learned:

The /var/log directory stores logs for system processes, user authentication, services like Apache, and kernel activity.



**Exercise 3: Filtering Syslog Entries**



Commands used and what they do:



grep 'Nov 12' syslog





Searches for entries containing the date Nov 12.



grep 'sshd' syslog





Filters for SSH daemon activity (logins, failed attempts, service start).



grep 'Nov 12' syslog | grep 'systemd'





Combines searches to show systemd events on Nov 12.



What I learned:



grep is essential for isolating log entries based on patterns.



Useful for incident response, filtering by date, service, or severity.



**Exercise 4: Analyzing Authentication Logs**



Commands used:



less auth.log





Viewed authentication logs which included:



User creation (useradd, groupadd)



Sudo activity



Login sessions



PAM (Pluggable Authentication Modules) messages



grep -i 'failed' auth.log





Filters failed authentication attempts (none in this case).



grep -i 'sudo' auth.log





Shows all sudo-related actions, including:



Opening/closing sudo sessions



Commands run as root



Group membership changes



What I learned:

auth.log is crucial for detecting unauthorized access, brute force attacks, and privilege escalation attempts.



**Exercise 5: Summarizing Log Data**

* 1\. Count sudo actions by timestamp

awk '/sudo/ {print $1}' auth.log | sort | uniq -c | sort -nr





Explanation:



Finds all sudo events



Prints the timestamp (field 1)



Counts occurrences of identical timestamps



My output:

Each timestamp had a count of 1, indicating many sudo commands, each executed at a unique time.



* 2\. Summarize sudo sessions by user

awk '/sudo/ {print $9}' auth.log | sort | uniq -c | sort -nr





Explanation:



The 9th field typically includes the target user or UID



This gives the number of times sudo escalated to root



My output:

root(uid=0) appeared 19 times, meaning sudo was used frequently for root-level changes.



* 3\. Identify the most common syslog message types

awk '{print $6}' syslog | sort | uniq -c | sort -nr





Explanation:



Prints the 6th field of each syslog entry

(often the service name, tag, or key term)



Counts the frequency



My output included frequent terms like:

GID, systemd, connecting, could, detected. and others.



This indicates heavy system activity from:



systemd services



kernel hardware detection



networking components



**Project Summary**



By completing this project, I learned:



How Linux’s syslog system collects and stores logs



How to navigate and read /var/log files



How to filter logs using grep



How to analyze authentication logs for security events



How to summarize log patterns using awk, sort, and uniq



These skills are foundational for SOC analysis, threat hunting, Linux administration, and incident response.

