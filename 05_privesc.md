## Privilege Escalation
useradmin@manage:~$ sudo -l
Matching Defaults entries for useradmin on manage:
    env_reset, timestamp_timeout=1440, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin,
    use_pty

User useradmin may run the following commands on manage:
    (ALL : ALL) NOPASSWD: /usr/sbin/adduser ^[a-zA-Z0-9]+$

Once logged as useradmin, proceed to create user "admin". 
Gets automatically inserted into "sudoers"

However, if you try to create another user even with "sudo /usr/sbin/adduser" that is not "admin" it won't be inserted into "sudoers" *(struggled a little bit with this one)*


Once logged in with the user that we create, *(admin)* we could easily just do "sudo -s" to move to user "root"
```
admin@manage:/home/useradmin$ sudo -s
root@manage:/home/useradmin# ```
