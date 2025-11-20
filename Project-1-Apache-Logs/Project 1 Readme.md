**# Project 1: Basic Apache Web Server Log Analysis**



\## Introduction

In this project, I analyzed Apache web server logs to understand web traffic, identify potential security incidents, and summarize activity patterns. Apache logs provide valuable information such as IP addresses, request timestamps, HTTP methods, URLs, status codes, and user agents.



**## Exercises and Screenshots**



\### Screenshot 0: Apache Installation and Log Directory

Shows that Apache is installed, running, and the log files exist in `/var/log/apache2/`.



apache2 -v                # Displays Apache version and build information

sudo systemctl start apache2   # Starts Apache server

sudo systemctl enable apache2  # Ensures Apache starts on boot

sudo systemctl status apache2  # Shows Apache service status

ls -l /var/log/apache2/       # Lists all files in Apache log directory



\### Screenshot 1: Viewing Access Logs

less access.log   # Opens the access log in a scrollable view





The access log contains entries of all HTTP requests to the server. Example log entry:



::1 - - \[19/Nov/2025:18:43:45 -0500] "GET / HTTP/1.1" 200 3460 "-" "Mozilla/5.0 ..."





* ::1 → IP address of requester (localhost in this case)



* \[19/Nov/2025:18:43:45 -0500] → Timestamp of the request



* "GET / HTTP/1.1" → HTTP method, requested URL, protocol



* 200 → HTTP status code (OK)



* 3460 → Response size in bytes



* "Mozilla/5.0 ..." → User agent



\### Screenshot 2: Filtering Logs by IP or Status Code

grep '192.168.1.100' access.log           # Filters entries for a specific IP address

grep ' 404 ' access.log                    # Filters entries with HTTP status code 404 (Not Found)

grep '192.168.1.100' access.log | grep ' 404 '   # Combines filters for specific IP and 404 errors





grep searches text patterns in files. Combining grep allows more specific filtering.

* There were no files containing the given IP address, therefore it rendered no results. But, there was one event with a 404 error.



\### Screenshot 3: Example of a 404 Error



Shows a filtered 404 error from the access log:



::1 - - \[19/Nov/2025:18:43:45 -0500] "GET /favicon.ico HTTP/1.1" 404 487 ...





Confirms the ability to isolate error traffic.



\### Screenshot 4: Analyzing Error Logs

less error.log   # Opens the Apache error log to review server errors





Error logs show issues like startup notices, file not found errors, or permission issues. Example entry:



\[Wed Nov 19 18:32:09.301711 2025] \[mpm\_event:notice] \[pid 1410] AH00489: Apache/2.4.58 (Ubuntu) configured





\[mpm\_event:notice] → Module logging the event



AH00489 → Apache-specific message code



\### Screenshot 5: Summarizing Log Data

awk '{print $1}' access.log | sort | uniq -c | sort -nr

\# Counts number of requests from each IP address



awk '{print $4}' access.log | cut -d: -f1 | sort | uniq -c

\# Counts requests per day



awk '{print $7}' access.log | sort | uniq -c | sort -nr

\# Counts requests per URL





* awk '{print $1}' → Extracts the 1st column (IP addresses)



* awk '{print $4}' → Extracts the 4th column (timestamps)
* awk '{print $7}' → Extracts the 7th column (URL)



* cut -d: -f1 → Cuts the timestamp to just the date portion



* sort → Sorts the output alphabetically or numerically



* uniq -c → Counts repeated entries



* sort -nr → Sorts numerically in reverse (highest first)



* This gives a summary of traffic by IP, by date, and by requested URL.



**Summary**



By completing this project, I've learned how to:



* Access Apache log files



* Understand the structure of log entries



* Filter logs using grep



* Analyze error logs



* Summarize logs with awk for insights into traffic patterns
