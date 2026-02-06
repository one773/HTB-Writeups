## Summary
Exciting Easy Machine in my opinion.


We were able to find a hidden subdomain looking inisde the Vue.js code exposed in plain text.

Once inside this subdomain, we find out it's running a Strapi CMS exposed to 2 CVEs that give us the ability to sign in as Admin and run RCE
CVE's mentioned for Strapi RCE : CVE-2019-18818, CVE-2019-19609

This gives us the ability to connect to the victim's machine signed as user "Strapi"

Then while we are enumerating services running on localhost ports, we find a laravel framework running that is vulnerable to CVE-2021-3129 and give us root access.


