PiVPN is a set of shell scripts developed to easily turn your Raspberry Pi (TM) into a VPN server using two free, open-source protocols:

- WireGuard
- OpenVPN

This script's primary mission in life is to allow a user to have as cost-effective as possible VPN at home without being a technical wizard with a one-command installer followed by easy management of the VPN with the 'pivpn' command.

# Installation
## Method 1

```Shell
curl -L https://install.pivpn.io | bash
```

## Method 2 (direct link)

```Shell
curl https://raw.githubusercontent.com/pivpn/pivpn/master/auto_install/install.sh | bash
```

## Method 3 (clone repo)

```Shell
git clone https://github.com/pivpn/pivpn.git
bash pivpn/auto_install/install.sh
```


>*After completion of any of the above installation methods, ensure to port forward the chosen port that the VPN application is hosted on.* 

---
# PiVPN WireGuard Commands
## List of commands

```
-a,  add              Create a client conf profile"
-c,  clients          List any connected clients to the server"
-d,  debug            Start a debugging session if having trouble"
-l,  list             List all clients"
-qr, qrcode           Show the qrcode of a client for use with the mobile app"
-r,  remove           Remove a client"
-h,  help             Show this help dialog"
-u,  uninstall        Uninstall pivpn from your system!"
-up, update           Updates PiVPN Scripts"
-bk, backup           Backup VPN configs and user profiles"
```

# Resources
**PiVPN GitHub Repo:**
{% embed url="https://github.com/pivpn/pivpn" %}

**PiVPN Install Documentation:**
{% embed url="https://docs.pivpn.io/install/" %}

**PiVPN WireGuard Documentation:**
{% embed url="https://docs.pivpn.io/wireguard/" %}


