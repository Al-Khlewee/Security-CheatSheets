# Windows Binary Analysis & Reverse Engineering Handbook

> **Classification:** Principal Malware Researcher Reference  
> **Version:** 2026.1 | **Last Updated:** January 2026  
> **Scope:** PE32/PE64 Binary Triage, Dynamic Analysis, Windows Internals

---

## Table of Contents

1. [PE File Anatomy & Triage](#1-pe-file-anatomy--triage)
2. [Windows API & Process Injection](#2-windows-api--process-injection)
3. [Dynamic Debugging (x64dbg & WinDbg)](#3-dynamic-debugging-x64dbg--windbg)
4. [Windows Anti-Analysis & Evasion](#4-windows-anti-analysis--evasion)
5. [Persistence & Execution Logic](#5-persistence--execution-logic)

---

## 1. PE File Anatomy & Triage

### 1.1 MZ/PE Header Manual Parsing

The Portable Executable format begins with the DOS Header (`IMAGE_DOS_HEADER`), followed by the PE signature and headers.

#### DOS Header Structure (Offset 0x00)

| Offset | Size | Field | Description |
|--------|------|-------|-------------|
| 0x00 | 2 | `e_magic` | MZ signature (`0x5A4D`) |
| 0x3C | 4 | `e_lfanew` | Offset to PE signature |

#### PE Signature & File Header (at `e_lfanew`)

| Offset | Size | Field | Description |
|--------|------|-------|-------------|
| 0x00 | 4 | Signature | PE\0\0 (`0x00004550`) |
| 0x04 | 2 | Machine | `0x014C` (x86), `0x8664` (x64) |
| 0x06 | 2 | NumberOfSections | Section count |
| 0x08 | 4 | TimeDateStamp | Compilation timestamp (UTC) |
| 0x14 | 2 | SizeOfOptionalHeader | Size of Optional Header |
| 0x16 | 2 | Characteristics | File attributes flags |

#### Optional Header (Immediately after File Header)

| Field | PE32 Offset | PE64 Offset | Description |
|-------|-------------|-------------|-------------|
| Magic | 0x18 | 0x18 | `0x10B` (PE32), `0x20B` (PE32+) |
| AddressOfEntryPoint | 0x28 | 0x28 | RVA of entry point |
| ImageBase | 0x34 | 0x30 | Preferred load address |
| SectionAlignment | 0x38 | 0x38 | Memory alignment (usually 0x1000) |
| FileAlignment | 0x3C | 0x3C | Disk alignment (usually 0x200) |
| SizeOfImage | 0x50 | 0x50 | Total image size in memory |
| NumberOfRvaAndSizes | 0x74 | 0x84 | Data Directory count (usually 16) |

**Manual Parsing via PowerShell:**

```powershell
# Read PE headers from binary
$bytes = [System.IO.File]::ReadAllBytes("C:\sample.exe")
$e_lfanew = [BitConverter]::ToInt32($bytes, 0x3C)
$machine = [BitConverter]::ToUInt16($bytes, $e_lfanew + 4)
$numSections = [BitConverter]::ToUInt16($bytes, $e_lfanew + 6)
$timestamp = [BitConverter]::ToUInt32($bytes, $e_lfanew + 8)
$compileTime = [DateTimeOffset]::FromUnixTimeSeconds($timestamp).DateTime

Write-Host "Machine: $('{0:X4}' -f $machine) | Sections: $numSections | Compiled: $compileTime"
```

**Manual Parsing via Python:**

```python
import struct
from datetime import datetime

with open("sample.exe", "rb") as f:
    # DOS Header
    mz = f.read(2)
    assert mz == b'MZ', "Invalid MZ signature"
    
    f.seek(0x3C)
    e_lfanew = struct.unpack('<I', f.read(4))[0]
    
    # PE Header
    f.seek(e_lfanew)
    pe_sig = f.read(4)
    assert pe_sig == b'PE\x00\x00', "Invalid PE signature"
    
    machine, num_sections = struct.unpack('<HH', f.read(4))
    timestamp = struct.unpack('<I', f.read(4))[0]
    
    print(f"Machine: {machine:04X} | Sections: {num_sections}")
    print(f"Compiled: {datetime.utcfromtimestamp(timestamp)}")
```

---

### 1.2 Data Directories Deep Dive

Data Directories are located at offset `0x78` (PE32) or `0x88` (PE32+) from Optional Header start.

| Index | Name | Purpose |
|-------|------|---------|
| 0 | Export Table | Functions exported by the module |
| 1 | Import Table | External dependencies (IAT/ILT) |
| 2 | Resource Table | Icons, strings, manifests, embedded files |
| 3 | Exception Table | SEH/x64 exception handlers |
| 4 | Certificate Table | Authenticode digital signatures |
| 5 | Relocation Table | Base relocation fixups |
| 6 | Debug Directory | PDB path, debug metadata |
| 11 | Bound Import | Pre-resolved import addresses |
| 12 | IAT | Import Address Table |
| 14 | CLR Header | .NET metadata (if managed) |

#### Import Table Analysis

The Import Directory (`IMAGE_IMPORT_DESCRIPTOR`) structure:

```
+0x00  OriginalFirstThunk (ILT RVA)
+0x04  TimeDateStamp
+0x08  ForwarderChain
+0x0C  Name (RVA to DLL name)
+0x10  FirstThunk (IAT RVA)
```

**Suspicious Import Patterns:**

| Category | Imports | Indication |
|----------|---------|------------|
| Injection | `VirtualAllocEx`, `WriteProcessMemory`, `CreateRemoteThread` | Process injection |
| Keylogging | `SetWindowsHookExA/W`, `GetAsyncKeyState` | Input capture |
| Networking | `WSAStartup`, `InternetOpenA`, `HttpSendRequestA` | C2 communication |
| Evasion | `IsDebuggerPresent`, `CheckRemoteDebuggerPresent` | Anti-debug |
| Privilege | `AdjustTokenPrivileges`, `LookupPrivilegeValueA` | Privilege escalation |
| Crypto | `CryptAcquireContextA`, `CryptEncrypt` | Ransomware/data exfil |

#### Export Table Analysis

```
+0x00  Characteristics (reserved)
+0x04  TimeDateStamp
+0x0C  Name RVA (DLL name)
+0x10  Base (ordinal base)
+0x14  NumberOfFunctions
+0x18  NumberOfNames
+0x1C  AddressOfFunctions (RVA)
+0x20  AddressOfNames (RVA)
+0x24  AddressOfNameOrdinals (RVA)
```

**Export Forwarding Detection:**

If an export's RVA points within the Export Directory's virtual range, it's a forwarder:
```
"NTDLL.RtlAllocateHeap" -> Forwarded to ntdll.dll
```

---

### 1.3 PE Triage Tools

#### PE-bear

| Feature | Description |
|---------|-------------|
| Section Viewer | Raw vs virtual size comparison |
| Import/Export Browser | Full dependency tree |
| Resource Extractor | Dump embedded binaries |
| Overlay Detection | Data appended after PE |

#### PEStudio

| Check | Red Flag Indicators |
|-------|---------------------|
| Entropy | Sections > 7.0 (packed/encrypted) |
| Imports | Blacklisted APIs flagged |
| Strings | URLs, IPs, suspicious paths |
| Sections | Executable + writable sections |
| Manifest | Missing or mismatched manifests |

#### Detect It Easy (DIE)

**Signature-Based Detection:**

```
Compiler:   Microsoft Visual C/C++ (19.29.30133)
Linker:     Microsoft Linker (14.29.30133)
Packer:     UPX (3.96) [Modified]
Overlay:    Present (0x4A00 bytes)
```

**DIE Script for Custom Detection:**

```javascript
// Custom DIE signature script
function detect(binaryData) {
    if (PE.getImportLibraryName(0) === "MSVBVM60.DLL") {
        return "Visual Basic 6.0 Application";
    }
    if (PE.getSectionName(0) === "UPX0") {
        return "UPX Packed";
    }
}
```

---

### 1.4 Entropy & Packing Analysis

#### Section Entropy Reference

| Section | Normal Entropy | Packed/Encrypted |
|---------|----------------|------------------|
| `.text` | 5.5 - 6.5 | > 7.0 |
| `.data` | 1.0 - 5.0 | > 7.0 |
| `.rdata` | 4.0 - 6.0 | > 7.0 |
| `.rsrc` | 3.0 - 6.5 | > 7.5 (embedded payload) |

#### Calculating Shannon Entropy

```python
import math
from collections import Counter

def entropy(data: bytes) -> float:
    if not data:
        return 0.0
    freq = Counter(data)
    length = len(data)
    return -sum((count/length) * math.log2(count/length) for count in freq.values())

# Usage
with open("sample.exe", "rb") as f:
    section_data = f.read()
    print(f"Entropy: {entropy(section_data):.4f}")
```

#### Common Packer Signatures

| Packer | Section Names | Entry Point Characteristics |
|--------|---------------|----------------------------|
| UPX | `UPX0`, `UPX1` | PUSHAD/POPAD unpacking loop |
| Themida | `.themida` | VM-based obfuscation |
| VMProtect | `.vmp0`, `.vmp1` | Virtualized code |
| ASPack | `.aspack` | Modified EP stub |
| PECompact | `PEC2` | Compressed sections |
| MPRESS | `.MPRESS1` | LZMA compression |

**Unpacking Detection via x64dbg:**

```assembly
; Classic UPX entry stub
60                   PUSHAD
BE 00 10 40 00       MOV ESI, 00401000
8D BE 00 F0 FF FF    LEA EDI, [ESI-1000]
; ... decompression loop ...
61                   POPAD
E9 XX XX XX XX       JMP OEP
```

> **Pro-Tip:** Use `binwalk -e sample.exe` to extract embedded files from resources or overlay. Combine with `foremost` for carving unknown file types from memory dumps.

---

## 2. Windows API & Process Injection

### 2.1 Standard Injection Pattern

The classic injection flow targets a remote process to execute arbitrary shellcode or DLLs.

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│  OpenProcess()  │───▶│ VirtualAllocEx()│───▶│WriteProcessMem()│
│  (PROCESS_ALL)  │    │   (RWX Memory)  │    │  (Write SC/DLL) │
└─────────────────┘    └─────────────────┘    └────────┬────────┘
                                                       │
                       ┌─────────────────┐             │
                       │CreateRemoteThread│◀───────────┘
                       │  (Execute Code) │
                       └─────────────────┘
```

#### Implementation Reference

```c
// Classic remote injection (x64)
HANDLE hProcess = OpenProcess(PROCESS_ALL_ACCESS, FALSE, targetPID);

LPVOID pRemoteMem = VirtualAllocEx(
    hProcess,
    NULL,
    sizeof(shellcode),
    MEM_COMMIT | MEM_RESERVE,
    PAGE_EXECUTE_READWRITE  // 0x40 - RWX
);

WriteProcessMemory(
    hProcess,
    pRemoteMem,
    shellcode,
    sizeof(shellcode),
    NULL
);

HANDLE hThread = CreateRemoteThread(
    hProcess,
    NULL,
    0,
    (LPTHREAD_START_ROUTINE)pRemoteMem,
    NULL,
    0,
    NULL
);
```

**Detection Points:**

| API Call | Artifact |
|----------|----------|
| `OpenProcess` | Cross-process handle with suspicious access rights |
| `VirtualAllocEx` | RWX memory in remote process |
| `WriteProcessMemory` | Memory write to foreign process |
| `CreateRemoteThread` | Thread creation in remote process (ETW event) |

---

### 2.2 Modern Injection Variations

#### Process Hollowing (RunPE)

**Flow:**
1. Create suspended process (`CREATE_SUSPENDED`)
2. Unmap original image (`NtUnmapViewOfSection`)
3. Allocate memory at ImageBase
4. Write malicious PE headers + sections
5. Set thread context (new entry point)
6. Resume thread

```c
// Key syscalls
NtUnmapViewOfSection(hProcess, pImageBase);
NtAllocateVirtualMemory(hProcess, &pImageBase, ...);
NtWriteVirtualMemory(hProcess, pImageBase, pMaliciousPE, ...);

// Hijack entry point
CONTEXT ctx;
ctx.ContextFlags = CONTEXT_FULL;
GetThreadContext(hThread, &ctx);
#ifdef _WIN64
    ctx.Rcx = (DWORD64)newEntryPoint;  // x64: RCX = entry
#else
    ctx.Eax = (DWORD)newEntryPoint;    // x86: EAX = entry
#endif
SetThreadContext(hThread, &ctx);
ResumeThread(hThread);
```

#### Thread Execution Hijacking

**Flow:**
1. Open target thread
2. Suspend thread (`SuspendThread`)
3. Get thread context
4. Modify RIP/EIP to shellcode address
5. Resume thread

```c
SuspendThread(hThread);

CONTEXT ctx;
ctx.ContextFlags = CONTEXT_CONTROL;
GetThreadContext(hThread, &ctx);

#ifdef _WIN64
    ctx.Rip = (DWORD64)pShellcode;
#else
    ctx.Eip = (DWORD)pShellcode;
#endif

SetThreadContext(hThread, &ctx);
ResumeThread(hThread);
```

#### DLL Injection Techniques

| Technique | Method | Detection Difficulty |
|-----------|--------|----------------------|
| CreateRemoteThread + LoadLibrary | `LoadLibraryA` as thread start | Low |
| SetWindowsHookEx | Global hook with DLL | Medium |
| QueueUserAPC | APC injection (alertable thread) | Medium |
| NtMapViewOfSection | Shared section mapping | High |
| Atom Bombing | GlobalAddAtom + APC | High |
| Early Bird | Inject before main thread runs | High |

**QueueUserAPC Injection:**

```c
// Requires alertable thread state
QueueUserAPC(
    (PAPCFUNC)pLoadLibrary,
    hThread,
    (ULONG_PTR)pRemoteDllPath
);
// Thread must call SleepEx, WaitForSingleObjectEx, etc.
```

---

### 2.3 Win32 vs Native API Mapping

| Win32 (kernel32.dll) | Native (ntdll.dll) | Notes |
|----------------------|--------------------|----- |
| `VirtualAlloc` | `NtAllocateVirtualMemory` | Local memory |
| `VirtualAllocEx` | `NtAllocateVirtualMemory` | Remote with handle |
| `VirtualProtect` | `NtProtectVirtualMemory` | Change protection |
| `ReadProcessMemory` | `NtReadVirtualMemory` | Cross-process read |
| `WriteProcessMemory` | `NtWriteVirtualMemory` | Cross-process write |
| `CreateThread` | `NtCreateThreadEx` | Local thread |
| `CreateRemoteThread` | `NtCreateThreadEx` | Remote thread |
| `OpenProcess` | `NtOpenProcess` | Process handle |
| `TerminateProcess` | `NtTerminateProcess` | Kill process |
| `CreateFile` | `NtCreateFile` | File/device I/O |
| `RegOpenKey` | `NtOpenKey` | Registry access |

**Direct Syscall Invocation (x64):**

```assembly
; NtAllocateVirtualMemory syscall stub
mov r10, rcx
mov eax, 0x18           ; Syscall number (version-dependent!)
syscall
ret
```

**Syscall Number Lookup:**

```powershell
# Dump ntdll exports with ordinals
dumpbin /exports C:\Windows\System32\ntdll.dll | Select-String "Nt"
```

**Dynamic Syscall Resolution:**

```c
// Halo's Gate / Hell's Gate technique
DWORD GetSSN(LPCSTR funcName) {
    HMODULE ntdll = GetModuleHandleA("ntdll.dll");
    FARPROC func = GetProcAddress(ntdll, funcName);
    
    // Check for syscall stub: mov eax, SSN
    if (*((PBYTE)func) == 0x4C &&       // mov r10, rcx
        *((PBYTE)func + 3) == 0xB8) {   // mov eax, SSN
        return *((PDWORD)((PBYTE)func + 4));
    }
    return 0;
}
```

> **Pro-Tip:** Modern EDRs hook ntdll.dll in usermode. Use direct syscalls or unhook ntdll by remapping a fresh copy from disk (`\KnownDlls\ntdll.dll`) to bypass inline hooks.

---

## 3. Dynamic Debugging (x64dbg & WinDbg)

### 3.1 x64dbg Essential Commands

#### Flow Control

| Command | Shortcut | Description |
|---------|----------|-------------|
| `StepInto` | F7 | Step into call |
| `StepOver` | F8 | Step over call |
| `Run` | F9 | Continue execution |
| `RunToCursor` | F4 | Run to selected address |
| `RunToUserCode` | Alt+F9 | Escape system DLLs |
| `Pause` | F12 | Break execution |
| `Restart` | Ctrl+F2 | Restart debuggee |

#### Breakpoint Management

```
bp 0x401000                    # Software breakpoint at address
bp kernel32.VirtualAlloc       # Break on API call
bp ntdll.NtAllocateVirtualMemory
bph 0x401000, r, 4             # Hardware read breakpoint (4 bytes)
bph 0x401000, w, 4             # Hardware write breakpoint
bph 0x401000, x                # Hardware execute breakpoint
bpc                            # Clear all breakpoints
bd 0                           # Disable breakpoint #0
be 0                           # Enable breakpoint #0
bplist                         # List all breakpoints
```

#### Conditional Breakpoints

```
bp VirtualAllocEx, "arg.get(3) == 0x40"    # Break if flProtect == RWX
bp CreateFileW, "arg.get(0) == L\"secret.txt\""
bp WriteProcessMemory, "log \"Writing {arg.get(3)} bytes\""
```

#### Memory Operations

```
dump 0x401000                  # Dump memory at address
dump esp                       # Dump stack
dump [eax]                     # Dump memory pointed by EAX
db 0x401000                    # Display bytes
dw 0x401000                    # Display words
dd 0x401000                    # Display dwords
dq 0x401000                    # Display qwords
da 0x401000                    # Display ASCII string
du 0x401000                    # Display Unicode string
find 0x401000, "MZ"            # Search for pattern
findall 0x401000, "4D 5A"      # Search all occurrences
```

#### Tracing & Logging

```
TraceIntoConditional           # Trace until condition met
TraceClear                     # Clear trace
log "Value: {eax}"             # Log to console
logclear                       # Clear log
scriptlog "Message"            # Log from script
```

---

### 3.2 WinDbg Kernel & User Mode

#### Essential Shortcuts

| Command | Description |
|---------|-------------|
| `g` | Go (continue) |
| `t` | Trace (step into) |
| `p` | Step over |
| `gu` | Go up (execute until return) |
| `bp` | Set breakpoint |
| `bl` | List breakpoints |
| `bc *` | Clear all breakpoints |
| `.reload` | Reload symbols |
| `!analyze -v` | Crash analysis |

#### Kernel Structures

**GDT (Global Descriptor Table) Inspection:**

```
0: kd> r gdtr           ; Get GDT base address
gdtr=fffff80012345000

0: kd> dg 0 ff          ; Dump all GDT entries
                        P Si Gr Pr Lo
Sel    Base     Limit   res ys ze es ng Flags
---- -------- -------- - --- -- -- -- -------- 
0000 00000000 00000000 0 Nb By Np Nl 00000000
0008 00000000 ffffffff 0 Nb Bg P  Lo 00c09b00  ; Kernel Code
0010 00000000 ffffffff 0 Nb Bg P  Lo 00c09300  ; Kernel Data
```

**IDT (Interrupt Descriptor Table) Inspection:**

```
0: kd> r idtr           ; Get IDT base
idtr=fffff80012340000

0: kd> !idt             ; Dump interrupt handlers
Dumping IDT:
00:   fffff80012345678 nt!KiDivideErrorFault
01:   fffff80012345700 nt!KiDebugTrapOrFault
03:   fffff80012345800 nt!KiBreakpointTrap
0e:   fffff80012345900 nt!KiPageFault
```

**LDT (Local Descriptor Table):**

```
0: kd> r ldtr           ; Get LDT selector
ldtr=0000                ; Usually 0 (not used in modern Windows)
```

**PEB/TEB Inspection:**

```
0: kd> !peb             ; Process Environment Block
PEB at 000000007ffd9000
    InheritedAddressSpace:    No
    BeingDebugged:            Yes    ; <-- Anti-debug flag
    ImageBaseAddress:         00007ff600000000
    Ldr                       00007ffa12340000

0: kd> !teb             ; Thread Environment Block
TEB at 000000007ffd0000
    StackBase:            0000000000a00000
    StackLimit:           00000000009f0000
```

#### Process & Thread Analysis

```
0: kd> !process 0 0                  ; List all processes
0: kd> !process <eprocess> 7         ; Full process details
0: kd> !thread <ethread>             ; Thread details
0: kd> dt nt!_EPROCESS <addr>        ; Dump EPROCESS structure
0: kd> dt nt!_KPROCESS <addr>        ; Dump KPROCESS structure

; Handle table
0: kd> !handle 0 f                   ; All handles in current process

; VAD (Virtual Address Descriptor) tree
0: kd> !vad                          ; Memory regions
```

#### Memory Inspection

```
0: kd> !pte <address>            ; Page Table Entry
0: kd> !vtop 0 <virtual>         ; Virtual to physical translation
0: kd> !pool <address>           ; Pool allocation info
0: kd> !poolused                 ; Pool usage summary
0: kd> dc <address>              ; Display as chars
0: kd> dps <address>             ; Display as pointers with symbols
```

---

### 3.3 Memory Dumping with Scylla

#### x64dbg + Scylla Workflow

1. **Set Breakpoint at OEP:**
   ```
   bp VirtualProtect    ; Many packers call this before jumping to OEP
   ```

2. **Run and Identify OEP:**
   - Watch for section changes (`.text` execution)
   - Look for `POPAD` / `JMP` to OEP pattern

3. **Dump with Scylla:**
   - Plugins → Scylla
   - Click "IAT Autosearch"
   - Click "Get Imports"
   - Fix invalid/missing imports manually
   - "Dump" → "Fix Dump"

**Manual Scylla Process:**

| Step | Action |
|------|--------|
| 1 | Pause at OEP (unpacked entry point) |
| 2 | Scylla: Enter OEP address |
| 3 | IAT Autosearch → Get Imports |
| 4 | Verify import tree (red = invalid) |
| 5 | Dump process memory |
| 6 | Fix PE headers (Fix Dump button) |

**Command Line Dumping (Alternative):**

```powershell
# Using procdump
procdump.exe -ma <PID> dump.dmp

# Using comsvcs.dll (LOLBin)
rundll32.exe C:\Windows\System32\comsvcs.dll, MiniDump <PID> C:\dump.dmp full
```

> **Pro-Tip:** Use **ScyllaHide** plugin (x64dbg → Plugins → ScyllaHide) to bypass anti-debug. Enable: PEB.BeingDebugged, NtQueryInformationProcess, GetTickCount, and Hardware Breakpoint hooks. This allows stepping through protected malware without triggering detection.

---

## 4. Windows Anti-Analysis & Evasion

### 4.1 Debugger Detection Techniques

#### IsDebuggerPresent

```c
// Checks PEB.BeingDebugged flag
if (IsDebuggerPresent()) {
    ExitProcess(0);
}
```

**Assembly Equivalent:**

```assembly
; x64
mov rax, gs:[60h]          ; Get PEB address
movzx eax, byte ptr [rax+2]; PEB.BeingDebugged
test eax, eax
jnz debugger_detected

; x86
mov eax, fs:[30h]          ; Get PEB address
movzx eax, byte ptr [eax+2]; PEB.BeingDebugged
test eax, eax
jnz debugger_detected
```

**Bypass:**
```
; In x64dbg: Modify PEB.BeingDebugged
dump peb
; Set byte at offset 0x2 to 0x00
```

#### NtQueryInformationProcess

```c
// Query DebugPort (class 7)
DWORD debugPort = 0;
NtQueryInformationProcess(
    GetCurrentProcess(),
    ProcessDebugPort,    // 7
    &debugPort,
    sizeof(debugPort),
    NULL
);
if (debugPort != 0) {
    // Debugger attached
}

// Query DebugObjectHandle (class 30)
// Query DebugFlags (class 31) - returns inverse
```

**Detection Classes:**

| Class | Value | Description |
|-------|-------|-------------|
| ProcessDebugPort | 7 | Non-zero if debugger attached |
| ProcessDebugObjectHandle | 30 | Debug object handle |
| ProcessDebugFlags | 31 | 0 if being debugged |

#### PEB Structure Flags

| Offset (x64) | Field | Debug Indicator |
|--------------|-------|-----------------|
| 0x02 | BeingDebugged | 1 = debugged |
| 0x68 | NtGlobalFlag | 0x70 under debugger |
| 0xBC | ProcessHeap.Flags | Various flags set |
| 0xC0 | ProcessHeap.ForceFlags | Non-zero under debug |

**NtGlobalFlag Check:**

```assembly
; x64
mov rax, gs:[60h]          ; PEB
mov eax, [rax+68h]         ; NtGlobalFlag
and eax, 70h               ; FLG_HEAP_ENABLE_TAIL_CHECK |
                           ; FLG_HEAP_ENABLE_FREE_CHECK |
                           ; FLG_HEAP_VALIDATE_PARAMETERS
jnz debugger_detected
```

#### Hardware Breakpoint Detection

```c
CONTEXT ctx;
ctx.ContextFlags = CONTEXT_DEBUG_REGISTERS;
GetThreadContext(GetCurrentThread(), &ctx);

if (ctx.Dr0 || ctx.Dr1 || ctx.Dr2 || ctx.Dr3) {
    // Hardware breakpoints set
}
```

---

### 4.2 Anti-VM Techniques

#### MAC Address Detection

```c
// VMware MAC prefixes
// 00:0C:29, 00:50:56, 00:05:69

IP_ADAPTER_INFO adapterInfo[16];
DWORD bufLen = sizeof(adapterInfo);
GetAdaptersInfo(adapterInfo, &bufLen);

for (PIP_ADAPTER_INFO adapter = adapterInfo; adapter; adapter = adapter->Next) {
    if (adapter->Address[0] == 0x00 &&
        adapter->Address[1] == 0x0C &&
        adapter->Address[2] == 0x29) {
        // VMware detected
    }
}
```

**Common VM MAC Prefixes:**

| Vendor | MAC Prefix |
|--------|------------|
| VMware | 00:0C:29, 00:50:56, 00:05:69 |
| VirtualBox | 08:00:27 |
| Hyper-V | 00:15:5D |
| Parallels | 00:1C:42 |
| QEMU/KVM | 52:54:00 |

#### I/O Port Detection (VMware)

```assembly
; VMware backdoor I/O port
mov eax, 564D5868h      ; "VMXh" magic
mov ebx, 0
mov ecx, 0Ah            ; Get VMware version
mov edx, 5658h          ; "VX" port number
in eax, dx              ; Triggers #GP if not in VMware
; If no exception, we're in VMware
cmp ebx, 564D5868h
je vmware_detected
```

#### Registry Artifacts

```powershell
# Common VM registry keys
$vmIndicators = @(
    "HKLM:\SOFTWARE\VMware, Inc.\VMware Tools",
    "HKLM:\SOFTWARE\Oracle\VirtualBox Guest Additions",
    "HKLM:\SYSTEM\CurrentControlSet\Services\VBoxGuest",
    "HKLM:\SYSTEM\CurrentControlSet\Services\vmci",
    "HKLM:\HARDWARE\ACPI\DSDT\VBOX__",
    "HKLM:\HARDWARE\DESCRIPTION\System\BIOS" # Check SystemManufacturer
)

foreach ($key in $vmIndicators) {
    if (Test-Path $key) {
        Write-Host "VM Detected: $key"
    }
}
```

**WMI Queries:**

```powershell
# BIOS check
(Get-WmiObject Win32_BIOS).SerialNumber -match "VMware|VBOX|Virtual"

# System Model
(Get-WmiObject Win32_ComputerSystem).Model -match "VMware|VirtualBox"

# Disk drives
(Get-WmiObject Win32_DiskDrive).Model -match "VBOX|VMware|Virtual"
```

#### Process/Service Detection

```c
// Suspicious process names
const char* vmProcesses[] = {
    "vmtoolsd.exe", "vmwaretray.exe", "vmwareuser.exe",  // VMware
    "VBoxService.exe", "VBoxTray.exe",                    // VirtualBox
    "prl_tools.exe",                                       // Parallels
    "xenservice.exe",                                      // Xen
    NULL
};

HANDLE hSnap = CreateToolhelp32Snapshot(TH32CS_SNAPPROCESS, 0);
PROCESSENTRY32 pe32 = { sizeof(PROCESSENTRY32) };

if (Process32First(hSnap, &pe32)) {
    do {
        for (int i = 0; vmProcesses[i]; i++) {
            if (_stricmp(pe32.szExeFile, vmProcesses[i]) == 0) {
                // VM detected
            }
        }
    } while (Process32Next(hSnap, &pe32));
}
```

---

### 4.3 Timing Checks (RDTSC)

#### Basic RDTSC Detection

```assembly
rdtsc                       ; Read timestamp counter
mov esi, eax                ; Store low 32 bits
mov edi, edx                ; Store high 32 bits

; ... suspicious code block ...

rdtsc                       ; Second measurement
sub eax, esi                ; Calculate delta
sbb edx, edi

; If delta > threshold (e.g., 0x10000), debugger detected
cmp eax, 10000h
ja debugger_detected
```

**C Implementation:**

```c
#include <intrin.h>

BOOL TimingCheck() {
    ULONGLONG start = __rdtsc();
    
    // Decoy operations
    volatile int x = 0;
    for (int i = 0; i < 1000; i++) x++;
    
    ULONGLONG end = __rdtsc();
    ULONGLONG delta = end - start;
    
    // Normal execution: ~10000-50000 cycles
    // Under debugger: >> 500000 cycles (step-through)
    return (delta > 500000);
}
```

#### QueryPerformanceCounter Alternative

```c
LARGE_INTEGER start, end, freq;
QueryPerformanceFrequency(&freq);
QueryPerformanceCounter(&start);

// Suspicious operations

QueryPerformanceCounter(&end);
double elapsed_ms = (double)(end.QuadPart - start.QuadPart) / freq.QuadPart * 1000;

if (elapsed_ms > 100.0) {  // Threshold
    // Debugging detected
}
```

#### GetTickCount Evasion

```c
DWORD t1 = GetTickCount();
Sleep(100);
DWORD t2 = GetTickCount();

if ((t2 - t1) > 150) {  // Allow some variance
    // Possible debug stepping
}
```

> **Pro-Tip:** To defeat timing checks, use **TitanHide** (kernel-mode anti-anti-debug driver) or patch RDTSC by setting the TF bit in CR4 (requires kernel access). For usermode, ScyllaHide's "RDTSC" hook returns consistent delta values regardless of single-stepping delays.

---

## 5. Persistence & Execution Logic

### 5.1 Registry Persistence

#### Run/RunOnce Keys

| Key | Scope | Execution |
|-----|-------|-----------|
| `HKCU\Software\Microsoft\Windows\CurrentVersion\Run` | User | Every login |
| `HKCU\...\RunOnce` | User | Next login, then deleted |
| `HKLM\Software\Microsoft\Windows\CurrentVersion\Run` | System | Every login |
| `HKLM\...\RunOnce` | System | Next login, then deleted |
| `HKLM\...\RunOnceEx` | System | With error handling |

```powershell
# Detection
Get-ItemProperty "HKCU:\Software\Microsoft\Windows\CurrentVersion\Run"
Get-ItemProperty "HKLM:\Software\Microsoft\Windows\CurrentVersion\Run"

# Persistence installation (malware behavior)
Set-ItemProperty -Path "HKCU:\Software\Microsoft\Windows\CurrentVersion\Run" `
    -Name "WindowsUpdate" -Value "C:\Users\Public\malware.exe"
```

#### AppInit_DLLs

**Location:**
```
HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Windows\AppInit_DLLs
HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Windows\LoadAppInit_DLLs
```

**Behavior:** DLL loaded into every process linking `user32.dll`

```powershell
# Detection
Get-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Windows" |
    Select-Object AppInit_DLLs, LoadAppInit_DLLs, RequireSignedAppInit_DLLs
```

**Note:** Disabled by default in Windows 8+ (Secure Boot).

#### Winlogon Persistence

| Value | Path | Purpose |
|-------|------|---------|
| Userinit | `HKLM\...\Winlogon` | Runs after login (default: `userinit.exe,`) |
| Shell | `HKLM\...\Winlogon` | Desktop shell (default: `explorer.exe`) |
| Notify | `HKLM\...\Winlogon\Notify` | Notification packages (legacy) |

```powershell
# Detection
Get-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" |
    Select-Object Userinit, Shell

# Malicious modification
# Userinit = "C:\Windows\System32\userinit.exe, C:\malware.exe"
```

#### Image File Execution Options (IFEO)

```
HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\<target.exe>
```

| Value | Effect |
|-------|--------|
| Debugger | Runs specified debugger instead of target |
| GlobalFlag | Sets global flags for process |

```powershell
# Persistence via debugger hijack
New-Item "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\sethc.exe"
Set-ItemProperty -Path "HKLM:\...\sethc.exe" -Name "Debugger" -Value "C:\Windows\System32\cmd.exe"
```

---

### 5.2 Hijacking Techniques

#### COM Hijacking

**Mechanism:** Override COM class registration to load malicious DLL

**High-Value Targets:**

| CLSID | Description | Trigger |
|-------|-------------|---------|
| `{BCDE0395-E52F-467C-8E3D-C4579291692E}` | MMDeviceEnumerator | Audio playback |
| `{4590F811-1D3A-11D0-891F-00AA004B2E24}` | WBEMLocator | WMI queries |
| `{F3130CDB-AA52-4C3A-AB32-85FFC23AF9C1}` | NetBT | Network operations |

```powershell
# Detection: Find hijackable CLSIDs (HKCU takes precedence)
Get-ChildItem "HKCU:\Software\Classes\CLSID" -Recurse |
    Where-Object { $_.GetValue("") -ne $null }

# Compare against HKLM
$hijacked = Get-ChildItem "HKCU:\Software\Classes\CLSID" |
    Where-Object { Test-Path "HKLM:\Software\Classes\CLSID\$($_.PSChildName)" }
```

**Hijack Installation:**

```powershell
# Create HKCU override
$clsid = "{BCDE0395-E52F-467C-8E3D-C4579291692E}"
$path = "HKCU:\Software\Classes\CLSID\$clsid\InprocServer32"
New-Item -Path $path -Force
Set-ItemProperty -Path $path -Name "(Default)" -Value "C:\malicious.dll"
Set-ItemProperty -Path $path -Name "ThreadingModel" -Value "Both"
```

#### DLL Search Order Hijacking

**Windows DLL Search Order:**

1. Known DLLs (`HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\KnownDLLs`)
2. Application directory
3. System directory (`C:\Windows\System32`)
4. 16-bit system directory (`C:\Windows\System`)
5. Windows directory (`C:\Windows`)
6. Current working directory
7. PATH environment directories

**Attack Vector:** Place malicious DLL in application directory before system search.

```powershell
# Find vulnerable applications
# Look for missing DLLs in procmon with Result = "NAME NOT FOUND"

# Common targets
$vulnDLLs = @(
    "version.dll",    # Many apps load but don't verify
    "winmm.dll",
    "userenv.dll",
    "winspool.drv"
)
```

#### Phantom DLL Hijacking

**Target:** DLLs referenced in code but never installed (optional dependencies).

**Common Phantom DLLs:**

| DLL | Referenced By |
|-----|---------------|
| `wlbsctrl.dll` | Windows services |
| `wow64log.dll` | WoW64 subsystem |
| `fxsst.dll` | Fax service |
| `msdart.dll` | MDAC |

```powershell
# Detection: Search for phantom DLL loads
# Use Procmon with filters:
# Path contains ".dll"
# Result = "NAME NOT FOUND"
# Operation = "CreateFile"
```

---

### 5.3 Scheduled Tasks & WMI

#### Scheduled Tasks (schtasks)

**Creation Methods:**

```powershell
# CMD/PowerShell
schtasks /create /tn "SystemUpdate" /tr "C:\malware.exe" /sc onlogon /ru SYSTEM

# PowerShell native
$action = New-ScheduledTaskAction -Execute "C:\malware.exe"
$trigger = New-ScheduledTaskTrigger -AtLogOn
$principal = New-ScheduledTaskPrincipal -UserId "SYSTEM" -LogonType ServiceAccount
Register-ScheduledTask -TaskName "SystemUpdate" -Action $action -Trigger $trigger -Principal $principal
```

**Common Trigger Types:**

| Trigger | Description |
|---------|-------------|
| ONSTART | System boot |
| ONLOGON | User login |
| ONIDLE | System idle |
| ONEVENT | Event log trigger |
| DAILY/WEEKLY | Time-based |

**Detection:**

```powershell
# List all scheduled tasks
Get-ScheduledTask | Where-Object { $_.State -ne "Disabled" } |
    Select-Object TaskName, TaskPath, State |
    Format-Table -AutoSize

# Detailed task info
Get-ScheduledTask -TaskName "SystemUpdate" | Get-ScheduledTaskInfo

# XML export for analysis
Export-ScheduledTask -TaskName "SystemUpdate" -TaskPath "\"
```

**Locations:**

- Files: `C:\Windows\System32\Tasks\`
- Registry: `HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Schedule\TaskCache`

#### WMI Event Consumers

**Persistence Components:**

| Component | Purpose |
|-----------|---------|
| Event Filter | Defines triggering condition |
| Event Consumer | Action to execute |
| Binding | Links filter to consumer |

**Common Filters:**

```powershell
# Process creation trigger
$filter = Set-WmiInstance -Namespace "root\subscription" -Class "__EventFilter" -Arguments @{
    Name = "ProcessStartFilter"
    EventNamespace = "root\cimv2"
    QueryLanguage = "WQL"
    Query = "SELECT * FROM __InstanceCreationEvent WITHIN 5 WHERE TargetInstance ISA 'Win32_Process' AND TargetInstance.Name = 'notepad.exe'"
}
```

**Consumer Types:**

| Type | Capability |
|------|------------|
| CommandLineEventConsumer | Execute command |
| ActiveScriptEventConsumer | Run VBScript/JScript |
| LogFileEventConsumer | Write to log |
| NtEventLogEventConsumer | Write event log |
| SMTPEventConsumer | Send email |

**Malicious Subscription:**

```powershell
# CommandLine Consumer
$consumer = Set-WmiInstance -Namespace "root\subscription" -Class "CommandLineEventConsumer" -Arguments @{
    Name = "MaliciousConsumer"
    ExecutablePath = "C:\Windows\System32\cmd.exe"
    CommandLineTemplate = "/c C:\malware.exe"
}

# Binding
Set-WmiInstance -Namespace "root\subscription" -Class "__FilterToConsumerBinding" -Arguments @{
    Filter = $filter
    Consumer = $consumer
}
```

**Detection:**

```powershell
# List all WMI subscriptions
Get-WmiObject -Namespace "root\subscription" -Class "__EventFilter"
Get-WmiObject -Namespace "root\subscription" -Class "__EventConsumer"
Get-WmiObject -Namespace "root\subscription" -Class "__FilterToConsumerBinding"

# One-liner detection
Get-WmiObject -Namespace "root\subscription" -Class "__FilterToConsumerBinding" |
    ForEach-Object { 
        "$($_.Filter.Split('=')[1].Trim('"')) -> $($_.Consumer.Split('=')[1].Trim('"'))"
    }
```

**WMI Repository Location:**

```
C:\Windows\System32\wbem\Repository\
  MAPPING*.MAP
  OBJECTS.DATA
  INDEX.BTR
```

> **Pro-Tip:** For comprehensive persistence detection, use **Autoruns** (Sysinternals) with `-a *` flag, or the PowerShell-based **PersistenceSniper** module. WMI persistence survives reimaging if the Repository folder is preserved—always wipe `C:\Windows\System32\wbem\Repository\` during incident response.

---

## Quick Reference Tables

### Critical APIs for Malware Analysis

| Category | Win32 API | Native API | Indication |
|----------|-----------|------------|------------|
| Memory | VirtualAlloc | NtAllocateVirtualMemory | Shellcode staging |
| Memory | VirtualProtect | NtProtectVirtualMemory | RWX transition |
| Process | CreateProcess | NtCreateUserProcess | Child process |
| Process | OpenProcess | NtOpenProcess | Process access |
| Thread | CreateThread | NtCreateThreadEx | Code execution |
| Injection | WriteProcessMemory | NtWriteVirtualMemory | Remote write |
| File | CreateFile | NtCreateFile | Disk I/O |
| Registry | RegSetValue | NtSetValueKey | Persistence |
| Network | WSAConnect | - | C2 connection |

### Common Malware Indicators by Section

| Section | Suspicious Characteristic |
|---------|---------------------------|
| .text | Entropy > 7.0, writable flag |
| .data | Executable flag, high entropy |
| .rsrc | PE signature inside, high entropy |
| .reloc | Missing in packed binary |
| UPX0/1 | Packing indicator |
| .vmp | VMProtect virtualization |

### Anti-Debug Quick Bypass (x64dbg)

| Check | Bypass Method |
|-------|---------------|
| IsDebuggerPresent | ScyllaHide or patch PEB+0x2 |
| NtGlobalFlag | Patch PEB+0x68 to 0 |
| DebugPort | ScyllaHide NtQueryInformationProcess |
| RDTSC | ScyllaHide timing hooks |
| Hardware BPs | Use software BPs instead |
| INT 2D | ScyllaHide exception hooks |

---

*Document maintained by Windows Security Research Team*  
*For authorized security research and malware analysis only*
