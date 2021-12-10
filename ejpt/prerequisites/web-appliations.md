---
description: >-
  The web app world is extremely heterogeneous. Every web application is
  different from others because developers have many ways to accomplish the same
  task.
---

# Web Applications

* Having flexibility in web app development also means having flexibility in creating insecure code.
* Fundamental aspects: HTTP protocol basics, Cookies, Sessions, Same Origin Policy.

## HTTP Protocol Basics

HTTP works on top of TCP protocol, so when the connection is established, the client sends a request and waits for the answer. The server processes the request and sends back its answer, along with status code and data:

* Client -> HTTP request -> Server
* Client <- HTTP response <- Server

HTTP Connection establishment:

* Client -> SYN -> Server
* Client <- SYN/ACK <- Server
* Client -> ACK + GET /html -> Server
* Client <- HTML response + Close connection <- Server

The format of an HTTP message is:

```
Headers\r\n
\r\n
Message Body\r\n
```

where:

```
\r # carriage return
\n newline
```

Verbs: PUT, TRACE, HEAD, POST.

Some status codes:

| Status code               | Meaning                                                        |
| ------------------------- | -------------------------------------------------------------- |
| 200 OK                    | the resource is found                                          |
| 301 Moved Permanently     | the requested resource has been assigned a new permanent URI   |
| 302 Found                 | the resource is temporarily under another URI                  |
| 403 Forbidden             | the client doesn't have enough privileges, server refuses req. |
| 404 Not Found             | the server cannot find the resource matching the request       |
| 500 Internal Server Error | the server does not support the functionality required         |

{% tabs %}
{% tab title="HTTP Request Example" %}
```javascript
GET / HTTP/1.1                                    # VERB path protocol version
Host: www.elarnsecurity.com                       # Specifies the internet hostname and port number, obtained from the URI of the resource
User-Agent: Mozilla/5.0 (X11; Linux x86_64 ...)   # Tells the server what client software is issuing the request
Accept: text/html                                 # Specifies which document type is expected in the response
Accept-Language: en-US,en;q=0.5                   # Browser can as for a specific human language in the response
Accept-Encoding: gzip, deflate                    # Restricts the content encoding
Connection: keep-alive                            # Future communications with the server will reuse the current connection
\r\n\r\n
< PAGE CONTENT > ...
```
{% endtab %}

{% tab title="HTTP Response Example" %}
```javascript
HTTP/1.1 200 OK                                   # Status-Line: protocol version + status code + textual meaning
Date: Wed, 19 Nov 2020 10:10:10 GNT               # Time at which the message was originated
Cache-Control: private, max-age=0                 # Using cached content saves bandwidth
Content-Type: text/html; charset=UTF=8            # Lets the client know how to interpret the body of the message
Content-Encoding: gzip                            # The message body is compressed with gzip
Server: Apache/2.2.15 (CentOS)                    # (optional) header of the server that generated the content
Content-Length: 99043                             # length in bytes of the message body


< PAGE CONTENT > ...
```
{% endtab %}
{% endtabs %}

### HTTPS: HTTP over SSL/TLS as an encryption layer

HTTPS: _HTTP over SSL/TLS_ is a method to run HTTP which is a clear-text protocol over SSL/TLS, a cryptographic protocol:

* Provides: Confidentiality, Integrity Protection and Authentication to the HTTP protocol.
* An attacker cannot neither sniff the application layer communication nor alter the application layer data.
* Traffic can be sniffed, but any adjacent user will not know request/response headers, request/response body, request target domain.
* The client can tell the _real identity of the server_ and, sometimes, vice-versa.
* When inspecting HTTPS, one cannot know what domain is contacted and what data is exchanged.
* HTTPS does not protect against web application flaws.
* All the attacks against an application happen regardless of SSL/TLS, such as XSS and SQL injection will still work.

### Tools

`netcat` opens a raw connection to a service port:

```bash
$ nc -v www.ferrari.com 80

...
GET / HTTP/1.1
Host: www.ferrari.com # 2 lines after inputting this line

...
HEAD / HTTP/1.1
Host: www.ferrari.com

...
HEAD /en_en/ HTTP/1.1
Host: www.ferrari.com
```

```bash
$ nc -v hack.me 443

GET / HTTP/1.1
Host: hack.me

# Nothing is displayed as netcat cannot open SSL websites
```

```bash
$ openssl s_client -connect hack.me:443
$ openssl s_client -connect hack.me:443 -debug
$ openssl s_client -connect hack.me:443 -state
$ openssl s_client -connect hack.me:443 -quiet

# Certificate is spat out

GET / HTTP/1.1
Host: hack.me


OPTIONS / HTTP/1.1
Host: hack.me
```

`Burp Suite` repeater tool also allows same operation in order to inspect raw response given a HTTP verb. It can also configure a HTTPS connection right away.

## HTTP Cookies (1994 - Netscape)

* HTTP is a stateless protocol, therefore HTTP cannot keep the state of a visit across different HTTP requests.
* HTTP requests are unrelated to the preceding and following ones.
* Often exploits rely on stealing cookies: Cookies / Cookie jar, just textual information installed by a website into a web browser.

A server can set a cookie via `Set-Cookie` HTTP header field in a response message:

```
HTTP/1.1 200 OK
Date: Wed, 19 Nov 2020 10:10:10 GNT
Cache-Control: private, max-age=0
Content-Type: text/html; charset=UTF=8
Content-Encoding: gzip
Server: Apache/2.2.15 (CentOS)
Set-Cookie: ID=Value; expires=Thu, 21-May-2015 15:25:20 GMT; path=/; domain=.example.site; Http
Content-Length: 99043


< PAGE CONTENT > ...
```

### **Cookie format**

A cookie contains the following attributes: the actual content, an expiration date, path, domain and optional flags (Http only flag, Secure flag).

| Field                  | Value                                  |
| ---------------------- | -------------------------------------- |
| Cookie Content         | ID=Value;                              |
| Expiration Date        | expires=Thu, 21-May-2015 15:25:20 GMT; |
| Path                   | path=/;                                |
| Domain                 | domain=.example.site                   |
| Flag-setting Attribute | HttpOnly                               |

* Browsers use domain, path, expires and flags attributes to choose whether or not to send a cookie in request.
* Cookies are sent only to the valid domain/path when they are not expired and according to their flags.
* The domain field and the path field set the scope of the cookie.
* The browser sends the cookie only if the request is for the right domain.
* When a web server installs a cookie, it sets the domain field.
* Then the browser will use the cookie for every request sent to that domain and _all its subdomains_.
* If the server does not specify the domain attribute, the browser will automatically set the domain as the server domain and set the cookie 'host-only' flag,meaning that this cookie will be sent only to that precise hostname.
* Respectively, when a cookie has the path attribute set, the browser will send the cookie to the right domain and to the resources in that path _and not any other_.
* A browser will not send an expired cookie to the server, session cookies will expire with the HTTP session.
* `http-only` flag is a mechanism that prevents JavaScript or any other non-HTML technology from reading the cookie (preventing a XSS robbery).
* `Secure` flag creates secure cookies that will only be sent over an HTTPS connection.

## Sessions

Sessions are a mechanism that lets the website store variables specific for a given visit on the server side:

* Sometimes the web developer prefers to store some information on the server side.
* This avoids the back and forth data transmission and hides the application logic.
* Each session is identified by a session id, where the client presents this ID for each subsequent request.
* With that ID, the server is able to retrieve the state of the client.

### Session cookies

Session cookies allow to install a session ID on a web browser

```bash
SESSION=0mwerj234w
PHPSESSID=1992maiwr2H     # PHP
JSESSIONID=W8234mSfsw3    # JSP
```

* The browser then uses the cookie in subsequent requests.
* A session could contain multiple variables, so sending a small cookie keeps the bandwidth usage low.
* The browser will send back the cookie according to the cookie protocol,thus sending the session ID.
* Session IDs can also be transmitted via GET requests.

Session cookies and Cookies can be inspected and manipulated via Firebug for Firefox or any other web developer tools. Your session will be forwarded to re-authenticate if you delete the session cookie after login process.

```javascript
// If HttpOnly isn't enabled, we'd be able to access the cookie jar from JavaScript
document.cookie
```

## Same Origin Policy (SOP)

* SOP / Same Origin Policy is a critical point of web application security.
* Prevents JavaScript code from getting/setting properties on a resource coming from a different origin.
* To determine if JS can access a resource, hostname, port and protocol _must match_.
* SOP only applies to the actual code of a script.
* It is still possible to include external resources by using HTML, like IMG, script, iFrame...
* If a script on Domain A was able to read content on Domain B, it would be possible to steal clients' information and mount a number of dangerous attacks.

## Burp Suite

* Any web application contains many objects like scripts, images, stylesheets, client and server-side intelligence.
* Having tools that help in the study and analysis of web application behavior is critical.
* An _intercepting proxy_ is a tool that lets you analyze and modify any request and any response exchanged between an HTTP client and a server.
* Intercepting proxy != web proxy (as Squid)

BurpSuite will let you:

* Intercept request/responses between your browser and web server.
* Build requests manually.
* Crawl a website by automatically visiting every page in a website.
* Fuzz webapps by sending them patterns of a valid and invalid inputs to test their behavior.
* You can modify the header and the body of a message by hand or automatically.
* Burp Repeater lets you manually build raw HTTP requests.
* Same can be achieved with `nc` or `telnet`.

Options:

* Proxy: let's you view, intercept and modify traffic between browser/
* Spider: web crawler.
* Repeater: manipulate and reissue HTTP requests.
* Learn how to find hidden resources on web application.
* Learn how to configure your browser's proxy to work with Burp Suite, apart of setting 'scope'.
