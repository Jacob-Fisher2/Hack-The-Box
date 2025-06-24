
![SNDqnINxlq](https://github.com/user-attachments/assets/17cc0173-fed4-491a-a4ec-2bed1ff3261c)

# Dream Job-1 
## About Dream Job-1
In this Sherlock, players will be introduced to the MITRE ATT&CK framework, which is a comprehensive tool used to research and understand advanced persistent threat (APT) groups. Specifically, players will focus on the APT group known as Lazarus Group. As they progress, players will get to explore various tactics, techniques, and procedures (TTPs) associated with Lazarus Group.


## Sherlock Scenario
You are a junior threat intelligence analyst at a Cybersecurity firm. You have been tasked with investigating a Cyber espionage campaign known as Operation Dream Job. The goal is to gather crucial information about this operation.

## Artifacts
- IOCs.txt - 9EDDA982FC5933181DA898305230B5A71B41C7314430C0EA2F15EBED10A6793C

## Skills
- Threat Intelligence
- MITRE ATT&CK
- ATT&CK Navigator

## Walkthrough

### Task 1: Who conducted Operation Dream Job?
Found on the [MITRE campaign page](https://attack.mitre.org/campaigns/C0022/) for Operation Dream Job.

### Task 2: When was this operation first observed?
Found on the [MITRE campaign page](https://attack.mitre.org/campaigns/C0022/) for Operation Dream Job.

### Task 3: There are 2 campaigns associated with Operation Dream Job. One is OperationOperationOperationOperation North StarNorth StarNorth StarNorth Star , what is the other?
Found on the [MITRE campaign page](https://attack.mitre.org/campaigns/C0022/) for Operation Dream Job.

### Task 4: During Operation Dream Job, there were the two system binaries used for proxy execution. One was regsvr, what was the other?
Looking into the MITRE ATT&CK Navigator of the same page, we will look for Defense Evasion technique there we will find System Binary Proxy Execution.

### Task 5: What lateral movement technique did the adversary use?
In the Navigate layer go to Lateral Movement technique.

### Task 6: What is the technique ID for the previous answer?
Hover over the technique from task 5.

### Task 7: What Remote Access Tool did the Lazarus Group use in Operation Dream Job?
In the MITRE Operation Dream Job page, find the RAT used in teh Software section.

### Task 8: What technique did the malware use for execution?
In the [DRATzrus page](https://attack.mitre.org/software/S0694/) open ATT&CK Navigator, you will find teh answer under Execution.

### Task 9: What technique did the malware use to avoid detection in a sandbox?
In the DRATzrus ATT&CK Navigator, look at Discovery Technique, the answer is in the Virtualization/Sandbox Evasion section.

### Task 10: To answer the remaining questions, utilize VirusTotal and refer to the IOCs.txt file. What is the name associated with the first hash provided in the IOC file?
Found in the [VirusTotal page](https://www.virustotal.com/gui/file/7bb93be636b332d0a142ff11aedb5bf0ff56deabba3aa02520c85bd99258406f) for the first hash.

### Task 11: When was the file associated with the second hash in the IOC first created?
Found in the [VirusTotal page](https://www.virustotal.com/gui/file/adce894e3ce69c9822da57196707c7a15acee11319ccc963b84d83c23c3ea802) for the second hash under Details.

### Task 12: What is the name of the parent execution file associated with the second hash in the IOC?
Same VirusTotal page as task 11, found under the Relations tab.

### Task 13: Examine the third hash provided. What is the file name likely used in the campaign that aligns with the adversary's known tactics?
Found in the [VirusTotal page](https://www.virustotal.com/gui/file/0160375e19e606d06f672be6e43f70fa70093d2a30031affd2929a5c446d07c1) for the third hash in the Details tab under the Names section.

### Task 14: Which URL was contacted on 2022-08-03 by the file associated with the third hash in the IOC file?
Same VirusTotal page as task 13, found in the Relations tab under Contacted URLs.
