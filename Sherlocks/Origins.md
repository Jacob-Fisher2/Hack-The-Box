![rFS2w964JJ](https://github.com/user-attachments/assets/60e921a6-937a-4135-a60f-176fe82a3ac1)

# Origins

## Sherlock Scenario
A major incident has recently occurred at Forela. Approximately 20 GB of data were stolen from internal s3 buckets and the attackers are now extorting Forela. During the root cause analysis, an FTP server was suspected to be the source of the attack. It was found that this server was also compromised and some data was stolen, leading to further compromises throughout the environment. You are provided with a minimal PCAP file. Your goal is to find evidence of brute force and data exfiltration.


## Artifacts
- ftp.pcap - B770184FBC4A68E64D8E28ED9D9CF3E778CA441869736B8B33D13AB69E317C8B

## Skills
- DFIR
- Wireshark
- Log Analysis
- Attacker Research


## Walkthrough


### Task 1: What is the attacker's IP address?
In WireShark filter for FTP traffic, pull the attackers IP from the brute force attempts.


### Task 2: It's critical to get more knowledge about the attackers, even if it's low fidelity. Using the geolocation data of the IP address used by the attackers, what city do they belong to?
Do a geolocation lookup of the attackers IP.


### Task 3: Which FTP application was used by the backup server? Enter the full name and version. (Format: Name Version)
Find the version banner in the first couple FTP packets, in WireShark this is in the 'Packet Details' section.


### Task 4: The attacker has started a brute force attack on the server. When did this attack start?
In WireShark filter by ip.src==15.206.185.207 and ftp. The first packet that matches a brute force pattern is when the attack started.


### Task 5: What are the correct credentials that gave the attacker access? (Format username:password)
Using the same filter as task 4, the last credential set in the brute force is what may have given the attacker access. It can be confirmed by looking for "Login Successful" in all the FTP traffic.


### Task 6: The attacker has exfiltrated files from the server. What is the FTP command used to download the remote files?
Filter for "ftp-data" in all traffic, this shows resources transferred over FTP.


### Task 7: Attackers were able to compromise the credentials of a backup SSH server. What is the password for this SSH server?
Exporting the files downloaded by the attacker leads to a pdf with sensitive credentials for the backup SSH server.


### Task 8: What is the s3 bucket URL for the data archive from 2023?
Also found in the files downloaded by the attacker.


### Task 9: The scope of the incident is huge as Forela's s3 buckets were also compromised and several GB of data were stolen and leaked. It was also discovered that the attackers used social engineering to gain access to sensitive data and extort it. What is the internal email address used by the attacker in the phishing email to gain access to sensitive data stored on s3 buckets?
Also found in the files downloaded by the attacker.
