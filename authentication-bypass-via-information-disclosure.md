Lab: Authentication Bypass via Information Disclosure

## Description
This lab demonstrates an authentication bypass vulnerability caused by information disclosure. The front-end leaks the custom HTTP header used to authorize admin access. By discovering and spoofing this header, an attacker can bypass authentication and perform privileged actions.

## Steps to Reproduce

1. **Initial request to `/admin`:**
   - Send a `GET /admin` request using Burp Repeater.
   - The response reveals access is only allowed for admins or from a local IP.

2. **Use `TRACE` method to reveal headers:**
   - Send a `TRACE /admin` request.
   - Observe the `X-Custom-IP-Authorization` header revealed in the response.

3. **Modify request to spoof header:**
   - Go to **Proxy > Match and Replace** in Burp Suite.
   - Add a new rule:
     - Leave **Match** empty.
     - **Type**: Request header.
     - **Replace**: `X-Custom-IP-Authorization: 127.0.0.1`

4. **Send authenticated request:**
   - Send a new `GET /admin` request with the spoofed header.
   - You are now authenticated as an admin.

5. **Complete the lab:**
   - Access the admin interface and delete the user `carlos`.

## Payload / Header
```
X-Custom-IP-Authorization: 127.0.0.1
```

## Mitigation
- Avoid relying solely on custom headers or IP-based restrictions for authentication.
- Use proper session-based authentication and authorization controls.
- Disable the HTTP `TRACE` method on production servers.

## Tags
Authentication Bypass, Information Disclosure, HTTP Headers, Burp Suite
