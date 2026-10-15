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

## Socket.IO polling request

While inspecting the browser's Network tab, I found a GET request to `/socket.io/` using Engine.IO protocol version 4 and polling transport.

The response returned HTTP 200 OK.

Headers observed:
- Access-Control-Allow-Origin: http://localhost:4200
- Cache-Control: no-store
- Vary: Origin
- Content-Type: text/plain; charset=UTF-8

This appears to be part of the application's real-time communication. The explicit CORS origin differs from the wildcard observed on teh root page response.

## Socket.IO CORS Comparison
Endpoint: `socket.io/?EIO=4&transport=polling&t=knljesk9`
Method: GET

### TEST 1: Broswer request
- Status: 200 Ok
- Access-Control-Allow-Origin: http://localhost:4200
- Vary: Origin
- Cache-Control: no-store
  
### TEST 2: Supplied origin
Sent a request with: `Origin: https://example.invalid`

Result: 
- Status: 200 Ok
- Access-Control-Allow_origin remained `http://localhost:4200`

### Observation

The response did not reflect the supplied origin. This is consistent with a fixed allowed-origin configuration.

### Limitation 
The curl does not enforce browser CORS rules and does not establish whether sensitive data is accessible cross-origin. Further browser-base testing is needed.

## Socket.IO handshake
The polling response returned an Engine.IO open packet

Fields observed:
- `sid`: connection session identifier
- `upgrades`: Websocket upgrade advertised
- `pingInterval`: 25,000 ms
- `pingTimeout`: 20,000 ms
- `maxPayload`: 10,00,000 bytes

This proves that the application supports real time communication using Socket.IO/Engine.IO

The session identifier is transient and not included in the repo's notes. No vulnerability has been established from this particular handshake alone.

