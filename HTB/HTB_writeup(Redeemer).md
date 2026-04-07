🔐 Lab Write-Up: Redeemer (Hack The Box)
Date:07-04-2026
🎯 Objective
The goal of this lab was to identify open services on the target machine and interact with them to retrieve sensitive information (flag).
🔍 Reconnaissance
Initial scan using:
nmap -sC -sV <target-ip>
Result:
•	No open ports found in default scan (top 1000 ports)
👉 Insight:
This indicated that the service might be running on a non-standard port, requiring a full port scan.
⚡ Full Port Scan
Executed:
nmap -p- --min-rate=1000 -T4 <target-ip>
Result:
6379/tcp open redis
👉 Key Finding:
•	Redis service detected on port 6379
•	No HTTP service present
🧠 Service Enumeration
Connected to Redis using:
redis-cli -h <target-ip>
Checked server information:
INFO
👉 Observed:
•	Redis version and system details
•	No authentication required (misconfiguration)
📂 Data Extraction
Enumerated keys:
KEYS *
Result:
"numb"
"flag"
"temp"
"stor"
Retrieved flag:
GET flag
👉 Output:
03e1d2b376c37ab3f5319922053953eb
💣 Vulnerability Identified
•	Redis server exposed without authentication
•	Sensitive data accessible remotely
👉 Risk:
•	Unauthorized data access
•	Potential for further exploitation
🧨 Key Learning Points
•	Default scans are not enough → always consider full port scan
•	Not all targets have HTTP → adapt to detected service
•	Redis can be dangerous if misconfigured
•	Enumeration is more important than tools
🚀 Conclusion
The lab demonstrated the importance of:
•	Proper scanning techniques
•	Service-based enumeration
•	Logical thinking over assumptions
Successfully identified Redis service, interacted with it, and extracted the flag, completing the objective.

