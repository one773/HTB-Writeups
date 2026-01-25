## Summary
Running through initial scans indicate a strong attack vector the 2222 port that is hosting a java-rmi service.

After further investigation I discovered that it is vulnerable to CVE 2015-2342 


Once we run this exploit, we gain access as "tomcat" user.


When we are the "tomcat" user we are looking for vectors that point to lateral movement to other users such as "Karl" or "useradmin"

Karl user is basically useless. Nothing interesting not even ssh keys/passwords.

On the other hand, "useradmin" has backup folder inside it's home directory.

We mount a python3 http server through the port 1773 *(my personal favorite)* and download this file onto my attacker machine.

Once downloaded, we decompress the file and we see some interesting files.
```
❯ tree -la
├── .google_authenticator
├── .profile
├── .ssh
│   ├── authorized_keys
│   ├── id_ed25519
│   └── id_ed25519.pub

```

This allowed me to see the private and public ssh keys for the user "useradmin"

Now with we log on to the victim's machine using the ssh key we found inside backup folder.

Once i gained access to the victim's machine with the user "useradmin"
We run "sudo -l"
```
useradmin@manage:~$ sudo -l
Matching Defaults entries for useradmin on manage:
    env_reset, timestamp_timeout=1440, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin,
    use_pty

User useradmin may run the following commands on manage:
    (ALL : ALL) NOPASSWD: /usr/sbin/adduser ^[a-zA-Z0-9]+$
```
This is the output that we get.

With the "sudo /usr/sbin/adduser admin " command we create a privileged user with the name "admin" *(for some reason, if you try and create another user, for example, 'one773' the machine will not add that user to sudoers. It has to be specifically the user 'admin', haven't tried any other combinations of admin.)*

When the user "admin" is created, we simply run "su admin", type the password we set.

Once authenticated as "admin", i ran "sudo -s" 
```
admin@manage:/home/useradmin$ sudo -s
root@manage:/home/useradmin# 
```

And that's how i gained root access on Manager machine
