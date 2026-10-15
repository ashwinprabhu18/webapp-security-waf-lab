# Web Application & WAF LAB
A hands-on cybersecurity project exploring web application penetration testing, Web application firewall (WF) protection, security monitoring and remediation validation.
## Objectives 
- Understand HTTP requests, responses, headers and cookies
- Assess vulnerabilities in an intentionally vulnerable web application.
- Practice web security with Burp and Caido
- Configure a web application firewall using safeline
- Compare application behaviour before and after WAF protection.
- Document findings, defensive controls and remediation

# Lab Environment
| Component | Purpose |
|---|---|
| OWASP Juice Shop | Intentionally vulnerable test application |
| Docker | Isolated application deployment |
| Burp Suite Community | HTTP traffic inspection and testing |
| SafeLine WAF | Web application protection |
| Linux | Lab host and command-line tools |
## Architecture

The initial application is deployed locally using Docker.

    Browser / Testing Tools
             |
             v
    127.0.0.1:3000
             |
             v
    OWASP Juice Shop

The initial configuration publishes the application on the device's loopback interface. WAF integration will be documented in a later milestone.

## Current Progress

- [x] Deploy OWASP Juice Shop using Docker Compose.
- [x] Verify that the application loads in a browser.
- [x] Confirm the published port is bound to 127.0.0.1.
- [ ] Establish an HTTP traffic baseline.
- [ ] Inspect requests and responses using Burp Suite.
- [ ] Perform controlled vulnerability assessments.
- [ ] Integrate SafeLine WAF.
- [ ] Compare detection and blocking results.
- [ ] Document findings and remediation.
- [ ] Retest vulnerabilities after applying mitigations.

## Methodology

Each assessment will follow this workflow:

1. Define the scope and testing hypothesis.
2. Capture normal application behavior.
3. Perform a controlled security test.
4. Record requests, responses, and relevant logs.
5. Assess impact and recommend remediation.
6. Repeat the test and document the result.

## Evidence and Reports

Screenshots, test evidence, and assessment reports will be
added as the project progresses. Findings will be documented
with reproduction steps, impact, remediation, and retest results.

## Status

In progress — initial local deployment completed.
