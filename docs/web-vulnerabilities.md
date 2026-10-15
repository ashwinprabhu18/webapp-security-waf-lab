# Web Vulnerabilities
##  DOM XSS
I started by searching for owasp in the search bar and noticed the URL changed to /#/search?q=owasp

In inspector, I found the search iem inside span#searchValue. I tried <b>ashwin-test</b> and saw an actual bold element appear in the DOM, so the input was being interpreted as HTML.

Then I tried <iframe src="javascript:alert(`xss`)"> in the search bar, and an alert popped up. 

This confirms that I could get js to execute through the search input in my local juice shop lab.

Also completed the bonus DOM challenge 😂. `<iframe width="100%" height="166" scrolling="no" frameborder="no" allow="autoplay" src="https://w.soundcloud.com/player/?url=https%3A//api.soundcloud.com/tracks/771984076&color=%23ff5500&auto_play=true&hide_related=false&show_comments=true&show_user=true&show_reposts=false&show_teaser=true"></iframe>`

Plays OWASP Juice Shop Jingle