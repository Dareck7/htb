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

1- The user-agent (the browser / the HTTP client) will send a GET request to the HTTP Server with the URL of the resource we requested
2- The HTTP server will look up the resource in the specified location (the given URL)
3- If the resource or directory exists, we will receive the HTTP Server response containing the data we requested (be it a webpage, an image, an audio file, a script, etc.) and response code 200 OK , because the resource was found and the request was fulfilled with success.
4- If the resource or directory cannot be found at the specified address, and there is no redirection implemented for it by the server administrator, the HTTP Server response will contain the typical 404 Page with the response code 400 Not Found attached.
```

The same attack type comes into play for password brute-forcing - submitting passwords from a wordlist
until we find the right one for the specified username. This method is prevalent for low-skilled attackers due
to its low complexity, with the downside of being "noisy", meaning that it involves sending a large number of
requests every second, so much that it becomes easily detectable by perimeter security devices that are
fine-tuned to listen for non-human interactions with log-in forms.

```
The pair of single quotes are used to specify the exact data that needs to be retrieved from the SQL Database, while the hashtag symbol is used to make comments. 
Therefore, we could manipulate the query command by inputting the following:

Username: admin'#

We will close the query with that single quote, allowing the script to search for the admin username. 
By adding the hashtag, we will comment out the rest of the query, which will make searching for a matching password for the specified username obsolete. 
The code will only approve the log-in once there is precisely one result of our username and password combination. 
However, since we have skipped the password search part of our query, the script will now only search if any entry exists with the username admin. 
There is indeed an account called admin, which will validate our SQL Injection and return the 1 value for the $count variable, which will be put through the if statement, allowing us to log-in without knowing the password. 
If there was no admin account, we could try any other accounts until we found one that existed. (administrator, root, john_doe, etc.) Any valid, existing username would make our SQL Injection work.
In this case, because the password-search part of the query has been skipped, we can throw anything we want at the password field, and it will not matter.
```
To be more precise, here is how the query part of the PHP code gets affected by our input.

<img src="img/Screenshot from 2022-10-29 14-29-46.png">

Notice how following our input, we have commented out the password check section of the query? 
This will result in the PHP script returning the value 1 (1 row found) for username = 'admin' without checking the password field to match that entry. 
This is due to a lack of input validation in the PHP code.

<img src="img/Screenshot from 2022-10-29 14-31-30.png">
