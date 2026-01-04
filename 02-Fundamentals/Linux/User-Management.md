
# 👥 Linux User Management Cheat Sheet

**"In Linux, everything is a file, and every file is owned by a User."**

Whether you are setting up a lab or trying to escalate privileges on a hacked box, you need to understand users and groups.

---

## 🛠️ Creating & Deleting Users
*How to add people to the system.*

| Command | Description | Example |
| :--- | :--- | :--- |
| **`adduser`** | **The Friendly Way.** Interactive script that asks for password, full name, etc. Creates home directory automatically. | `sudo adduser newhacker` |
| **`useradd`** | **The "Script" Way.** Low-level. Does NOT create a password or home folder by default. (Painful for humans). | `sudo useradd -m -s /bin/bash newhacker` |
| **`deluser`** | Deletes a user (and optionally their files). | `sudo deluser --remove-home olduser` |
| **`passwd`** | Change a password. | `sudo passwd root` |

---

## 🕵️ Recon: Who am I?
*The first commands I run when I get a shell on a target.*

### `whoami`
Shows the current effective username.
```bash
whoami
# Output: www-data
```
# `id`

The most important command. Shows your User ID (uid), Group ID (gid), and group memberships.

```bash 
id
# Output: uid=1000(john) gid=1000(john) groups=1000(john),27(sudo),999(docker)
```

Hacker Tip: Look for special groups like `docker`, `lxd`, or `sudo`. These are paths to root!

# `w` or # `who`

Shows who else is logged into the system right now.
```bash
w
```
# `last`

Shows the history of who logged in recently.


## 🔧 Modifying Users & Groups

Changing permissions.

### Adding a User to Sudo

This gives a user administrative powers.

```bash 
sudo usermod -aG sudo username
# -a = Append (Don't overwrite existing groups!)
# -G = Groups
``` 
### Changing Shell

If a user is stuck in /bin/sh and wants /bin/bash:

```bash 
sudo usermod -s /bin/bash username
```
### Creating a Group
```bash 
sudo addgroup developers
```
### 🔐 The Sudoers File

The configuration file that controls who can run sudo.

Never edit /etc/sudoers directly with nano/vim. Always use:

```bash 
sudo visudo
```

Why? Because `visudo` checks for syntax errors before saving. If you break the sudoers file, you lock yourself out of root forever.

### Common PrivEsc Finding

If you see this in /etc/sudoers:

```bash 
john ALL=(ALL) NOPASSWD: /usr/bin/python3
```

Means: User `john` can run Python as ROOT without typing a password.

Exploit:   `sudo python3 -c 'import os; os.system("/bin/bash")'` -> Root Shell!

## 🧠 Sudo vs. Su

`su -`: "Switch User." Asks for the Target's password. (e.g., to become root, you need root's password).

`sudo`: "SuperUser Do." Asks for YOUR password. (You must be in the sudo group).

Tip: `sudo -i` gives you a full root shell environment.

