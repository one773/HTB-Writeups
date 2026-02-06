## Web Enumeration
### Entry Points
```- 
❯ feroxbuster -u http://horizontall.htb/
                                                                                           
 ___  ___  __   __     __      __         __   ___
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 2.13.1
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://horizontall.htb/
 🚩  In-Scope Url          │ horizontall.htb
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
404      GET        7l       13w      178c Auto-filtering found 404-like response and created new filter; toggle off with --dont-filter
301      GET        7l       13w      194c http://horizontall.htb/css => http://horizontall.htb/css/
301      GET        7l       13w      194c http://horizontall.htb/js => http://horizontall.htb/js/
301      GET        7l       13w      194c http://horizontall.htb/img => http://horizontall.htb/img/
200      GET        1l       35w     6796c http://horizontall.htb/favicon.ico
200      GET        1l        5w      720c http://horizontall.htb/css/app.0f40a091.css
200      GET        2l      394w    18900c http://horizontall.htb/js/app.c68eb462.js
200      GET       10l     2803w   218981c http://horizontall.htb/css/chunk-vendors.55204a1e.css
200      GET       55l    86826w  1190830c http://horizontall.htb/js/chunk-vendors.0e02b89e.js
200      GET        1l       43w      901c http://horizontall.htb/
[####################] - 37s   120007/120007  0s      found:9       errors:0      
[####################] - 36s    30000/30000   830/s   http://horizontall.htb/ 
[####################] - 35s    30000/30000   850/s   http://horizontall.htb/js/ 
[####################] - 35s    30000/30000   846/s   http://horizontall.htb/css/ 
[####################] - 35s    30000/30000   851/s   http://horizontall.htb/img/
```

## Website Research

Once we are inside the website, it doesn't show anything in particular, either the scan or the website itself.

I decide to check for the stack that this website is running in case there's any info or outdated app that the website is using.

We find Vue javascript framework as well as the code that is running.

Next step was to check the code in case anything interesting appears.
Next thing you know, there's a hidden subdomain "http://api-prod.horizontall.htb/" So we check it out
```js
<template>
  <div id="app">
    <navbar/>
    <home/>
    <footer class="gradient">
      <div class="container-fluid text-center">
        <span>Made by
          <a href="https://horizontall.htb">Horizontall.htb</a></span
        >
      </div>
    </footer>
  </div>
</template>

<script>
import axios from 'axios'
import Navbar from './components/Navbar.vue'
import Home from './components/Home.vue'
export default {
  name: 'App',
  components: {
    Navbar,
    Home
  },
  data(){
    return {
      reviews:[],
    }
  },
  methods:{
    getReviews(){
      axios.get('http://api-prod.horizontall.htb/reviews')
      .then(response => this.reviews = response.data)
    }
  }
}
</script>

<style>
html,
body {
  height: 100%;
  font-size: 1rem;
  font-family: "Montserrat", sans-serif;
}
</style>


```

Next we run feroxbuster on this new domain.

```

❯ feroxbuster -u http://api-prod.horizontall.htb/
                                                                                           
 ___  ___  __   __     __      __         __   ___
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 2.13.1
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://api-prod.horizontall.htb/
 🚩  In-Scope Url          │ api-prod.horizontall.htb
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
404      GET        1l        3w       60c Auto-filtering found 404-like response and created new filter; toggle off with --dont-filter
200      GET       19l       33w      413c http://api-prod.horizontall.htb/
200      GET      223l     1051w     9230c http://api-prod.horizontall.htb/admin/runtime~main.d078dc17.js
200      GET       16l      101w      854c http://api-prod.horizontall.htb/Admin
200      GET       16l      101w      854c Auto-filtering found 404-like response and created new filter; toggle off with --dont-filter
403      GET        1l        1w       60c http://api-prod.horizontall.htb/users
403      GET        1l        1w       60c http://api-prod.horizontall.htb/admin/plugins
[>-------------------] - 2s       573/60005   11m     found:5       errors:0      
🚨 Caught ctrl+c 🚨 saving scan state to ferox-http_api-prod_horizontall_htb_-1770296479.state ...
[>-------------------] - 2s       591/60005   11m     found:5       errors:0      
[>-------------------] - 2s       393/30000   232/s   http://api-prod.horizontall.htb/ 
[>-------------------] - 1s       176/30000   314/s   http://api-prod.horizontall.htb/admin/  


```


We find the version of the Strapi CMS. Now we get into the fun part.

```
curl http://api-prod.horizontall.htb/admin/strapiVersion
{"strapiVersion":"3.0.0-beta.17.4"}% 

```
