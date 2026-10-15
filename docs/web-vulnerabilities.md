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