# Initial network configuration on Windows Server Core

First of all, we need to list all available interfaces:
```
Get-NetAdapter
```
When you get *ifIndex*`es of interfaces configure DHCP-client:
```
Remove-NetRoute -Ifindex 3
Set-NetIPInterface -Dhcp Enabled
```
Or static IP configuration:
```
Remove-NetRoute -Ifindex 3
New-NetIPAddress -InterfaceIndex 5 -IPAddress 192.168.99.202 -PrefixLength 24 -DefaultGateway 192.168.99.1
$DNSSet = @{
    InterfaceIndex = 8
    ServerAddresses = ("8.8.8.8","8.8.4.4")
}
Set-DnsClientServerAddress $DNSSet

Test-NetConnection
Resolve-DnsName google.com
```
Dont foreget to complete License Key activation
```
sconfig
```
Initial network configuration is done. Go to [next step](/3_DHCPandDNSConfig.md)

# Useful links

* [Get-NetAdapter][def]
* [Set-NetIPInterface][def2]
* [New-NetIPAdress][def3]
* [Set-DnsClientServerAddress][def4]
* [Test-NetConnection][def5]
* [Resolve-DnsName][def6]
* Get-Command -Module NetTCPIP

[def]: https://learn.microsoft.com/en-us/powershell/module/netadapter/get-netadapter
[def2]: https://learn.microsoft.com/en-us/powershell/module/nettcpip/set-netipinterface
[def3]: https://learn.microsoft.com/en-us/powershell/module/nettcpip/new-netipaddress
[def4]: https://learn.microsoft.com/en-us/powershell/module/dnsclient/set-dnsclientserveraddress
[def5]: https://learn.microsoft.com/en-us/powershell/module/nettcpip/test-netconnection
[def6]: https://learn.microsoft.com/en-us/powershell/module/dnsclient/resolve-dnsname

# Navigation
[<-Previous](/1_rightAfterInstall.md)---[Next->](/3_domainController.md)