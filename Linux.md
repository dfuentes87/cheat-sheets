# Linux

## Linux tools I should use more often

| Task                      | Command                                |
| :---                      | :---                                   |
| Watch command output      | watch -n SECONDS 'cmd1 \| cmd2'       |

## Network configuration

Easy mode: Use `nmtui`

| Action                             | Command                                       |
| :---                               | :---                                          |
| Set IP address of an interface*    | `ip address add 192.168.56.1/24 dev vboxnet0` |


### Host name

There are *three* kinds of host names:

- Static: "traditional" host name, stored in `/etc/hostname`
- Transient: dynamic, set in kernel. Default value is the static host name, can be set by e.g. DHCP or mDNS.
- Pretty: free form, for presentation to the user. Default value is the static host name.

| Action                 | Command                                         |
| :---                   | :---                                            |
| Get host names        | `hostnamectl`                                   |
| Set (all) host names   | `hostnamectl set-hostname HOSTNAME`             |
| Set specific host name | `hostnamectl set-hostname --static HOSTNAME`    |
|                        | `hostnamectl set-hostname --transient HOSTNAME` |
|                        | `hostnamectl set-hostname --pretty HOSTNAME`    |

### Resources

* [RedHat Enterprise Linux 7 Networking Guide](https://access.redhat.com/site/documentation/en-US/Red_Hat_Enterprise_Linux/7-Beta/html/Networking_Guide/index.html)
* [Fedora Wiki: Networking/CLI](https://fedoraproject.org/wiki/Networking/CLI)


## Managing services with `systemctl`

| Action                                      | Command                                          |
| :---                                        | :---                                             |
| List services                               | `systemctl list-units --type service`            |
| List failed services on boot                | `sudo systemctl --failed`                        |
| *Kill* SERVICE (all processes) with SIGTERM | `sudo systemctl kill SERVICE.service`            |
| *Kill* SERVICE (all processes) with SIGKILL | `sudo systemctl kill -s SIGKILL SERVICE.service` |

## Runlevels

Run with root privileges (`sudo`)

| Action                     | Command                                  |
| :---                       | :---                                     |
| Go to single user mode     | `systemctl rescue`                       |
| Go to multi-user mode      | `systemctl isolate multi-user.target`    |
| (= old runlevel 3)         | `systemctl isolate runlevel3.target`     |
| Go to graphical level      | `systemctl isolate graphical.target`     |
| Get default runlevel       | `systemctl get-default`                  |
| Set default runlevel       | `systemctl set-default graphical.target` |
| Shutdown                   | `systemctl poweroff`                     |
| Reboot, suspend, hibernate | `systemctl STATE`                        |

### Resources

* [RedhHat 7 System Administrator's Guide](https://access.redhat.com/site/documentation/en-US/Red_Hat_Enterprise_Linux/7-Beta/html/System_Administrators_Guide/sect-Managing_Services_with_systemd-Services.html)
* [Systemd for Administrators, Part IV: Killing Services](http://0pointer.de/blog/projects/systemd-for-admins-4.html)

## Perusing system logs

| Action                               | Command                                                       |
| :---                                 | :---                                                          |
| Show log since last boot             | `journalctl -b`                                               |
| Kernel messages (like `dmesg`)       | `journalctl -k`                                               |
| Show latest log and wait for changes | `journalctl -f`                                               |
| Reverse output (newest first)        | `journalctl -r`                                               |
| Show only errors and worse           | `journalctl -b -p err`                                        |
| Filter on time (example)             | `journalctl --since=2014-06-00 --until="2014-06-07 12:00:00"` |
| Since yesterday                      | `journalctl --since=yesterday`                                |
| Show only log of SERVICE             | `journalctl -u SERVICE`                                       |
| Match executable, e.g. `dhclient`    | `journalctl /usr/sbin/dhclient`                               |
| Match device node, e.g. `/dev/sda`   | `journalctl /dev/sda`                                         |

### "Traditional" logs

Traditionally, logs are text files in `/var/log`. Some services still write their logs to these text files and not to journald.

| Action                                      | Command                 |
| :---                                        | :---                    |
| Live view of log FILE                       | `tail -f /var/log/FILE` |
| Colorized live view of boot/kernel messages | `dmesg -wH`             |

### Resources

* [Systemd for Administrators, Part XVII: Using the journal](http://0pointer.de/blog/projects/journalctl.html)

## Configuring the firewall with `firewalld`

The `firewalld-cmd` should run with root privileges, do always use `sudo`.

| Action                           | Command                                                          |
| :---                             | :---                                                             |
| List supported zones             | `firewall-cmd --get-zones`                                       |
| List preconfigured services      | `firewall-cmd --get-services`                                    |
| Enabled features in current zone | `firewall-cmd --list-all`            |
| Turn panic mode on               | `firewall-cmd --panic-on`                                        |
| Turn panic mode off              | `firewall-cmd --panic-off`                                       |
|                                  |                                                                  |

* Configuration is stored in `/etc/firewalld` and `/usr/lib/firewalld`
* The default zone is `public`, which you don't have to specify on the command line when adding/removing rules


### Resources

* [Using Firewalls, in *RHEL 7 Security Guide*](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Security_Guide/sec-Using_Firewalls.html)
* [FirewallD, in *Fedora Project Wiki*](https://fedoraproject.org/wiki/FirewallD#Using_firewall-cmd)
* [Deprecated Linux networking commands and their replacements](https://dougvitale.wordpress.com/2011/12/21/deprecated-linux-networking-commands-and-their-replacements/)

## Managing SELinux

| Action                                           | Command                                |                    
| :---                                             | :---                                   |  
| Set individual domain permissive                 | `semanage permissive -a httpd_t`       |             
| Mappings between SELinux and Linux user accounts | `semanage login -l`                    |
| SELinux context of processes                     | `ps -eZ`                               |                          
| SELinux context associated with your user        | `id -Z`                                |                 
| Show all booleans                                | `getsebool -a`                         |
| Turn off boolean                                 | `setsebool [boolean] 0`                |
| Turn on boolean                                  | `setsebool [boolean] 1`                |
| Make boolean permanent                           | `setsebool -P [boolean] [0\|1]`         |
| Change SELinux context for a desired folder      | `chcon -t httpd_sys_content_t /var/www/html/index.html` |
| Resets the original context of a directory       | `restorecon -vR /var/www/html/`        |
|                                                  |                                        |


### Resources

* [SELINUX USER'S AND ADMINISTRATOR'S GUIDE](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7-beta/html/selinux_users_and_administrators_guide/index)
