# User Management

| COMMAND                               | DESCRIPTION                            |
| ------------------------------------- | -------------------------------------- |
| `sudo adduser username`               | Create a new user                      |
| `sudo userdel username`               | Delete a user                          |
| `sudo usermod -aG groupname username` | Add a user to group                    |
| `sudo deluser username groupname`     | Remove a user from a group             |
| `groups username`                     | Check user group privleges             |
| `id username`                         | View user ID, group ID, and groups     |
| `cat /etc/passwd`                     | View users on the system               |
| `cat /etc/group`                      | View groups on the system              |
| `su username`                         | Switch User                            |
| `su -l username`                      | Switch Users Temporarily (login shell) |
