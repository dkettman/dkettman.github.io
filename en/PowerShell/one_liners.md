# PowerShell Secret Vault
Powershell has a built-in secret vault! Who knew? I didn't until I stumbled upon it.

## Setup
To Setup the PowerShell Secret Vault

# Setup Network Interfaces
## Turn of DHCP On an interface
```pwsh
Get-Netadapter -InterfaceIndex 6 | Set-NetIPInterface -Dhcp Disabled -PolicyStore activestore
```

## Set IP address and Gateway
```pwsh
New-NetIPAddress -InterfaceIndex 6 -IPAddress 172.31.1.207  -PrefixLength 24 -DefaultGateway 172.31.1.1
```

## Set DNS Servers
```pwsh
Set-DnsClientServerAddress -InterfaceIndex 6 -ServerAddresses @("172.31.1.10")
```

## Enable Remote Desktop
```powershell
Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server' -name "fDenyTSConnections" -value 0
```
