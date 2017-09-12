# Types of hackers

1. Black Hat - malicious, releases 0day bugs
1. Grey Hat - curious, explore vulnerabilities
1. White Hat - gets permission, penetration testers, responsible disclosure

# M.O. of an attacker
- Gather info about your system
- Research site vulnerabilities
- Get a foothold in the system
- Plan for more serious attacks, could be a chain of events later

# Types of attacks

## Cross-Site Scripting XSS
- An injection attack
- Older browsers are especially vulnerable to this attack
- Allow attacker to read data, perform opeartions on users' behalf, change databases

### 3 + 1 categories of XSS
1. Stored XSS: Stored code that executes attacker's script
2. Reflected XSS: A server response (e.g. validation error of username)
3. DOM based XSS: Exploits via queryParams
4. Blind XSS: Exploits vulnerability in another app (e.g. log reader, other internal softwares e.g. HR, accounting, CRM) that an attacker can't see or access under normal means

### XSS Danger zones
- User generated rich text, e.g. comments
- Embedded content, e.g. iFrame, flash plugin
- Any input field where users have control over a URL, e.g. upload assets
- Anywhere user input is reflected (e.g. can't find ______ (search, username))
- Query params rendered into DOM
- element.innerHTML =?

### NEVER trust these data!
- Directly in a script `<script><%= userData %></script>`
- In a HTML comment `<!-- <%= userData %>-->`
- In an attribute name `<iframe <%=userData%>="myVal" ></iframe>`
- In a tag name `<<%= userData%> class="myElement" >`
- Directly in <style> block `<style> <%=userData%></style>`

### XSS Defenses
- Sanitize data BEFORE rendering & persisting
Github repo: [ESAPI4JS (Enterprise Security API for JavaScript) encoder](https://github.com/ESAPI/node-esapi).

- User input should always be treated as **VALUE**, not as code!
- Content Security Policy ([CSP](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP)): tell modern browsers which sources / origins they should trust, and for what types of resources
    - by default, directives are permissive
    - Multiple directives are separated by semicolon ;
- Generate cryptographic nonces per page load, changed unpredictably

### XSS Defence
1. Be restrictive on file upload types and ability to access those types
2. Compress files to drop non-visible data
3. Before allowing attatchment types, research capabilities thoroughly (esp. PDFs!)

## Cross Site Request Forgery (CSRF)
- Takes advantage of cookies/ basic Authentification credential, that are passed along requests to authenticate & authorize user

### Defense
- Use a 'Client side' cookie
- Using cookies doesn't require the ability to READ cookies (only avail in the domain that receives the cookie)
- localStorage / sessionStorage
- Use a CSRF token
- Validate request origin
- Set your Cross-Origin Resource Sharing (CORS) option correctly. 
    - A preflight OPTIONS request giving server a chance to indicate what's allowed
    - Main request will follow

### CSRF tokens
- changes with each request and are generated randomly
- Similar to 2-factor authentification:
    1. something proving you are authenticatd
    2. something proving you are sending a request from authorized origins
- For server rendered apps: meta tags are fine

### Validate Request Origin
- Modern browsers send an Origin header

## ClickJacking
- A UI redress attack
- Can be used to capture keystrokes

### Defence
- Incl. `X-Frame-Options` in HTTP reponse header
- Use `frame-ancestors` CSP directive

## 3rd Party Assets
- If files are hosted by external vendors, your security is at risk of their business being compromised
- All businesses are managed by people! People who write your dependencies make mistakes!

### Defence
- Use LTS versions
- Support BUG Bounties in open source projects
- Use Subresource Integiry attributes on <script> and <link> tags
- Ask vendors to VERSION scripts

