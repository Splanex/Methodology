https://appsecexplained.gitbook.io/appsecexplained/enumeration/methodology
## Burp extensions
**Must have**
- Active Scan ++
- Backslash powered scanner
- Param miner
- Taborator

**Nice to have**
- Turbo intruder
- Autorize
- Software vulnerability scanner
- Collaborator everywhere

**Honorable Mentions**
- Freddy
- NTLM Challenge Decoder
- GraphQL raider
- Retire.js
- JSON Web Tokens
- Additional Scanner Checks

## Methodology
	Contect discovery
	Fuzzing
	Hypothesis & test cases
	Business process and logic flaws
	Frameworks
	Tools

### Content discovert: Find everything
	I. Map browsable attack surface
	II. Find unlinked content & params
	III. Repeat for each 'Platform indistinctions' of the application
### Platform Distinctions
A web application may have several platform distinctions
- Load balancers may balance on an endpoint
- Reverse proxies does the same

 Do your best if the target is split into different platforms
 - Each platform distinction should receive a full test process

### Map browsable attack surface
- Browse the entire application
- Use Burps suite's Crawl feature on the top level of the application
- For JavaScript, extract file paths and references

### Find unlinked content
Fuzz verbs and functionalities
- e.g. /?action=showUser&id=123
	action, show and user and id can be fuzzed
Use and create wordlists based on target functionality

### Unlinked Parameters
Discover if there are any unlinked parameters
Headers might bypass authentications
Might find attack surface
Parameter miner extension

### Archive.org
- WaybackRobots.py
- WaybackURLS.py

### Fuzzing
### Fuzzing bytes 101
1. For-each script and input
2. Send their script to repeater / play with it in browser
3. Send to intruder and fuzz
	- %00 to %FF
		- URL decode target middleware
		- URL encode target app
	- Anomalies, discrepancies, interesting results?
		- Create hypothesis
		- Work with team if you cannot produce hypothesis
	- Use wordlists
4. Utilize vulnerabily scanner
	- Backslash powered scanner and other extensions will also aid here
5. Scanner results? Update methodology

### Use wordlists
Without our fuzzing efforts, wordlists can help produce valuable results, e.g. anomalies in cases of different results, timing impacted or external server interaction

Use wordlists that help you target technology and hypothesis.
- Seclists
- AssetNote

### Use Collaborator with placeholders
- Many wordlists rely on external server interactions
- Brup suite has a built in external interaction monitor
- Taborator plugin makes for quick access to colllaborator
- Or use interactsh (https://github.com/projectdiscovery/interactsh)

### Build custom wordlists
- Cewl
- Cewler

### Scan with plugins and web app

### Review logger 500 errors

### Hypothesis & Test cases

1. Does the application allow me to reset someone else's password? Redirect password reset email? Password reset email not high enough entropy? Predictable values?
2. Does the input have likelihood of being reflected somewhere else, indicating XSS should be tested more in depth.
3. Is there any delays when doing certain actions, indicating things such as server-side lookups, processes being run or something else?
4. Could there be a state machine which can be influenced by race conditions?
### Utilize the tean
If incapable of formulating any hypothesism, work with the team to build one.

### Business process and logic flaws
As testing is typically done from a technical perspective, it is imperative to also look at the application from a logical and business perspective.
We want to produce interesting test cases, for example:
- Can I break into onother tenant of the application if I cleverly utilize the application against itself?
- Will the process flow break if I jump forwards or backwards in the process, perhaps unintended by the application?
- Will files be uploaded and perhaps executed or used somewhere else?

### Map out application flows

### Frameworks
### Minimum viable penetration testing
Define an absolute minimum of activity to platform on a certain system or piece of technology or application
- Allow experience from previous tests to be used
- A way to support pentesters. Don't start from scratch
	- Hacktricks
- Not training on concepts, but simple bullets of what needs to be done
- Make pentester accountable for:
	- Checking the things that need to be checked
	- Asking team for help when there are interesting anomalies
- There are applications and technologies specific MVP's


### Tools
Technology specific tools
- IIS Short Name Scanning
- WPscan
- SQLmap
- Kiterunner
- Nuclei
- Nikto
- OpenVAS
- Magescan
- Jwt_tool
- Trufflehog