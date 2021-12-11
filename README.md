# AoC-3-2021

## Day 1 :: IDOR
Insecure Direct Object Reference, IDOR, server returns query results without authenticating user identity. 
- Usually abused by query info appended to the URL to send request to the server. 
`https://domain/page?query[profile?id=31]`
- Examining form details sometimes reveal vulnerable details. `updated HTML of a form after receiving info from user`
- change in cookie characters can result in leading to someone else's profile

## Day 2 :: Cookie Manipulation
HTTP is a stateless protocol. At every request, server cant distinguish btw user1 and user2. Cookie acts as a proof of identity of both to establish a stateful session.<br>
- Schematic of a HTTP request
  - POST query
  - Server response. returns a `200 OK` if successful.
  - cookie set
  - GET query
  - Server response.
  - authentication with cookie 
  - if successful, locating and de-serializing of session.
  - if successful, `200 OK` reply
<br>
<b>De-serializing</b> is fetching data from say JSON format and changing its type to Object to prep it to be abused.
<br>

- Cookie components are key-value pairs. tbf, `name-value` others are `attribute-value`. <br>
- `Set-Cookie` example ```Set-Cookie: <cookie-name>=<cookie-value>; Domain=<domain-value>; Secure; HttpOnly```
#### Cookie Manipulation
1. fetch a cookie
2. decode its value
3. identify the scheme and abuse
4. re-encode and replace with og value
5. refresh...voila!

## Day 3 :: Content Discovery
