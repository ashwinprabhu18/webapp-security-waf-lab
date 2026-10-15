# Web Application Security Lab

I'm using this project to learn web application penetration testing and
how defensive controls like a Web Application Firewall (WAF) work.

The idea is to test an intentionally vulnerable application, understand
what happens during each test, and then see how the results change
when security controls are introduced.

## Lab setup

- OS: Linux
- Docker / Docker Compose
- OWASP Juice Shop
- Burp Suite Community (planned)
- SafeLine WAF (planned)

Juice Shop is running locally at http://127.0.0.1:3000.

## What I've done so far

- Set up Juice Shop using Docker Compose.
- Confirmed that the application loads in the browser.
- Checked the Docker port mapping to make sure it is bound to localhost.
- Started looking at HTTP response headers using curl.
- Tested how the application responds to a supplied Origin header.

## What I'm working on next

- Learn to inspect HTTP traffic using browser DevTools and Burp Suite.
- Explore the application's API and understand its normal behaviour.
- Work through selected web security vulnerabilities.
- Set up SafeLine in front of Juice Shop.
- Repeat the tests and compare the results.
- Document findings and possible fixes.

## Notes

This is a learning project, so I'll update the documentation as I go.
I'll include commands, screenshots, observations and test results rather
than just documenting the final setup.

All testing is intended for my own local lab.
