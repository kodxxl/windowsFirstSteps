# First steps you need to do right after Windows Core installation

Right after system restart, set up new password for Administrator and then go to:
```
powershell
```
You need to activate Windows Core with valid License key:
```
sconfig
11 - 2 - 3 (enter key - OK) 4 - 15 (rest part of activation after network configuration)
```
Rename current computer with following command:
```
Rename-Computer dc1 -Restart
```
After reboot your system is ready to further costomization
[go to next section](/2_networkConfiguration.md)

If you need prepare VM Template:
```
C:\Windows\System32\Sysprep\sysprep.exe
```

# Useful links

* [Rename-Computer][def]
* [Microsoft Powershell Management][def2]

[def]: https://learn.microsoft.com/ru-ru/powershell/module/microsoft.powershell.management/rename-computer
[def2]: https://learn.microsoft.com/ru-ru/powershell/module/microsoft.powershell.management

# Navigation
[<-Previous](/README.md)---[Next->](/2_networkConfiguration.md)