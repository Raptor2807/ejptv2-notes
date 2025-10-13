- WAF = Web Application Firewall
### WAFW00F (two zeros not O's)

**The Web Application Firewall Fingerprinting Tool.**

## How does it work?

To do its magic, WAFW00F does the following:

- Sends a _normal_ HTTP request and analyses the response; this identifies a number of WAF solutions.
- If that is not successful, it sends a number of (potentially malicious) HTTP requests and uses simple logic to deduce which WAF it is.
- If that is also not successful, it analyses the responses previously returned and uses another simple algorithm to guess if a WAF or security solution is actively responding to our attacks.
- comes pre packaged with kali 

- identifying that a site is being protected by a WAF will help us to better strategize what our next steps should be.
```
wafw00f hackersploit.org

                   ______
                  /      \                                                                                                                                                                                                                 
                 (  Woof! )                                                                                                                                                                                                                
                  \  ____/                      )                                                                                                                                                                                          
                  ,,                           ) (_                                                                                                                                                                                        
             .-. -    _______                 ( |__|                                                                                                                                                                                       
            ()``; |==|_______)                .)|__|                                                                                                                                                                                       
            / ('        /|\                  (  |__|                                                                                                                                                                                       
        (  /  )        / | \                  . |__|                                                                                                                                                                                       
         \(_)_))      /  |  \                   |__|                                                                                                                                                                                       

                    ~ WAFW00F : v2.3.1 ~
    The Web Application Firewall Fingerprinting Toolkit                                                                                                                                                                                    
                                                                                                                                                                                                                                           
[*] Checking https://hackersploit.org
[+] The site https://hackersploit.org is behind Cloudflare (Cloudflare Inc.) WAF.
[~] Number of requests: 2

```

- if WAF is protecting the web server then the IP addr of the server will not be visible as in case of hackersploit.org 
- but for the case of zonetransfer.me the WAF wasn't detected. hence the ip addr we were getting from dnsdumpster tool is the real IP addr of the server
- we use argument -a for checking all instances for the WAF