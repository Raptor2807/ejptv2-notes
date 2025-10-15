- very powerful tool for red team
- it is OSNIT tool
- the power of harvester is really dependent on the public data sources that you essentially ask the harvester to query
- it is designed for early stage information gathering during a pen tester red team engagement
- theHarvester is a simple to use, yet powerful tool designed to be used during the reconnaissance stage of a red team assessment or penetration test. It performs open source intelligence (OSINT) gathering to help determine a domain's external threat landscape. The tool gathers names, emails, IPs, subdomains, and URLs by using multiple public resources
```
- theHarvester -d zonetransfer.me -b duckduckgo,baidu,bing,yahoo,urlscan,crtsh,intelx,otx,rapiddns,whoisxml,tomba
Read proxies.yaml from /etc/theHarvester/proxies.yaml
*******************************************************************
*  _   _                                            _             *
* | |_| |__   ___    /\  /\__ _ _ ____   _____  ___| |_ ___ _ __  *
* | __|  _ \ / _ \  / /_/ / _` | '__\ \ / / _ \/ __| __/ _ \ '__| *
* | |_| | | |  __/ / __  / (_| | |   \ V /  __/\__ \ ||  __/ |    *
*  \__|_| |_|\___| \/ /_/ \__,_|_|    \_/ \___||___/\__\___|_|    *
*                                                                 *
* theHarvester 4.8.0                                              *
* Coded by Christian Martorella                                   *
* Edge-Security Research                                          *
* cmartorella@edge-security.com                                   *
*                                                                 *
*******************************************************************

[*] Target: zonetransfer.me 

Read api-keys.yaml from /etc/theHarvester/api-keys.yaml
Read api-keys.yaml from /etc/theHarvester/api-keys.yaml

[!] Missing API key for Intelx. 
Read api-keys.yaml from /etc/theHarvester/api-keys.yaml
Read api-keys.yaml from /etc/theHarvester/api-keys.yaml

[!] Missing API key for Tomba Key and/or Secret. 
Read api-keys.yaml from /etc/theHarvester/api-keys.yaml

[!] Missing API key for whoisxml. 
An exception has occurred: Server disconnected
An exception has occurred: Server disconnected
An exception has occurred: Server disconnected
An exception has occurred: Server disconnected
An exception has occurred: Server disconnected
An exception has occurred: Server disconnected
An exception has occurred: Server disconnected
An exception has occurred: Server disconnected
An exception has occurred: Server disconnected
An exception has occurred: Server disconnected
An exception has occurred: Server disconnected
An exception has occurred: Server disconnected
An exception has occurred: Server disconnected
An exception has occurred: Server disconnected
An exception has occurred: Server disconnected
An exception has occurred: Server disconnected
An exception has occurred: Server disconnected
An exception has occurred: Server disconnected
An exception has occurred: Server disconnected
An exception has occurred: Server disconnected
An exception has occurred: Server disconnected
An exception has occurred: Server disconnected
An exception has occurred: Server disconnected
An exception has occurred: Server disconnected
An exception has occurred: Server disconnected
An exception has occurred: Server disconnected
An exception has occurred: Server disconnected
An exception has occurred: Server disconnected
An exception has occurred: Server disconnected
An exception has occurred: Server disconnected
An exception has occurred: Server disconnected
An exception has occurred: Server disconnected
An exception has occurred: Server disconnected
An exception has occurred: Server disconnected
An exception has occurred: Server disconnected
An exception has occurred: Server disconnected
An exception has occurred: Server disconnected
An exception has occurred: Server disconnected
An exception has occurred: Server disconnected
An exception has occurred: Server disconnected
An exception has occurred: Server disconnected
An exception has occurred: Server disconnected
An exception has occurred: Server disconnected
An exception has occurred: Server disconnected
An exception has occurred: Server disconnected
        Searching 0 results.
[*] Searching Bing. 
[*] Searching CRTsh.                                                                                                                                                                                                                       
[*] Searching Baidu.                                                                                                                                                                                                                       
[*] Searching Duckduckgo.                                                                                                                                                                                                                  
[*] Searching Rapiddns.                                                                                                                                                                                                                    
[*] Searching Urlscan.                                                                                                                                                                                                                     
[*] Searching Otx.                                                                                                                                                                                                                         
[*] Searching Yahoo.                                                                                                                                                                                                                       
                                                                                                                                                                                                                                           
[*] ASNS found: 2                                                                                                                                                                                                                          
--------------------                                                                                                                                                                                                                       
AS16276                                                                                                                                                                                                                                    
AS16509                                                                                                                                                                                                                                    
                                                                                                                                                                                                                                           
[*] Interesting Urls found: 14                                                                                                                                                                                                             
--------------------                                                                                                                                                                                                                       
http://alltcpportsopen.firewall.test.zonetransfer.me/                                                                                                                                                                                      
http://asfdbbox.zonetransfer.me/                                                                                                                                                                                                           
http://canberra-office.zonetransfer.me/                                                                                                                                                                                                    
http://dc-office.zonetransfer.me/                                                                                                                                                                                                          
http://deadbeef.zonetransfer.me/                                                                                                                                                                                                           
http://email.zonetransfer.me/                                                                                                                                                                                                              
http://home.zonetransfer.me/                                                                                                                                                                                                               
http://intns1.zonetransfer.me/                                                                                                                                                                                                             
http://intns2.zonetransfer.me/                                                                                                                                                                                                             
http://ipv6actnow.org.zonetransfer.me/                                                                                                                                                                                                     
http://office.zonetransfer.me/                                                                                                                                                                                                             
http://owa.zonetransfer.me/                                                                                                                                                                                                                
http://staging.zonetransfer.me/                                                                                                                                                                                                            
http://vpn.zonetransfer.me/                                                                                                                                                                                                                
                                                                                                                                                                                                                                           
[*] IPs found: 18                                                                                                                                                                                                                          
-------------------                                                                                                                                                                                                                        
143.228.181.132                                                                                                                                                                                                                            
167.88.42.94                                                                                                                                                                                                                               
207.46.197.32                                                                                                                                                                                                                              
217.147.177.157                                                                                                                                                                                                                            
2600:9000:206f:4200:7:60:4d00:93a1                                                                                                                                                                                                         
2600:9000:206f:ba00:7:60:4d00:93a1                                                                                                                                                                                                         
2600:9000:206f:c200:7:60:4d00:93a1                                                                                                                                                                                                         
2600:9000:266e:1a00:7:60:4d00:93a1                                                                                                                                                                                                         
2600:9000:277c:aa00:7:60:4d00:93a1                                                                                                                                                                                                         
5.196.105.14                                                                                                                                                                                                                               
52.91.28.78                                                                                                                                                                                                                                
54.192.51.114                                                                                                                                                                                                                              
54.192.51.120                                                                                                                                                                                                                              
99.84.79.100                                                                                                                                                                                                                               
99.84.79.36                                                                                                                                                                                                                                
99.84.79.5                                                                                                                                                                                                                                 
99.84.79.52                                                                                                                                                                                                                                
                                                                                                                                                                                                                                           
[*] Emails found: 2                                                                                                                                                                                                                        
----------------------                                                                                                                                                                                                                     
customer-service@zonetransfer.me                                                                                                                                                                                                           
pippa@zonetransfer.me                                                                                                                                                                                                                      
                                                                                                                                                                                                                                           
[*] No people found.                                                                                                                                                                                                                       
                                                                                                                                                                                                                                           
[*] Hosts found: 21                                                                                                                                                                                                                        
---------------------                                                                                                                                                                                                                      
alltcpportsopen.firewall.test.zonetransfer.me                                                                                                                                                                                              
asfdbbox.zonetransfer.me                                                                                                                                                                                                                   
aspmx.l.google.com.zonetransfer.me                                                                                                                                                                                                         
canberra-office.zonetransfer.me                                                                                                                                                                                                            
dc-office.zonetransfer.me                                                                                                                                                                                                                  
deadbeef.zonetransfer.me                                                                                                                                                                                                                   
email.zonetransfer.me                                                                                                                                                                                                                      
home.zonetransfer.me                                                                                                                                                                                                                       
intns1.zonetransfer.me                                                                                                                                                                                                                     
intns2.zonetransfer.me                                                                                                                                                                                                                     
ipv6actnow.org.zonetransfer.me                                                                                                                                                                                                             
office.zonetransfer.me                                                                                                                                                                                                                     
owa.zonetransfer.me                                                                                                                                                                                                                        
robin.zonetransfer.me.zonetransfer.me                                                                                                                                                                                                      
robinwood.zonetransfer.me                                                                                                                                                                                                                  
rp.zonetransfer.me                                                                                                                                                                                                                         
sip.zonetransfer.me                                                                                                                                                                                                                        
staging.zonetransfer.me                                                                                                                                                                                                                    
staging.zonetransfer.me:d3gdbrxsb9xhmf.cloudfront.net                                                                                                                                                                                      
tcp.zonetransfer.me                                                                                                                                                                                                                        
vpn.zonetransfer.me                  
```
