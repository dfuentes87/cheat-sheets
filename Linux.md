# Linux

## Linux tools I should use more often

| Task                      | Command                                |
| :---                      | :---                                   |
| Watch command output      | `watch -n SECONDS 'cmd1 \| cmd2'`       |

## Network configuration

Easy mode: Use `nmtui`

Manual mode: Use `nmcli`

| Action                             | Command                                       |
| :---                               | :---                                          |
| Set IP address of an interface     | `nmcli con modify 'ens192' ipv4.method manual ipv4.addresses 192.168.1.181/24 gw4 192.168.1.1` |
| Set DNS Servers                    | `nmcli con modify 'ens192' ipv4.dns 4.2.2.2`  |

### Resources

* [Fedora Wiki: Networking/CLI](https://fedoraproject.org/wiki/Networking/CLI)

## Managing services

| Action                                      | Command                                          |
| :---                                        | :---                                             |
| List services                               | `systemctl list-units --type service`            |
| List failed services on boot                | `sudo systemctl --failed`                        |
| Kill service with SIGTERM (15)              | `sudo systemctl kill SERVICE.service`            |
| Kill service with SIGKILL (9)               | `sudo systemctl kill -s SIGKILL SERVICE.service` |

## Targets (runlevels)

| Action                     | Command                                  |
| :---                       | :---                                     |
| Go to single user mode     | `systemctl rescue`                       |
| Go to multi-user mode      | `systemctl isolate multi-user.target`    |
| Go to graphical level      | `systemctl isolate graphical.target`     |
| Get default runlevel       | `systemctl get-default`                  |
| Set default runlevel       | `systemctl set-default graphical.target` |
| Shutdown                   | `systemctl poweroff`                     |
| Reboot, suspend, hibernate | `systemctl STATE`                        |

### Resources

* [How to Change Runlevels (targets) in SystemD](https://www.tecmint.com/change-runlevels-targets-in-systemd/)
* [Systemd for Administrators, Part IV: Killing Services](http://0pointer.de/blog/projects/systemd-for-admins-4.html)

## Journalctl

| Action                               | Command                                                       |
| :---                                 | :---                                                          |
| Show log since last boot             | `journalctl -b`                                               |
| Kernel messages (like `dmesg`)       | `journalctl -k`                                               |
| Show latest log and tail             | `journalctl -f`                                               |
| Show only errors and worse           | `journalctl -b -p err`                                        |
| Filter on time (relative)            | `journalctl --since "1 hour ago"`                             |
| Filter on time (example)             | `journalctl --since=2014-06-00 --until="2014-06-07 12:00:00"` |
| Since yesterday                      | `journalctl --since=yesterday`                                |
| Show only log of SERVICE             | `journalctl -u SERVICE`                                       |
| Match executable, e.g. `dhclient`    | `journalctl /usr/sbin/dhclient`                               |
| Match device node, e.g. `/dev/sda`   | `journalctl /dev/sda`                                         |

### "Traditional" logs

Traditionally, logs are text files in `/var/log`. Some services still write their logs to these text files and not to journald.

| Action                                      | Command                 |
| :---                                        | :---                    |
| Colorized live view of boot/kernel messages | `dmesg -wH`             |

### Resources

* [Systemd for Administrators, Part XVII: Using the journal](http://0pointer.de/blog/projects/journalctl.html)

## Configuring the firewall with `firewalld`

The `firewalld-cmd` should run with root privileges, do always use `sudo`.

| Action                           | Command                        |
| :---                             | :---                           |
| List supported zones             | `firewall-cmd --get-zones`     |
| List preconfigured services      | `firewall-cmd --get-services`  |
| Enabled features in current zone | `firewall-cmd --list-all`      |
| Turn panic mode on               | `firewall-cmd --panic-on`      |
| Turn panic mode off              | `firewall-cmd --panic-off`     |

* Configuration is stored in `/etc/firewalld` and `/usr/lib/firewalld`
* The default zone is `public`, which you don't have to specify on the command line when adding/removing rules

### Resources

* [FirewallD in the Fedora Project Wiki](https://fedoraproject.org/wiki/FirewallD#Using_firewall-cmd)
* [Deprecated Linux networking commands and their replacements](https://dougvitale.wordpress.com/2011/12/21/deprecated-linux-networking-commands-and-their-replacements/)
