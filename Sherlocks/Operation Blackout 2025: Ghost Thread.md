
![NWQs9JjEQe](https://github.com/user-attachments/assets/c823da63-1bd5-46f6-8c22-4c00afbdb659)

# Operation Blackout 2025: Ghost Thread 

## Sherlock Scenario
Byte Doctor suspects the attacker used a process injection technique to run malicious code within a legitimate process, leaving minimal traces on the file system. The logs reveal Win32 API calls that hint at a specific injection method used in the attack. Your task is to analyze these logs using a tool called API Monitor to uncover the injection technique and identify which legitimate process was targeted.

## Artifacts
- Ghost-Thread.apmx64 -  1ADEE8789B3AF0ABAD4E31510B5080975441A43DCDEFF3D60C51F2AC5F325151
- inject.exe.i64 - 651B0F57835EA1F11EB2078D8375EA9BA6A760835FD660FA5F67593487697F37
- INSTRUCTIONS.txt - 565F0003671AC2E513E95B8A728206D333F4B147F6F6E2FD2F44247F01B64896

## Skills
- DFIR
- Code Injection
- Low-level Windows API calls
- Filter and correlate API calls to uncover hidden execution flows

## Tools Used
- IDA Freeware
- Rohitab API Monitor

## Walkthrough

### Task 1: What process injection technique did the attacker use?
Examine inject.exe.i64 in IDA Freeware under the exports tab you can find the entry called TlsCallback_0. Digging deeper into TlsCallback_0 we can find the injection technique.


### Task 2: Which Win32 API was used to take snapshots of all processes and threads on the system?
In API Monitor, investigating inject.exe shows that the attacker is  using the 'CreateToolhelp32Snapshot' API with the 'TH32CS_SNAPPROCESS' flag.


### Task 3:Which process is the attacker's binary attempting to locate for payload injection?
You can see that the inject.exe is attempting to locate 'notepad.exe' by comparing 'notepad.exe' with running background processes, and if there's no match, it calls the 'Process32Next' API to check the next process.


### Task 4: What is the process ID of the identified process?
After locating the 'notepad.exe' process, the binary invokes the 'OpenProcess' API. In the arguments of the 'OpenProcess' the process ID can be found under Type = DWORD: Name = dwProcessID.


### Task 5: What is the size of the shellcode?
The binary calls 'VirtualAllocEx' to allocate memory in the target process, the shellcode size is found under Type = SIZE_T: Name = dwSize.


### Task 6: Which Win32 API was used to execute the injected payload in the identified process?
After allocating memory, the binary uses 'WriteProcessMemory' to write the shellcode to a specific location, followed by 'CreateRemoteThread' to execute the payload.

### Task 7: The injection method used by the attacker executes before the main() function is called. Which Win32 API is responsible for terminating the program before main() runs?
As identified in the first question, the attacker is using the Thread Local Storage (TLS) injection technique, which involves manipulating pointers within a Portable Executable(PE) file to redirect execution to malicious code before the program reaches its legitimate entry point. Based on this, all the above shellcode injection is occuring within a malicious TLS callback. To prevent the program from continuing to its actual entry point (main function), the process must terminate early. The API calls confirm that the binary invokes 'ExitProcess' to terminate execution before reaching main().
