- Sublist3r is a python tool designed to enumerate subdomains of websites using OSINT. It helps penetration testers and bug hunters collect and gather subdomains for the domain they are targeting. 

- Sublist3r enumerates subdomains using many search engines such as Google, Yahoo, Bing, Baidu and Ask. Sublist3r also enumerates subdomains using Netcraft, Virustotal, ThreatCrowd, DNSdumpster and ReverseDNS.

[subbrute](https://github.com/TheRook/subbrute) was integrated with Sublist3r to increase the possibility of finding more subdomains using bruteforce with an improved wordlist. The credit goes to TheRook who is the author of subbrute.

- if we do subdomain brute force then it becomes active IG
- doesn't come pre packaged in kali
- |Short Form|Long Form|Description|
|---|---|---|
|-d|--domain| Domain name to enumerate subdomains of|
|-b|--bruteforce| Enable the subbrute bruteforce module|
|-p|--ports |Scan the found subdomains against specific tcp ports|
|-v|--verbose| Enable the verbose mode and display results in realtime|
|-t|--threads| Number of threads to use for subbrute bruteforce|
|-e|--engines| Specify a comma-separated list of search engines|
|-o|--output| Save the results to text file|
|-h|--help| show the help message and exit|


#### Now one thing to note about this tool is that it will use your IP to make a lot of requests to google, and google has a rate limiting functionality that will limit the number of requests you can make 

Example 
```
raptor㉿RAPTOR)-[~]
└─$ sublist3r -d hackersploit.org                      

                 ____        _     _ _     _   _____                                                                                                                                                                                       
                / ___| _   _| |__ | (_)___| |_|___ / _ __                                                                                                                                                                                  
                \___ \| | | | '_ \| | / __| __| |_ \| '__|                                                                                                                                                                                 
                 ___) | |_| | |_) | | \__ \ |_ ___) | |                                                                                                                                                                                    
                |____/ \__,_|_.__/|_|_|___/\__|____/|_|                                                                                                                                                                                    
                                                                                                                                                                                                                                           
                # Coded By Ahmed Aboul-Ela - @aboul3la                                                                                                                                                                                     
                                                                                                                                                                                                                                           
[-] Enumerating subdomains now for hackersploit.org                                                                                                                                                                                        
[-] Searching now in Baidu..
[-] Searching now in Yahoo..
[-] Searching now in Google..
[-] Searching now in Bing..
[-] Searching now in Ask..
[-] Searching now in Netcraft..
[-] Searching now in DNSdumpster..
[-] Searching now in Virustotal..
[-] Searching now in ThreatCrowd..
[-] Searching now in SSL Certificates..
[-] Searching now in PassiveDNS..
Process DNSdumpster-8:
Traceback (most recent call last):
  File "/usr/lib/python3.13/multiprocessing/process.py", line 313, in _bootstrap
    self.run()
    ~~~~~~~~^^
  File "/usr/lib/python3/dist-packages/sublist3r.py", line 269, in run
    domain_list = self.enumerate()
  File "/usr/lib/python3/dist-packages/sublist3r.py", line 649, in enumerate
    token = self.get_csrftoken(resp)
  File "/usr/lib/python3/dist-packages/sublist3r.py", line 644, in get_csrftoken
    token = csrf_regex.findall(resp)[0]
            ~~~~~~~~~~~~~~~~~~~~~~~~^^^
IndexError: list index out of range
[!] Error: Virustotal probably now is blocking our requests
[!] Error: Google probably now is blocking our requests
[~] Finished now the Google Enumeration ...
[-] Total Unique Subdomains Found: 14
www.hackersploit.org
cloud.hackersploit.org
community.hackersploit.org
apps.community.hackersploit.org
preview.community.hackersploit.org
studio.community.hackersploit.org
demo.hackersploit.org
forum.hackersploit.org
new.hackersploit.org
prm.hackersploit.org
studio.hackersploit.org
test.hackersploit.org
videos.hackersploit.org
www.videos.hackersploit.org

```
