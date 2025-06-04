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


