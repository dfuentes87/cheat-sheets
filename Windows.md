# Windows Shell Survival Guide

Useful PowerShell and cmd-shell commands for Linux sysadmins.

## Basic operations

| Task                     | Command                                          |
| :---                     | :---                                             |
| Shutdown                 | `Stop-Computer`                                  |
| Reboot                   | `Restart-Computer`                               |

## Network interfaces

| Task                    | Command                                                                          |
| :---                    | :---                                                                             |
| List adapters           | `Get-NetAdapter`                                                                 |
| Get connection profiles | `Get-NetAdapter `&#124;` Get-NetConnectionProfile`                                      |
| Get IP addresses        | `Get-NetIPAddress`                                                               |
| Get routing table       | `Get-NetRoute`                                                                   |
| DNS servers             | `Get-DnsClientServerAddress`                                                     |
| "netstat"               | `Get-NetTCPConnection`                                                           |
| Troubleshooting         | [`Test-NetConnection`](http://technet.microsoft.com/en-us/library/dn372891.aspx) |

## Services

| Task                      | Command                                                            |
| :---                      | :---                                                               |
| List all services         | `Get-Service `&#124;` Format-Table -autosize`                      |
| View single SERVICE       | `Get-Service `&#124;` Where name -eq SERVICE `&#124;` Format-list` |
| Start SERVICE             | `Start-Service -name SERVICE`                                      |
| Stop SERVICE              | `Stop-Service -name SERVICE`                                       |
| SERVICE startup type      | `Get-WMIObject Win32_Service `&#124;` where Name -eq SERVICE`        |
| Start SERVICE at boot    | `Set-Service -name SERVICE -StartupType Automatic`                 |
| Disable start SERVICE at boot    | `Set-Service -name SERVICE -StartupType Disabled`                  |

## Firewall

| Task               | Command                                                                                          |
| :---               | :---                                                                                             |
| Get FW state       | `Get-NetFirewallProfile -PolicyStore ActiveStore`                                                |
| Disable firewall   | `Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled False`                           |
| Enable firewall    | `Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled True`                            |
| Allow RDP          | `Get-NetFirewallRule -DisplayName "Remote Desktop*" `&#124;` Set-NetFirewallRule -enabled true`  |

## Active Directory

| Task                    | Command                                                 |
| :---                    | :---                                                    |
| TASK           | `COMMAND`      |

