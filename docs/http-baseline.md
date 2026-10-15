# HTTP baseline notes

## First request

Command:

    curl -sS -D - -o /dev/null http://127.0.0.1:3000/

Result: HTTP 200 OK

Some headers returned:

- Access-Control-Allow-Origin: *
- X-Content-Type-Options: nosniff
- X-Frame-Options: SAMEORIGIN
- Content-Type: text/html; charset=UTF-8
- X-Recruiting: /#/jobs

The root page returned successfully. The response includes a wildcard 
CORS header, as well as headers related to content handling and framing.

## CORS check

Command:

    curl -sS -D - -o /dev/null \
      -H 'Origin: https://example.invalid' \
      http://127.0.0.1:3000/

Result:
- Status: 200 OK
- Access-Control-Allow-Origin: *

The wildcard header was still present when I supplied a different
Origin header.

This doesn't prove that sensitive data is exposed or that there is
a CORS vulnerability. I need to understand how the API behaves before
drawing any conclusions.

## Next

- Inspect requests in browser DevTools.
- Identify an API endpoint.
- Compare its normal response with its response when an Origin header
  is supplied.
- Record anything interesting and investigate it further.