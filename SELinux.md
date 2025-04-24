# SELinux

## Contexts and labels

| Action                                           | Command                                 |
| :---                                             | :---                                    |
| List context of files                            | `ls -Z`                                 |
| Change context of a file                         | `chcon -t CONTEXT FILE`                 |
| Change context of a directory (recursively)      | `chcon -R -t CONTEXT DIR`               |
| Reset file context to default                    | `restorecon -v FILE`                    |
| Reset directory context to default (recursively) | `restorecon -v -R DIR`                  |
| Set default context of a directory               | `semanage fcontext -a -t CONTEXT REGEX` |

### Examples:

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
| Make boolean permanent                           | `setsebool -P [boolean] [0\|1]`        |
| Change SELinux context for a desired folder      | `chcon -t httpd_sys_content_t /var/www/html/index.html` |
| Resets the original context of a directory       | `restorecon -vR /var/www/html/`        |

### Resources

* [Using SELinux](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html-single/using_selinux/index)

## Creating a new SELinux policy module

TODO

## Troubleshooting

Is SELinux enforcing?

`getenforce`, `sestatus`

Is SELinux blocking something?

`sudo grep denied /var/log/audit/audit.log`

List booleans for specific service (e.g. `http`):

`getsebool -a | grep http`

Check SELinux context of files/directories
`ls -Z`

## Resources

* [Using SELinux](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html-single/using_selinux/index)
