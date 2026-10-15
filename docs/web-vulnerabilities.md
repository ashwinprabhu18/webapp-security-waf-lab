# Web Vulnerabilities
##  DOM XSS
I started by searching for owasp in the search bar and noticed the URL changed to /#/search?q=owasp

In inspector, I found the search iem inside span#searchValue. I tried <b>ashwin-test</b> and saw an actual bold element appear in the DOM, so the input was being interpreted as HTML.

Then I tried <iframe src="javascript:alert(`xss`)"> in the search bar, and an alert popped up. 

This confirms that I could get js to execute through the search input in my local juice shop lab.