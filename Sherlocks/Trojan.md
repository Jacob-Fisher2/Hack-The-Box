
![vFICq6QA6i](https://github.com/user-attachments/assets/366e4b8f-f8a6-4817-b977-f65b882edd36)

# Trojan

## Sherlock Scenario
John Grunewald was deleting some old accounting documents when he accidentally deleted an important document he had been working on. He panicked and downloaded software to recover the document, but after installing it, his PC started behaving strangely. Feeling even more demoralised and depressed, he alerted the IT department, who immediately locked down the workstation and recovered some forensic evidence. Now it is up to you to analyze the evidence to understand what happened on John's workstation.

## Artifacts
```bash
  └── Trojan
      ├── disk artifacts
            └── disk_artifacts.ad1
      ├── memory capture
            ├── memory.vmem
            └── memory.vmsn
      └── packet capture
            └── network.pcapng
  
```

## Skills
- DFIR
- PCAP Investigation
- Malware Research
- Forensic Data Imaging
- Volatile Memory Extraction

## Tools Used
- Volatility 3
- Wireshark
- FTK Imager
- PECmd (PreFetch Parser)
- VirusTotal

## Walkthrough

### Task 1: What is the build version of the operating system?
Checked the memory capture for OS version with volatility 3's windows.info plugin.
```
python3 ~/volatility3-develop/vol.py -f memory.vmem windows.info
```
![ZlZIbY3IHd](https://github.com/user-attachments/assets/2a33638d-5225-4fab-8918-cac492a41e36)

Was stuck on this task the longest until I figured out through trial and error that the answer was Major/Minor 15.19041.

### Task 2: What is the computer hostname?
Hostname was found in multiple places within the network capture, found by filtering by:
```
ip.addr == 192.168.116.133
```

### Task 3: What is the name of the downloaded ZIP file?
Found with Wireshark by filtering by '.zip'


### Task 4: What is the domain of the website (including the third-level domain) from which the file was downloaded?
In Wireshark under the same fitler as task 3 you can see the full domain that the zip was downloaded from.


### Task 5: The user then executed the suspicious application found in the ZIP archive. What is the process PID?
Use Volatility 3 to analyze the memory capture with the plugin windows.pstree with a grep for Data_Recovery.
```
python3 ~/volatility3-develop/vol.py -f memory.vmem windows.pstree|grep -i data_recovery
```


### Task 6.What is the full path of the suspicious process?
Using the same output from task 5 gives the full path of the suspicious process.


### Task 7:What is the SHA-256 hash of the suspicious executable?
Pulled the suspicious .exe out of the disk artifact with FTK Imager and hashed with Powershell;
```
PS Get-FileHash .\Recovery_Setup.exe
```
> Algorithm      Hash
> SHA256         C34601c5da3501f6ee0efce18de7e6145153ecfac2ce2019ec52e1535a4b3193


### Task 8: When was the malicious program first executed?
Exported the PreFetch files from the out of the disk artifact with FTK Imager, then used PECmd to analyze the PreFetch and output to readable a csv. 
Timestamp found on "PreviousRun0" for a filter of "RECOVERY_SETUP.EXE"


### Task 9:How many times in total has the malicious application been executed?
Same line as task 8, column "RunCount"
