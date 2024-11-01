# How to setup and tune DHCP server

Prepare new dedicated server for DHCP role, join it into Domain:
```
Add-Computer -DomainName test.local
```
Then, install DHCP service:
```
Get-WindowsFeature | ? InstallState -like Installed
Install-WindowsFeature DHCP
```
Add necessery security groups, and set configuration state to configured
```
Add-DhcpServerSecurityGroup -Verbose

$IV = @{
    Path = 'HKLM:\SOFTWARE\Microsoft\ServerManager\Roles\12'
    Name = 'ConfigurationState'
    Value = 2
}
Set-ItemProperty @IV
```
Authorize newly created DHCP server on Domain controller
```
Add-DhcpServerInDc -DnsName dc1.test.local
Restart-Service DHCPServer -Force
Get-DhcpServerInDc
```

# Configure DHCP scopes

First of all, we need to create new scope:
```
$S100 = @{
    Name = ''
    StartRange = '192.168.0.100'
    EndRange = '192.168.0.200'
    SubnetMask = '255.255.255.0'
    LeaseDuration = '1:00:00'
}
Add-DhcpServerv4Scope @S100
Get-DhcpServerv4Scope
```
Then you may set Option values for Sever and separate scopes:
```
$ServerOV = @{
    DnsDomain = 'test.local'
    DnsServer = '192.168.0.10'
}
Set-DhcpServerv4OptionValue @ServerOV
Set-DhcpServerv4OptionValue -ScopeId 192.168.0.0 -OptionId 51 -Value 3600
Get-DhcpServerv4OptionValue
Get-DhcpServerv4OptionValue -ScopeId 192.168.0.0
```

# How to setup failover DHCP and DNS servers

Install and configure empty DHCP server on the other server, then:
```
$FailoverDC = @{
    ComputerName = 'dc1'
    PartnerServer = 'dc2'
    Name = 'dc1-dc2'
    ScopeId = '192.168.0.0'
    LoadBalancePercent = 60
    SharedSecret = 'P@$$\/\/0Rd'
    Force = $true
}
Add-DhcpServerv4Failover @FailoverDC
Get-DhcpServerv4Failover
```
# Useful links

* ```Get-Command -Module DhcpServer```
* [Add-DhcpServerSecurityGroup](https://learn.microsoft.com/en-us/powershell/module/dhcpserver/add-dhcpserversecuritygroup)
* [Add-DhcpServerInDc](https://learn.microsoft.com/en-us/powershell/module/dhcpserver/add-dhcpserverindc)
* [Add-DhcpServerv4Scope](https://learn.microsoft.com/en-us/powershell/module/dhcpserver/add-dhcpserverv4scope)
* [Set-DhcpServerv4OptionValue](https://learn.microsoft.com/en-us/powershell/module/dhcpserver/set-dhcpserverv4optionvalue)
* [Add-DhcpServerv4Failover](https://learn.microsoft.com/en-us/powershell/module/dhcpserver/add-dhcpserverv4failover)

# Navigation
[<-Previous](/3_domainController.md)---[Next->](/5_configureDNS.md)
