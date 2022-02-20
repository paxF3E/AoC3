# AoC-3-2021

## Day 1 Web Exploitation :: IDOR
Insecure Direct Object Reference, IDOR, server returns query results without authenticating user identity. 
- Usually abused using query info appended to the URL to send request to the server. 
`https://domain/page?query[profile?id=31]`
- Examining form details sometimes reveal vulnerable details. `updated HTML of a form after receiving info from user`
- change in cookie characters can result in leading to someone else's profile

## Day 2 Web Exploitation :: Cookie Manipulation
HTTP is a stateless protocol. At every request, server cant distinguish btw user1 and user2. Cookie acts as a proof of identity of both to establish a stateful session.<br>
- Schematics of a HTTP request
  - POST query
  - Server response. returns a `200 OK` if successful.
  - cookie set
  - GET query
  - Server response.
  - authentication with cookie 
  - if successful, locating and de-serializing of session.
  - if successful, `200 OK` reply
<br>
<b>De-serializing</b> is fetching data from say JSON format and changing its type to Object to prep it to be abused.
<br>

- Cookie components are key-value pairs. tbf, `name-value` others are `attribute-value`. <br>
- `Set-Cookie` example ```Set-Cookie: <cookie-name>=<cookie-value>; Domain=<domain-value>; Secure; HttpOnly```
#### Cookie Manipulation Schematics
1. fetch a cookie
2. decode its value
3. identify the scheme and abuse
4. re-encode and replace with og value
5. refresh...voila!

## Day 3 Web Exploitation :: Content Discovery
Content == assets
1. Config files
2. pwds and secrets
3. backups
4. CMS
5. admin dashboards

#### Dirbuster
automates guessing work with provided wordlists and seclists<br>
`dirb <URL> <path_of_wordlist>`

## Day 4 Web Exploitation :: Fuzzing, Authentication
authenticating is verifying user's identity by either
  1. creds- usrname and pwd
  2. tokens
  3. biometric - finger print/iris/retina
required when there is a need to who is accessing the data to create accountablity
<br>
Authorisation is a term for the rules defining what an `authenticated` user can and cannot access.
<br>

#### Fuzzing
- automated testing of a webapp's element, untill app leaves out a vulnerability
- info input is provided at a faster rate using seclists and wordlists
<br>

FYI:
- Wordlists sorted by probability originally created for password generation and testing - make sure your passwords aren't popular!
- SecLists is the security tester's companion. It's a collection of multiple types of lists used during security assessments, collected in one place. List types include usernames, passwords, URLs, sensitive data patterns, fuzzing payloads, web shells, and many more.
<br>

#### Burp Suite
“Burp” is a proxy-based tool used to evaluate the security of web-based applications and do hands-on testing. `king of web pentesting`<br>
Schematics of Fuzzing using BurpSuite
- visit the form either in external brower (with proxy setup) or Burp's embedded browser
- turn intercept on in `proxy` tab -> `intercept`
- submit dummy data in the form
- send the request to `intruder`
- switch to `intruder` tab then `positions`
- select the attribute to be fuzzed and add marker `§`
- turn to `payloads`, add a list, select attack type
- start attack, analyse results. done...
<br>

FYI:
- Sniper attack
  - single payload
  - targets payload positions in turns, places each payload in that position in turn
  - params with common vulnerabilities
  - #requests = #positions * #payloads in payload set
- Battering ram
  - single set of payload
  - iterates over payloads, put same payload into all positions at once
  - requests with same input in multiple places
  - #requests = #payloads in payload set
- Pitchfork
  - multiple payloads, one for each position
  - iterates over payloads, put related payloads from set 1 & 2 to positions 1 & 2
  - requests with positions having a relation (mapped like `[usrname,some ID]`)
  - #reqeusts = #payloads in smallest payload set
- Cluster bomb
  - multiple payloads
  - iterates and turns in every permutation of payload combinations
  - requests with unrelated unknown params (like `[usrname,pwd]`)
  - #requests = Π(#payloads in each payload set)

## Day 5 Web Exploitation :: XSS
- cross-site scripting or XSS, is an injection attack, where a malicious JS script is injected to be executed with the source of the webapp. Once executed, numerous things can be targetted to exploit the victim.
- <b>Injection Attack</b> is the exploitation of a computer bug that is caused by processing invalid data. The injection is used by an attacker to introduce code into a vulnerable computer program and change the course of execution.
#### XSS Vulnerabilities
1. DOM, document object model, is an interface for HTML
  - JS script is executed in the browser, without loading any new page
  - executed when source JS is acted by user interaction
  - example `JS code fetching some contents from any param, and writing on the page section being viewed. contents of the hash arnt checked, thus malicous code can be put anywhere in the source JS`
2. Reflected
  - when user's data is put in the src
  - example `https://website.thm/login?error=Username%20Is%20Incorrect` script is executed when someone visits the page
3. Stored
  - when XSS script is stored on the webapp (say, db) 
  - gets run on every visit on the page
  - numerous victims, triggering the script without checking the XSS payload
4. Blind
  - similar to stored XSS
  - cant test against yourself. put the payload. gets executed when viewed.

## Day 6 Web Exploitation :: LFI
- web app vulnerablity allowing an attack to exploit the files that are included on the server. could be including cryptographic keys, secured dbs, private data.
- lack of input validation, unsanitized user inputs, grants them control over the vulnerability
- Risks :
  - exposed critical data
  - LFI triggered RCE
    - RCE, Remote Code Execution or ACE, `Arbitary Code Execution` is an attacker's ability to run any commands or code of the attacker's choice on a target machine or in a target process
    - program designed to exploit such a vulnerability is called an `arbitrary code execution exploit`
    - `exploit` is a software, data, or commands that take advantage of a bug/vulnerability to cause unanticipated behaviour like 
      1. gaining control of the system
      2. allowing priveledge escalation
        - <b>Priveledge Escalation</b> is the act of exploiting a bug/design flaw/config oversight in an OS or app to gain elevated access to resources, protected from an application or user resulting in an application with more privileges than intended by the application developer or system administrator which can perform unauthorized actions
      3. denial of service (DoS or DDoS attack)
        - **DoS** is making a machine/network resource unavailable to the user by disrupting the services of the host
        - accomplished by flooding the targeted machine/network with request and overload to prevent some/all legitimate requests from being fulfilled
        - **DDoS** the incoming traffic flooding the victim is from various sources, making it impossible to prevent the attack by blocking a single source...wow
<br>

- possible entry points, `GET` or `POST` params -> test for vulnerabilities
  - include OS files -> record reaction of the app
  - techniques
    1. direct file inclusion
    2. use of `..` to move one level up
    3. bypassing filters using `....//`
    4. URL encoding (like double encoding)
- **HTTP Headers** are list of strings sent and received by both client and server on every HTTP request.
  - invisible to end-user; processed and logged by server and client apps
  - defines the encoding type; session verification and identification; how to handle data on server side, age of doc being accessed
  - earlier format:
    - `:` separated key-value pairs
    - terminated by `CF` (carriage return, a control character or mechanism used to reset a device's position to the beginning of a line of text) and `LF` (line feed, EOL or SOL)
   - modern format : binary protocol
  
- PHP Wrappers
  - additional code guiding the stream how to handle protocols/encodings
  - examples ![image](https://user-images.githubusercontent.com/75432450/146645982-d7848f5d-301c-4a5e-a78a-d3f5fe03437c.png)

<br>

- **PHP filter** wrapper allows to read actual PHP files content encoded in formats cz they are executed directly and are never show the existing code `?file=php://filter/convert.base64-encode/resource=$PATH <\n> ?file=php://filter/read=string.rot13/resource=$PATH`
- **PHP data** wrapper includes the raw plain text or base64 encoded data `?file=data://text/plain;base64,ENCODED_TEXT`
#### RCE via LFE
- Log Poisoning
  - technique to gain RCE on webserver
  - malicious payload is included in the service log files
  - LFI is used to request the page including malicious payload
  - depends on
    1. design of webapp and server configs
    2. requires enumerations and analysis of schematics of the target webapp

- `User-Agent` is an HTTP header containing user's browser info to allow servers to identify type of OS, vendor, and version that can be controlled by the user.
  - In order to gain RCE, include PHP code in User-Agent; send request to log file to execute in the browser `curl -A $CODE_TEXT $URL`
  - include the code into webapp log `curl -A "<?php phpinfo();?>" $URL`
  - include the PHP code into the User-Agent and execute it using LFI vulnerability

#### RCE via PHP Sessions
- use session files instead of log files
  - enumeration to read the PHP config files
  - include PHP code in the session
  - call the file via LFI
- PHP uses a naming scheme `sess_<SESSION_ID>`; `SESSION_ID` can be retrieved from cookies/browser `https://$URL/$PAGE.php?err=$PATH/sess_<SESSION_ID>`

## Day 7 Web Exploitation :: NoSQL Injection
#### NoSQL 
- non-relational-database or non-SQL or not-only-SQL
- data storing/retrieving systems
- used for big Data and IoT devices
- fast queries, ease of use, scalability, flexible DS
#### MongoDB
- **Collection** <=> tables
- **Documents** <=> rows
- **Fields** <=> columns
#### SQL Injection
- db control with the attacker
- by sending queries over untrusted and unfiltered webapp input
- unauthorised leakage of info
- modification, privilage escalation, DoS attack
  - `> db.COLLECTION.findOne({username: $USERNAME, password: {"$ne":"xyz"}})` ; changed logic tricks the webapp to let us through
- **Exploiting SQL Injection**
  - find an entry point for unsanitized user input
  - how requests are passed by webapp, JSON or GET/POST
  - GET/POST
    - inject an array to match the JSON objection to match Key:Value pair
    - `http://URL/search?username=admin&role[$ne]=user`
  - JSON 
    - manipulate the JSON file by inserting MongoDB operators
<br>

- Inshort, trick the webapp logic

## Day 8 Special :: Powershell Transcriptions Log
- captures the input and output of powershell commands
- Windows Registry
  - large database of OS settings and config.
  - organized by hives
    - logical group of keys, subkeys, and values in the registry that has a set of supporting files loaded into memory when the operating system is started or a user logs in

## Day 9 Networking :: Packet Analysis
- Packet analysis is a technique used to capture and intercept network traffic that passes the computer’s network interfaces
  - network troubleshooting and communication protocol analysis
  - `Wireshark` captures network packets in real-time and displays them in a human-readable format

- The TCP/IP Stack, or the internet protocol suite, is a set of communication protocols used by the Internet or similar networks
  - https://www.linkedin.com/pulse/what-tcpip-stack-phillip-zito/
- OSI stands for Open System Interconnection is a reference model that describes how information from a software application in one computer moves through a physical medium to the software application in another computer
- a handshake is a signal between two devices or programs, used to, e.g., authenticate, coordinate; for example the handshaking between a hypervisor and an application in a guest virtual machine

- Berkeley Packet Filter (BPF) syntax is used in packet analyzers to filter specific packets pre-capture
  - Filtering packets is beneficial when locating information within a packet capture process
- DNS is like a giant phone book that takes a URL and turns it into an IP address

## Day 10 Networking :: Security Assessment
### IP Addresses
  - Every computer (host) that connects to a network needs to have a logical address
  - this address is logical because it's assigned by software and could change over time
  - Internet Protocol version 4 (IPv4)
    - made up of 4 decimal numbers (0-255)
      - `192.168.0.10`; `172.16.0.100`; `10.10.11.12`; `1.1.1.1`; first 3 are private(can be accessed from private networks only) and last one is a public IP
      - `127.0.0.1` is often referred to as the 'loopback address' or 'localhost', by default, any packet or traffic destined to this address won't leave the host
### Web Server
  - program that listens for incoming connections, usually from web browsers, and responds to their requests
  - server usually refers to a computer system that provides services to other clients, i.e. other computers, over a network. Ex: serving webpages, delivering email, facilitating video conferencing
  - TCP/IP protocols:
    - Hypertext Transfer Protocol (HTTP) for serving webpages
    - Domain Name System (DNS) for resolving hostnames to IP addresses
    - Post Office Protocol version 3 (POP3) for delivering email
    - Simple Mail Transfer Protocol (SMTP) for sending email
    - Telnet for remote login
    - Secure Shell (SSH) for secure remote login
### Ports
  - On a host, multiple processess can access the network simultaneously. To deliver packets correctly, port numbers are required to recognize the process
    ![image](https://user-images.githubusercontent.com/75432450/152498401-87b5be8a-fc67-45c9-b3bc-ed8849a01e07.png)
  - TCP and UDP protocols live on top of the IP protocol and connect processes running on different hosts
  - TCP requires a three-way handshake for a connection to be established, while UDP does not
### Nmap
  - Nmap is a network scanning tool that uses IP packets to identify all the devices connected to a network and to provide information on the services and operating systems they are running
  - `nmap -sT/sS MACHINE_IP`checks 1000 most common TCP ports
    - TCP Connect Scan: completes the three-way handshake in order to establish a connection with each port scanned (knocks the door and answers on answered)
    - TCP SYN Scan: does not complete a TCP three-way handshake (knocks the door and pretends not to when answered)

## Day 11 Networking :: MS SQL ans `sqsh`
  -  basic `sqsh` use and accessing the active MS SQL server
  -  `xp_cmdshell` commands to access the windows and look for the flag in dir

## Day 12 Networking :: Server scans
  - `nmap` to scan the server
  - Network File System (NFS) is a protocol that allows the ability to transfer files between different computers
  - `mount` command

## Day 13 Networking :: Priviledges
  - Account Types in Windows Server
    - Domain Administrator : highest account level
      - `domain` is a central registry used to manage all users
    - Services : accounts used by software to perform tasks such as back-ups or antivirus scans
    - Domain users : accounts with enuf priviledges to do daily jobs
    - Local accounts : accounts valid on the local system and not over the domain
    - -> Account management is done using groups

  ### Priviledge Escalation Pointers
  - Stored Creds - imp creds could be saved in a config file
  - Windows Kernel Exploit - vulnerability exploit
  - Exploitable file/dir perms - dir with critical files, can be accessed
  - Insecure Service perms - access and manipulate services on the system
  - DLL Hijacking - DLL are the sub apps required by parent app, which when manipulated, could help exploit using the priviledged application running a malicious DLL
  - Unquoted Service Path - service exec path not enclosed in quotes `" "`, could allow to run malicious execs
  - Always Install Elevated - installing a malicious app as administrator (deafult set as, always as admin) could grant high priviledges to unintended apps
  - Other Softwares - other softwares, services, scripts could provide additional priviledge escalation vectors
 - A mixture of diff vectors could result in hardship to identify and revert

  ### Info Collection
  - `net users` lists users on the system
  - `systeminfo | findstr /B /C: "OS Name"/C: "OS Version"` returns info about the system
  - `wmic service list` returns running/installed services on the system

  ### Exploitation
  - setup the disguise/frontline app 
  - fix the malicious app/script exploiting the perms of the carrying app
  - setup a listener
  - run the service

## Day 14 Networking :: DevOps & CI/CD 
- CI/CD is a continuous process or loop that includes steps of the software development process, to enable development teams to make changes, test their code, and deploy the application more reliably
- CI: Continuous Integration is the process in which software source code is kept in a central repository (GitHub)
- CD: Continuous Delivery is the continuous, automatic code deployment to test, pre-production, or production environments

- Aim : uncover weaknesses in the automation process
- Risks
  - access security : The increasing number of integration points can make access management difficult and also open a path for malicious activity
  - permissions : Components are connected with each other and perform their tasks with user accounts, so perms should be checked
  - keys and secrets : keys(API keys, ID keys) should be secured
  - user security : user with source code access can be attacked to insert malicious code
  - default config : if deafult configs like default passwords arnt changed, that could cause a blunder
  - A locally installed Jenkins application can have an unpatched component deployed for various operational reasons

## Day 15 :: Cyber Careers

## Day 16 :: OSINT
### OSINT or Open Source Intelligence, is obtaining info from free and public sources
  - Information is the core of OSINT
    - Sources of info:
      1. Clearnet : anything public like facebook, twitter, github; widely used for voluminous data available
      2. Darknet : accessed using special software and additional config, like TOR, Freenet, I2P, IPFS, Zeronet
  - Information originates from digital footprint. We look for left behind data to proceed towards the target
  - RIS OSINT Model :
    - this data-info model approaches gathering info to identify and categorize data
    - ![image](https://user-images.githubusercontent.com/75432450/154808748-2893951d-baea-4936-922d-ebceafb49f98.png)
  - RIS OSINT Roller Coaster represents phases of OSINT investigation about client
    - this is an ever evolving process, that improves acc to feedback(intelligence and decisions) from previous model
    - ![image](https://user-images.githubusercontent.com/75432450/154808855-35a5e956-950f-4878-8c80-079cf77a95a1.png)
    - insights of the stages of RIS OSINT web
      - ![image](https://user-images.githubusercontent.com/75432450/154808902-2e7178b2-fc8b-419d-a856-749c5d1e039f.png)
### Account Discovery and Analysis
- Accounts are a great source of info and digital footprints
- analysing target account 
  - ![image](https://user-images.githubusercontent.com/75432450/154809169-4ff0ba82-852c-4605-b412-0de8d704de27.png)

### Google Dorking
- enumerating sources using `google dorks`, a syntax to granulate the searches
- some key terms (complete list [https://gist.github.com/sundowndev/283efaddbcf896ab405488330d1bbc06])
  - ![image](https://user-images.githubusercontent.com/75432450/154809283-cfd6fbdc-3c2f-4227-9f92-4a5dadec38a8.png)

### OSINT and Blockchain
- core principle of blockchain tech and decentralization is 'anonymity'; if a target is anonymous, then how info can be gathered?
- since, blockchain is open yet anonymous, indentifying can be done quickly, but linking them is difficult
- tools aiding in blockchain exploration : blocktrail, bitcoing who's who, graphsense, block explorer
- same methodolody is applied to web3, goal is to get back to a traditional persona

### OSINT and further..
- at core, OSINT is searching and identifying info; but an understanding of the platform functionality is required
  - for ex GitHub, version control if not properly sanitized, could reveal critical info like editing/correcting accidental API key, passwords etc, could be accessed thru logs

## Day 17 Cloud :: Shadow IT
- compromised attention on security, due to hasty completion, results in systems not built on IT security standards
- public could is one way to get into shadow IT; most accessible public could is AWS

### Amazon Web Services (AWS)
  - Amazon AWS is public cloud service provider, two services discussed here
    - Amazon S3 : simple storage service && AWS IAM : identity and access management
      - AWS infra is divided into regions, independent clusters of datacenters; each region has an availability zone (AZ), each having a separate power grid, located in flood plains
      - S3 is the publicly hosted object storage service, objs are stored in buckets using a global namespace
      - identifying and listing the contents of a bucket is easy for buckets with public info; after parsing the data, it can be put to use
      - Authentication levels
        ![image](https://user-images.githubusercontent.com/75432450/154815725-2dbc7337-6904-4ec0-99b0-314ea7470414.png)
    - AWS IAM : all requests to AWS must be signed using IAM access keys consisting of an Access key ID(AKIA... 20 chars) and Secret Access Key(40 chars)
      - short terms creds have username for AWS API as `ASIA%` and have an additional string Session Token

## Day 18 Cloud :: Docker Containers
  - Containers are a virtualization mechanism similar to Virtual Machines
    - a container image is composed of a number of layers
    - packaging an app in a container freezes the app and its dependencies into an image as a part of build process
    - build process is part of the Software Development Lifecycle (SDLC), where applications and their dependencies are packaged together and tested prior to distribution and use
    - running the container image will always result in the same configuration state as specified at build-time, built from a `Dockerfile` source file
      - Dockerfiles are a list of new-line separated instructions that instruct the Docker daemon how to generate a container image
  - Docker
    - Docker API : local comm interface on a configured Linux machine to communicate with a Docker Daemon
    - Docker Daemon : a process that runs on the machine, to interact with container components
    - Docker Container Image Format : ultimately a `.tar` file
  - AWS Elastic Container Registry (ECR) - an online registry for public and private container images

## Day 19 Blue Teaming :: Phishing
  - When emails are sent to a target(s) purporting to be from a trusted entity to lure individuals into providing sensitive information
  - Spearphishing - targetted phishing, targetting an individual/org/industry/co
  - Mail analysis for phishing
    - is sender known?
    - does mail ID matches the sender?
    - does reply-to mail matches the sender?
    - is greeting msg, personal or general?
    - misspelt words and incorrect grammar
    - sense of urgency, act `NOW`
    - included link/button, redirecting to a website, does that website matches?
    - attachments

## Day 20 Blue Teaming :: File Analysis
  - `file` - file type regardless of extension
  - `string` - extracts and prints the printable char sequences; can disclose IP addresses, URLs, commands like ssh, session, message, etc.
  - further analysis using `VIRUSTOTAL`; scans the uploaded file/URL/IP for diff scan results of 60+ antiviruses; giving out details on relations with redirecting malicious websites/behaviors/community feedbacks

## Day 21 Blue Teaming :: YARA 

