# Web Vulnerabilities
##  DOM XSS
I started by searching for owasp in the search bar and noticed the URL changed to /#/search?q=owasp

In inspector, I found the search iem inside span#searchValue. I tried `<b>ashwin-test</b>` and saw an actual bold element appear in the DOM, so the input was being interpreted as HTML.

Then I tried `<iframe src="javascript:alert(`xss`)">` in the search bar, and an alert popped up. 

This confirms that I could get js to execute through the search input in my local juice shop lab.

Also completed the bonus DOM challenge 😂. `<iframe width="100%" height="166" scrolling="no" frameborder="no" allow="autoplay" src="https://w.soundcloud.com/player/?url=https%3A//api.soundcloud.com/tracks/771984076&color=%23ff5500&auto_play=true&hide_related=false&show_comments=true&show_user=true&show_reposts=false&show_teaser=true"></iframe>`

Plays OWASP Juice Shop Jingle on soundcloud

## SQL Injection Investigation

While running the ZAP active scan, it flagged SQL injection on */rest/products/search* using the q parameter.

Normal search request works fine but ZAP's test input caused a SQLite error. The response was `"SQLITE_ERROR: near \"(\": syntax error"` and exposed the SQL query being executed.

This looks like a potential SQL Injection. As of right now I haven;t fully validated whether the input can change the query's logic or not but the database error and exposed query are clear evidence worth investigating.

**Possible Fix:** Use parameterized queries instead of building SQL with user input. Also don't return raw database errors or stack traces to the client.

**Evidence:**
- [SQL injection request](../evidence/screenshots/02-sql-injection-request.png)
- [SQL injection response](../evidence/screenshots/03-sql-injection-response.png)

## Login Endpoint

ZAP also flagged SQL injection on *POST /rest/user/login*.

The request body had a single quote in the email field and ZAP in the password field. This caused a SQLite Error which led to the SQL query exposed and stack trace.

This is another SQL injection possibility. Query and stack trace returned to the client is clearly information disclosure but haven't fully validated whether the input can change the query's logic or not.

**Possible fix:** Use parameterized queries for the login and return a generic error to teh client instead of raw database errors and stack traces.

### SQL Injection - Product Search

**Endpoint:** `GET /rest/products/search`

I tested the search functionality with normal input first, then tried some malformed inputs.

- Normal searches returned product results as expected.
- Adding a single quote caused a `500 Internal Server Error`.
- The response showed a SQLite error and exposed the SQL query.
- I tried comparing `1=1` and `1=2`, but both returned large product lists, so I couldn't confirm SQL injection with this test.

**Result:** The endpoint exposes database errors and SQL query details. SQL injection is still suspected, but I haven't confirmed it yet.

**Possible fix:** Use parameterised queries and avoid returning raw database errors to the user.

### SQL Injection - Login

**Endpoint:** `POST /rest/user/login`

I sent a login request with a single quote in the email field and an invalid password.

- The request triggered a SQLite database error.
- The response exposed the SQL query and a Sequelize stack trace.
- This shows that internal database and application details are being returned to the client.
- I haven't confirmed whether this can be used to bypass authentication.

**Result:** Database error disclosure confirmed. SQL injection is suspected but needs further validation.

**Possible fix:** Use parameterised queries and return a generic error instead of exposing database errors and stack traces.