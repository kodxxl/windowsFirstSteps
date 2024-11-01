# Installing and configuring Domain controller

First of all we need to install ADDS feature, and tools
```
Install-WindowsFeature AD-Domain-Service -IncludeManagementTools
```
Before installing a domain forrest, you need to decide on some parameters:
```
$PTPass = @{
    String = 'Pa$$w0rd'
    AsPlainText = $true
    Force = $true
}
$SPass = ConvertTo-SecureString @PTPass
$FP = @{
    DomainName = 'test.local'
    SafeModeAdministratorPassword = $SPass
    InstallDNS = $true
    DomainMode = 'WinThreshold'
    ForestMode = 'WinThreshold'
    Force = $true
}
Install-ADDSForest @FP
```
After all, restart server, and try to verify installation:
```
Get-ADRootDSE
Resolve-DNSName dc1 -Server dc1
$Env:USERNAME
$Env:USERDNSDOMAIN
```

# Creating ADDS users and administrators
Create OU for IT department:
```
$OU = @{
    Name = 'IT'
    DisplayName = 'IT dept.'
    Path = 'DC=test,DC=local'
}

New-ADOrganizationalUnit @OU
```
Prepare password and initial settings for new user
```
$SPass = ConvertTo-SecureString -String 'P@$$w0rD' -AsPlainText -Force
$User = @{}
$User.AccountPassword = $SPass
$User.Enabled = $true
$User.PasswordNeverExpires = $true
$User.ChangePasswordAtLogon = $false
$User.SamAccountName = 'user1'
$User.PrincipalName = 'user1@test.local'
$User.Name = 'User One'
$User.DisplayName = 'User One (IT dept.)'

New-ADUser @User
```
Move new user into Domain Administrators group
``
$Admin = Get-ADUser -Identity 'user1'
Add-ADGroupMember -Identity 'Domain Administrators' -Members $Admin 
``
Create new group for rest users:
```
$UG = @{}
$UG.Name = 'Team'
$UG.Path = 'DC=test,DC=local'
$UG.Descriptions = 'Some Team'
$UG.GroupScope = 'DomainLocal'

New-ADGroup @UG
```
Now we can create new users and add them into Team

# Useful links

* ```Get-Command -Module ADDSDeployment```
* ```Get-Command -Module ActiveDirectory```
* [ConvertTo-SecureString](https://learn.microsoft.com/ru-ru/powershell/module/microsoft.powershell.security/convertto-securestring)
* [Install-ADDSForest](https://learn.microsoft.com/en-us/powershell/module/addsdeployment/install-addsforest)
* [Get-ADRootDSE](https://learn.microsoft.com/en-us/powershell/module/activedirectory/get-adrootdse)
* [New-ADOrganizationalUnit](https://learn.microsoft.com/en-us/powershell/module/activedirectory/new-adorganizationalunit)
* [New-ADUser](https://learn.microsoft.com/en-us/powershell/module/activedirectory/new-aduser)
* [Get-ADUser](https://learn.microsoft.com/en-us/powershell/module/activedirectory/get-aduser)
* [New-ADGroup](https://learn.microsoft.com/en-us/powershell/module/activedirectory/new-adgroup)
* [Add-ADGroupMember](https://learn.microsoft.com/en-us/powershell/module/activedirectory/add-adgroupmember)

# Navigation
[<-Previous](/2_networkConfiguration.md)---[Next->](/4_configureDHCP.md)
