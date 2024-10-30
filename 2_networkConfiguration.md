# Initial network configuration on Windows Server Core
---

First of all, we need to ist all available interfaces:
```
[Get-NetAdapter](https://learn.microsoft.com/en-us/powershell/module/netadapter/get-netadapter)
```
Then you get ifIndex`es of interfaces