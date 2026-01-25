## Network Enumeration
### Nmap
```
./allPorts -> Initial scan, discovered ports 22 - 2222 - 8080 - 42633 - 44353
./targeted -> Targeted scan to ports:

# Nmap 7.98 scan initiated Fri Jan 23 11:59:34 2026 as: nmap -sCV -p22,2222,8080,426
     │ 33,44353 -oN targeted 10.129.234.57
   2 │ Nmap scan report for 10.129.234.57
   3 │ Host is up (0.13s latency).
   4 │ 
   5 │ PORT      STATE SERVICE    VERSION
   6 │ 22/tcp    open  ssh        OpenSSH 8.9p1 Ubuntu 3ubuntu0.13 (Ubuntu Linux; protocol 
     │ 2.0)
   7 │ | ssh-hostkey: 
   8 │ |   256 a9:36:3d:1d:43:62:bd:b3:88:5e:37:b1:fa:bb:87:64 (ECDSA)
   9 │ |_  256 da:3b:11:08:81:43:2f:4c:25:42:ae:9b:7f:8c:57:98 (ED25519)
  10 │ 2222/tcp  open  java-rmi   Java RMI
  11 │ |_ssh-hostkey: ERROR: Script execution failed (use -d to debug)
  12 │ | rmi-dumpregistry: 
  13 │ |   jmxrmi
  14 │ |     javax.management.remote.rmi.RMIServerImpl_Stub
  15 │ |     @127.0.1.1:44353
  16 │ |     extends
  17 │ |       java.rmi.server.RemoteStub
  18 │ |       extends
  19 │ |_        java.rmi.server.RemoteObject
  20 │ 8080/tcp  open  http       Apache Tomcat 10.1.19
  21 │ |_http-title: Apache Tomcat/10.1.19
  22 │ 42633/tcp open  tcpwrapped
  23 │ 44353/tcp open  java-rmi   Java RMI
  24 │ Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
  25 │ 
  26 │ Service detection performed. Please report any incorrect results at https://nmap.org
     │ /submit/ .
  27 │ # Nmap done at Fri Jan 23 12:00:19 2026 -- 1 IP address (1 host up) scanned in 45.76
     │  seconds
```

