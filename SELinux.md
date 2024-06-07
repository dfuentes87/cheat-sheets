# SELinux

## Booleans

| Action                     | Command                           |
| :---                       | :---                              |
| List all booleans          | `getsebool -a`                    |
| List specific booleans     | `getsebool -a \| grep PATTERN` |
| Set boolean (until reboot) | `setsebool BOOLEAN VALUE`         |
|                            | `VALUE` can be 0/false or 1/true  |
| Set boolean permanently    | `setsebool -P BOOLEAN VALUE`      |

## Contexts and labels

| Action                                           | Command                                 |
| :---                                             | :---                                    |
| List context of files                            | `ls -Z`                                 |
| Change context of a file                         | `chcon -t CONTEXT FILE`                 |
| Change context of a directory (recursively)      | `chcon -R -t CONTEXT DIR`               |
| Reset file context to default                    | `restorecon -v FILE`                    |
| Reset directory context to default (recursively) | `restorecon -v -R DIR`                  |
| Set default context of a directory               | `semanage fcontext -a -t CONTEXT REGEX` |

**Examples:**

Set default context of /srv/web and its content (recursively) to httpd_sys_content_t:
`semanage fcontext -a -t httpd_sys_content_t "/srv/web(/.*)?"`

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
| Resets the original context of a directory       | `restorecon -vR /var/www/html/`        |                                        |


### Resources

* [SELINUX USER'S AND ADMINISTRATOR'S GUIDE](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7-beta/html/selinux_users_and_administrators_guide/index)


## Creating a new SELinux policy module

TODO

## Troubleshooting

- Is SELinux enforcing?
    - `getenforce`, `sestatus`
- Is SELinux blocking something?
    - `sudo grep denied /var/log/audit/audit.log`
- List booleans for specific service (e.g. `http`):
    - `getsebool -a | grep http`
- Check SELinux context of files/directories
    - `ls -Z`

## Resources

- RedHat, *RHEL 7 SELinux User's and Administrator's Guide,* <https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/selinux_users_and_administrators_guide/>
- Thomas Cameron, *SELinux for mer mortals*, <https://youtu.be/bQqX3RWn0Yw>
