# Windows Shell Survival Guide

Useful PowerShell commands for Linux sysadmins.

## Basic operations

| Task                     | Command                                          |
| :---                     | :---                                             |
| Shutdown                 | `Stop-Computer`                                  |
| Reboot                   | `Restart-Computer`                               |

## Network interfaces

| Task                    | Command                                             |
| :---                    | :---                                                |
| List adapters           | `Get-NetAdapter`                                    |
| Get connection profiles | `Get-NetAdapter \| Get-NetConnectionProfile`        |
| Get IP addresses        | `Get-NetIPAddress`                                  |
| Get routing table       | `Get-NetRoute`                                      |
| DNS servers             | `Get-DnsClientServerAddress`                        |
| "netstat"               | `Get-NetTCPConnection`                              |
| Troubleshooting         | `Test-NetConnection`                              |

## Services

| Task                      | Command                                                            |
| :---                      | :---                                                               |
| List all services         | `Get-Service \| Format-Table -autosize`                           |
| View single SERVICE       | `Get-Service \| Where name -eq SERVICE \| Format-list`            |
| Start SERVICE             | `Start-Service -name SERVICE`                                      |
| Stop SERVICE              | `Stop-Service -name SERVICE`                                       |
| SERVICE startup type      | `Get-WMIObject Win32_Service \| where Name -eq SERVICE`            |
| Start SERVICE at boot    | `Set-Service -name SERVICE -StartupType Automatic`                 |
| Disable start SERVICE at boot    | `Set-Service -name SERVICE -StartupType Disabled`           |

## Firewall

| Task               | Command                                                                                          |
| :---               | :---                                                                                             |
| Get FW state       | `Get-NetFirewallProfile -PolicyStore ActiveStore`                                                |
| Disable firewall   | `Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled False`                           |
| Enable firewall    | `Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled True`                            |
| Allow RDP          | `Get-NetFirewallRule -DisplayName "Remote Desktop*" \| Set-NetFirewallRule -enabled true`        |

## Active Directory

| Task                    | Command             |
| :---                    | :---                |
| TODO                    | `TODO`              |
