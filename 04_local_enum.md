## Local Enumeration
The first thing i want to see is a user i can target to which i want to move.

$ cat /etc/passwd | grep /bin/bash
root:x:0:0:root:/root:/bin/bash
developer:x:1000:1000:hackthebox:/home/developer:/bin/bash


only 2 users except us can have a shell.


Now that we have this info we look for interesting vectors but can't find anything besides some open ports with services running on them

```
$ netstat -tnlp
(Not all processes could be identified, non-owned process info
 will not be shown, you would have to be root to see it all.)
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 127.0.0.1:8000          0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.1:3306          0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.1:1337          0.0.0.0:*               LISTEN      1954/node /usr/bin/ 
tcp6       0      0 :::80                   :::*                    LISTEN      -                   
tcp6       0      0 :::22                   :::*                    LISTEN      - 

```

port 3306 is MySQL
But what about 1337 and 8000?

Next thing i want to do is tunnel the first port we see which is 8000 to my attacker machine.

So we create a SSH key to tunnel with SSH.
```
$ ls
authorized_keys  id_rsa  id_rsa.pub  strapi  strapi.pub
$ ls -la
total 28
drwxrwxr-x  2 strapi strapi 4096 Feb  5 08:37 .
drwxr-xr-x 10 strapi strapi 4096 Feb  5 08:21 ..
-rw-rw-r--  1 strapi strapi  102 Feb  5 08:37 authorized_keys
-rw-------  1 strapi strapi 1675 Feb  5 08:21 id_rsa
-rw-r--r--  1 strapi strapi  400 Feb  5 08:21 id_rsa.pub
-rw-------  1 strapi strapi 1675 Feb  5 08:27 strapi
-rw-r--r--  1 strapi strapi  400 Feb  5 08:27 strapi.pub
```
im going to use "my public key" and insert it inside authorized_keys so i can tunnel the port 8000 to my machine

On my machine i run this command:

```

ssh -i $HOME/.ssh/one773.git strapi@10.129.71.135 -L 8000:localhost:8000

```
Once on we forward the victim's service running on port 8000 to our attacker's machine, we should be able to access it by opening our browser and going to http://localhost:8000/

After it loads we should see a Laravel website.

We can use feroxbuster again on this new website and look for potential vectors.

```
❯ feroxbuster -u http://localhost:8000/
                                                                                           
 ___  ___  __   __     __      __         __   ___
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 2.13.1
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://localhost:8000/
 🚩  In-Scope Url          │ localhost
 🚀  Threads               │ 50
 📖  Wordlist              │ /usr/share/seclists/Discovery/Web-Content/raft-medium-directories.txt
 👌  Status Codes          │ All Status Codes!
 💥  Timeout (secs)        │ 7
 🦡  User-Agent            │ feroxbuster/2.13.1
 🔎  Extract Links         │ true
 🏁  HTTP methods          │ [GET]
 🔃  Recursion Depth       │ 4
───────────────────────────┴──────────────────────
 🏁  Press [ENTER] to use the Scan Management Menu™
──────────────────────────────────────────────────
404      GET       36l      123w     6609c Auto-filtering found 404-like response and created new filter; toggle off with --dont-filter
200      GET      119l      979w    17473c http://localhost:8000/
500      GET      247l    18586w   616205c http://localhost:8000/profiles
[>-------------------] - 5s       665/30018   5m      found:2       errors:0      
🚨 Caught ctrl+c 🚨 saving scan state to ferox-http_localhost_8000_-1770366030.state ...
[>-------------------] - 5s       668/30018   5m      found:2       errors:0      
[>-------------------] - 5s       643/30000   123/s   http://localhost:8000/  

```

The only thing that is useful is "/profiles" and it takes us to this "debug" type of website.

We can see it runs on the "/home/developer/myproject" path inside victim's machine.


As I search for potential exploits for this framework, i found this laravel exploit PoC.

https://github.com/ambionics/laravel-exploits

I clone it to my attacker machine.

We proceed to 05_privesc.md
