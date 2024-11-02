# How to install secondary DNS server

Previously, we installed a DNS server together with a domain controller,
now we will install a second server to increase fault tolerance on DC2:
```
Add-WindowsFeature DNS
Resolve-DnsName DC2 -Server dc2.test.local
```
Then we need to create some DNS-records:
```

```

# Configure DHCP dynamic updates