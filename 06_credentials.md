## Credentials

We could've gotten the credentials for the tomcat user using an automated scanning

I jumped straight to Java RMI CVE-2015-2342 

Using this exploit we managed to log in as tomcat user through a pseudo-shell

Miss-configured /home/useradmin/ directory had a folder named "backup"

inside we had a zip that i couldn't manage to unzip for some reason but i used gzip to decompress the ".gz" part and then i used "strings backup.tar".

I was able to use the private ssh key with OTP password key that i registered inside my phone.

Logged on successfully after inserting useradmin@manage public ssh key inside the same folder as the private key and using the OTP.
