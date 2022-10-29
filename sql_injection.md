### SQL Injection

```diff
PII : Personally Identifiable Information 
SQL : Structured Query Language
HTTP stands for Hypertext Transfer Protocol, and it is an application-layer protocol used for transmitting hypermedia documents, such as HTML (Hypertext Markup Language).

What does the OWASP Top 10 list name the classification for this vulnerability? 
- A03:2021-Injection 

HHTP TCP/80
HTTPS TCP/443
- Alternatively on HTTP ports such as 8080 TCP or 8000 TCP.  

What is a folder called in web-application terminology? 
- directory
What response code is given for "Not Found" errors? 
- 404

Post-log-in, the web application will set the user a special permission in the form of a
cookie or authentication token that associates his online presence with his authenticated presence on the
website. This cookie is stored both locally, on the user's browser storage, and the webserver.

SQL Injection is a common way of exploiting web pages that use SQL Statements that retrieve and store
user input data. If configured incorrectly, one can use this attack to exploit the well-known SQL Injection
vulnerability. 
There are many different techniques of protecting from SQL injections, some of them being input validation, parameterized queries, stored procedures, and
implementing a WAF (Web Application Firewall) on the perimeter of the server's network.
``` 

```
The HTTP client, which is your browser, communicates with the HTTP server (in this case Apache 2.4.38) using the HTTP protocol by sending an HTTP Request (a GET or POST message) which the server will then process and return with an HTTP Response.

HTTP Responses contain status codes, which detail the interaction status between the client's request and
how the server handled it. Some of the more common status codes for the HTTP protocol are:

- HTTP1/1 200 OK : Page/resource exists, proceeds with sending you the data.
- HTTP1/1 404 Not Found : Page/resource does not exist.
- HTTP1/1 302 Found : Page/resource found, but by redirection to another directory (moved temporarily). This is an invitation to the user-agent (the web browser) to make a second, identical request to the new URL specified in the location field. You will perceive the whole process as a seamless redirection to the new URL of the specified resource.


```
