
![lzcXlHdk8f](https://github.com/user-attachments/assets/2e5f65ab-29d8-4129-897f-d9ab1b200ec8)


# About SmartyPants

Players will use SmartScreen debug logs to understand user activities and corporate sabotage. This Sherlock is released alongside a HackTheBox blog post (https://hackthebox.com/blog/smartscreen-logs-evidence-execution) to highlight how this log, which was discovered by HackTheBox content engineers during their research, can serve as a crucial artefact in incident investigations. 

## Sherlock Scenario

Forela's CTO, Dutch, stores important files on a separate Windows system because the domain environment at Forela is frequently breached due to its exposure across various industries. On 24 January 2025, our worst fears were realised when an intruder accessed the fileserver, installed utilities to aid their actions, stole critical files, and then deleted them, rendering them unrecoverable. The team was immediately informed of the extortion attempt by the intruders, who are now demanding money. While our legal team addresses the situation, we must quickly perform triage to assess the incident's extent. Note from the manager: We enabled SmartScreen Debug Logs across all our machines for enhanced visibility a few days ago, following a security research recommendation. These logs can provide quick insights, so ensure they are utilised.

## Artifacts
- SmartyPants.zip - 08408ADE4DCC93B0D10960D29CE707E8F94D902158B41C8F0BBB04B9BE4CCFE0

## Skills
- Windows forensics
- Event Log Analysis
- DFIR
- User Behavior Forensics

## Tools Used
[Eric Zimmerman's](https://ericzimmerman.github.io/#!index.md) EvtxEcmd and Timeline Explorer 

## Walkthrough
Parse Event logs folder with EvtxEcmd in PowerShell
```
.\EvtxECmd.exe -d ".\Sherlocks\SmartyPants\Logs" --csv ".\Sherlocks\SmartyPants" --csvf results.csv
```
Open the results.csv in Timeline Explorer

### Question 1: The attacker logged in to the machine where Dutch saves critical files, via RDP on 24th January 2025. Please determine the timestamp of this login.
Under Provider filter by RemoteConnectionManager and filter for Event ID: 1149. Result 1 event of "RDP network connection established" by User \Dutch at 2024-01-24 10:15:14
### Answer: 2024-01-24 10:15:14


### Question 2: The attacker downloaded a few utilities that aided them for their sabotage and extortion operation. What was the first tool they downloaded and installed?
Clear previous filters, filter Provider by SmartScreen, search the Payload column for suspicious events after the RDP connection timestamp.
The first couple processes are related to msedge, the first tool downloaded and installed is WinRAR:

Downloaded:
```
{"EventData":{"Data":{"@Name":"Data","#text":"{\"$type\":\"isFileSupported\",\"executionTime\":\"9281\",\"path\":\"C:\\\\Users\\\\Dutch\\\\Downloads\\\\winrar-x64-701.exe\",\"size\":\"3912088\"}"}}}
```
Installed:
```
{"EventData":{"Data":{"@Name":"Data","#text":"{\"$type\":\"isFileSupported\",\"executionTime\":\"5045\",\"path\":\"C:\\\\Program Files\\\\WinRAR\\\\WinRAR.exe\",\"size\":\"3289752\"}"}}}
```
### Answer: WinRAR


### Question 3: They then proceeded to download and execute the portable version of a tool that could be used to search for files on the machine quickly and efficiently. What was the full path of the executable?
Using the same filter I also see Everything.exe:
```{"EventData":{"Data":{"@Name":"Data","#text":"{\"$type\":\"isFileSupported\",\"executionTime\":\"8701\",\"path\":\"C:\\\\Users\\\\Dutch\\\\Downloads\\\\Everything.exe\",\"size\":\"1778192\"}"}}}``` 
A quick Google tells me Everything.exe is a data exfil tool that searches and indexes faster than Windows built in search; threat actors use this tool to quickly find critical files and documents to exfil.
### Answer: C:\Users\Dutch\Downloads\Everything.exe (after removing extra backslashes from the raw data)

### Question 4: What is the execution time of the tool from task 3?
### Answer: Timestamp for Everything.exe line is 2025-01-24 10:17:33


### Question 5: The utility was used to search for critical and confidential documents stored on the host, which the attacker could steal and extort the victim. What was the first document that the attacker got their hands on and breached the confidentiality of that document?
The first document accessed was at 2025-01-24 10:19:00 and it was
```
{"EventData":{"Data":{"@Name":"Data","#text":"{\"$type\":\"isFileSupported\",\"executionTime\":\"3720\",\"path\":\"C:\\\\Users\\\\Dutch\\\\Documents\\\\2025- Board of directors Documents\\\\Ministry Of Defense Audit.pdf\",\"size\":\"2679956\"}"}}}
```
### Answer: C:\Users\Dutch\Documents\2025- Board of directors Documents\Ministry Of Defense Audit.pdf


### Question 6: Find the name and path of second stolen document as well?
The second document was accessed at 2025-01-24 10:19:19 and it was
```
{"EventData":{"Data":{"@Name":"Data","#text":"{\"$type\":\"isFileSupported\",\"executionTime\":\"3726\",\"path\":\"C:\\\\Users\\\\Dutch\\\\Documents\\\\2025- Board of directors Documents\\\\2025-BUDGET-ALLOCATION-CONFIDENTIAL.pdf\",\"size\":\"523480\"}"}}}
```
### Answer: C:\Users\Dutch\Documents\2025- Board of directors Documents\2025-BUDGET-ALLOCATION-CONFIDENTIAL.pdf


### Question 7: The attacker installed a Cloud utility as well to steal and exfiltrate the documents. What is name of the cloud utility?
1 minute after accessing the documents, a file called MegaSyncSetup64.exe was executed:
```
{"EventData":{"Data":{"@Name":"Data","#text":"{\"$type\":\"isFileSupported\",\"executionTime\":\"3675\",\"path\":\"C:\\\\Users\\\\Dutch\\\\AppData\\\\Local\\\\MEGAsync\\\\MEGAsync.exe\",\"size\":\"77568264\"}"}}}
```
A quick google search shows that this is a cloud utility.
### Answer: MEGAsync


### Question 8: When was this utility executed?
### Answer: 2025-01-24 10:22:19

### Question 9:The Attacker also proceeded to destroy the data on the host so it is unrecoverable. What utility was used to achieve this?
Further down the timeline I find:
```
{"EventData":{"Data":{"@Name":"Data","#text":"{\"$type\":\"isFileSupported\",\"executionTime\":\"7943\",\"path\":\"C:\\\\Users\\\\Dutch\\\\Downloads\\\\file_shredder_setup.exe\",\"size\":\"2317839\"}"}}}
```
A quick google search shows that this is a utility to permanently remove data from drives; the tool is also running:
```
{"EventData":{"Data":{"@Name":"Data","#text":"{\"$type\":\"isFileSupported\",\"executionTime\":\"5736\",\"path\":\"C:\\\\Program Files\\\\File Shredder\\\\Shredder.exe\",\"size\":\"2454528\"}"}}}
```
### Answer: File Shredder


### Question 10: The attacker cleared 2 important logs, thinking they covered all their tracks. When was the security log cleared?
Clear previous filters, filter Channel by Security and Event ID by 1102. 1 result showing the Security Log was cleared.
### Answer: 2025-01-24 10:28:41

