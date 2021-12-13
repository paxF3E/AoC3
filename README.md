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
#### Cookie Manipulation
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
