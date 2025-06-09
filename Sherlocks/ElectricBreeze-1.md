
![iyGGfACI9D](https://github.com/user-attachments/assets/c7ae4dfa-89e8-4c22-83f6-76a1a3c371ad)

# ElectricBreeze-1

## Sherlock Scenario
Your security team must always be up-to-date and aware of the threats targeting organizations in your industry. As you begin your journey as a Threat Intelligence Intern, equipped with some SOC experience, your manager has assigned you a task to test your research skills and how effectively you can leverage the MITRE ATT&CK framework. * Conduct thorough research on Volt Typhoon. * Use the MITRE ATT&CK framework to map adversary behavior and tactics into actionable insights. Impress your manager with your assessment, showcasing your passion for threat intelligence.

## Skills
- MITRE ATT&CK framework
- Research
- Threat Intelligence
- SOC

## Tools Used
- MITRE ATT&CK knowledgebase
- VirusTotal

## Walkthrough

### Question 1: Based on MITRE’s sources, since when has Volt Typhoon been active?
Looking up Volt Typhoon's MITRE page shows that right at the top of the [page](https://attack.mitre.org/groups/G1017/):
> Volt Typhoon is a People's Republic of China (PRC) state-sponsored actor that has been active since at least **2021** primarily targeting critical infrastructure organizations in the US and its territories including Guam. 

### Question 2: MITRE identifies two OS credential dumping techniques used by Volt Typhoon. One is LSASS Memory access (T1003.001). What is the Attack ID for the other technique?
Ctrl-F'ing T1003 finds 2 OS Dumping Credential Dumping techniques, T1003.001 (LSASS Memory) and T1003.003 (NTDS) within the MITRE page for Volt Typhoon.

### Question 3: Which database is targeted by the credential dumping technique mentioned earlier?
The T1003.003 (NTDS) entry shows that
> Volt Typhoon has used ntds.util to create domain controller installation media containing usernames and password hashes
NTDS.DIT stands for "the New Technology Directory Services Directory Information Tree, is the database for Active Directory Domain Services (AD DS).”
### Answer: Active Directory


### Question 4: Which registry hive is required by the threat actor to decrypt the targeted database?
Ctrl-F'ing NTDS.DIT finds:
> Volt Typhoon has saved stolen files including the ntds.dit database and the SYSTEM and SECURITY Registry hives locally to the C:\Windows\Temp\ directory.
### Answer: SYSTEM


### Question 5: During the June 2024 campaign, an adversary was observed using a Zero-Day Exploitation targeting Versa Director. What is the name of the Software/Malware that was used?
Ctrl-F'ing 2024 finds the attack from June of 2024 with a Campaign ID of C0039. In [C0039](https://attack.mitre.org/campaigns/C0039/) we find this "Versa Director Zero Day Exploitation involved the development of a new web shell variant, **VersaMem**.”

### Question 6: According to the Server Software Component, what type of malware was observed?
Under Techniques Used and we find “Server Software Component: Web Shell”

### Question 7: Where did the malware store captured credentials?
On the [VersaMem page](https://attack.mitre.org/software/S1154/), under Techniques Used, it says “VersaMem staged captured credentials locally at /tmp/.temp.data.”

### Question 8: According to MITRE’s reference, a Lumen/Black Lotus Labs article(Taking The Crossroads: The Versa Director Zero-Day Exploitation.), what was the filename of the first malware version scanned on VirusTotal?
At the bottom of the VersaMem page under the References section you can find the Lumen/Black Lotus Labs article, you find this early in the article, “The VersaMem web shell is a sophisticated JAR web shell that was uploaded to VirusTotal on June 7, 2024, with the filename “VersaTest.png” 

### Question 9: What is the SHA256 hash of the file?
Within the same Lumen/Black Lotus Labs article you can find the “VersaTest.png” SHA256: 4bcedac20a75e8f8833f4725adfc87577c32990c3783bf6c743f14599a176c37

### Question 10: According to VirusTotal, what is the file type of the malware?
The [VirusTotal page](https://www.virustotal.com/gui/file/4bcedac20a75e8f8833f4725adfc87577c32990c3783bf6c743f14599a176c37) under details shows that the File Type is JAR

### Question 11: What is the ‘Created by’ value in the file’s Manifest according to VirusTotal?
The VirusTotal Detail -> Manifest section:
> Manifest-Version: 1.0
> 
> Archiver-Version: Plexus Archiver
> 
> Created-By: Apache Maven 3.6.0
> 
> Built-By: versa
> 
> Build-Jdk: 11.0.19
> 
> Agent-Class: com.versa.vnms.ui.TestMain
> 
> Can-Redefine-Classes: true
> 
> Can-Retransform-Classes: true
> 
> Main-Class: com.versa.vnms.ui.TestMain
> 
> Premain-Class: com.versa.vnms.ui.TestMain
> 

### Question 12: What is the CVE identifier associated with this malware and vulnerability?
The VirusTotal Detections tab has the CVE listed under DrWeb and as the object's tag: CVE-2024-39717

### Question 13: According to the [CISA document](https://www.cisa.gov/sites/default/files/2024-03/aa24-038a_csa_prc_state_sponsored_actors_compromise_us_critical_infrastructure_3.pdf) referenced by MITRE, what is the primary strategy Volt Typhoon uses for defense evasion?
Under the section titled “Defense Evasion”, it says “Volt Typhoon has strong operational security. Their actors primarily use LOTL for defense evasion, which allows them to camouflage their malicious activity with typical system and network behavior, potentially circumventing simplistic endpoint security capabilities.” 

### Question 14: In the CISA document, which file name is associated with the command potentially used to analyze logon patterns by Volt Typhoon?
“Specifically, in one incident, analysis of the PowerShell console history of a domain
controller indicated that security event logs were directed to a file named user.dat, as evidenced by the executed command Get-EventLog security -instanceid 4624 -after [year-month-date] | fl * | Out-File ‘C:\users\public\documents\user.dat’.”
### Answer: C:\users\public\documents\user.dat


