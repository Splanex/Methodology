## Find signature

ThreatCheck (https://github.com/rasta-mouse/ThreatCheck)

	ThreatCheck.exe -f evil.exe -e Defende
	ThreatCheck.exe -f evil.exe -e AMSI

DefenderCheck (https://github.com/matterpreter/DefenderCheck)

AMSITrigger (https://github.com/RythmStick/AMSITrigger)

Find-AVSignature (https://github.com/PowerShellMafia/PowerSploit/blob/master/AntivirusBypass/Find-AVSignature.ps1)
## Static Code-Based
- Splitting and merging objects
- Removing and obscuring Identifiable Information
## Static Property-Based
- File properties (including file hash, entropy, author, name)
- File Hash -> Bit Flipping
- Entropy -> Use random english words instead of rubbish

### Bit Flipping

```
import sys

orig = list(open(sys.argv[1], "rb").read())

i = 0
while i < len(orig):
	current = list(orig)
	current[i] = chr(ord(current[i]) ^ 0xde)
	path = "%d.exe" % i
	
	output = "".join(str(e) for e in current)
	open(path, "wb").write(output)
	i += 1
	
print("done")
```

Verify working variants
```
FOR /L %%A IN (1,1,10000) DO (
	signtool verify /v /a flipped\\%%A.exe
)
```

### Entropy
Cyberchef

## Behavioral Signatures

Create Structures for known API calls

```
// 1. Define the structure of the call
typedef BOOL (WINAPI* myNotGetComputerNameA)(
	LPSTR   lpBuffer,
	LPDWORD nSize
);
// 2. Obtain the handle of the module the call address is present in 
HMODULE hkernel32 = LoadLibraryA("kernel32.dll");
// 3. Obtain the process address of the call
myNotGetComputerNameA notGetComputerNameA = (myNotGetComputerNameA) GetProcAddress(hkernel32, "GetComputerNameA");
```

Another Example

```
int WSAAPI WSAConnect(
  [in]  SOCKET         s,
  [in]  const sockaddr *name,
  [in]  int            namelen,
  [in]  LPWSABUF       lpCallerData,
  [out] LPWSABUF       lpCalleeData,
  [in]  LPQOS          lpSQOS,
  [in]  LPQOS          lpGQOS
);
```

Turns into

```
typedef int(WSAAPI* WSACONNECT)(
	SOCKET s,
	const struct sockaddr *name,
	int namelen,
	LPWSABUF lpCallerData,
	LPWSABUF lpCalleeData,
	LPQOS lpSQOS,
	LPQOS lpGQOS
);


HMODULE hws2_32 = LoadLibraryW(L"ws2_32");
WSACONNECT myWSAConnect = (WSACONNECT) GetProcAddress(hws2_32, "WSAConnect");
```