
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

### Task 1:What is the build version of the operating system?
Checked the memory capture for OS version with volatility 3's windows.info plugin.
```
python3 ~/volatility3-develop/vol.py -f memory.vmem windows.info
```
![ZlZIbY3IHd](https://github.com/user-attachments/assets/2a33638d-5225-4fab-8918-cac492a41e36)

Was stuck on this task the longest until I figured out through trial and error that the answer was Major/Minor 15.19041.

