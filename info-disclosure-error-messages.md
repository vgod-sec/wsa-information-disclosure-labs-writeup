# Information Disclosure in Error Messages – Lab Write-up

## Overview
This lab illustrates how verbose error handling can expose sensitive internal details. By forcing the application to throw an exception, we extract the version of its underlying framework and use this information to complete the challenge.

## Goal
Obtain the exact version number of the third-party framework revealed in a server-side stack trace and submit it.

## Vulnerability
- **Type:** Information disclosure through verbose error messages  
- **Impact:** Reveals framework name and version (Apache Struts 2.3.31), enabling targeted exploitation of known vulnerabilities

## Environment & Tools
- **Browser** with traffic routed through **Burp Suite Community/Professional**
  - *Proxy* tab for interception
  - *HTTP History* to review requests
  - *Repeater* to modify and resend requests

## Exploitation Steps
1. **Intercept a product request**  
   - Browse to any product page while Burp’s proxy is active.  
   - Locate the corresponding `GET /product?productId=<id>` entry in **Proxy → HTTP History**.

2. **Send to Repeater**  
   - Right-click the request and select *Send to Repeater*.

3. **Manipulate the parameter**  
   - In Repeater, replace the numeric `productId` with a non-numeric string to cause a type mismatch, e.g.  
     ```
     GET /product?productId=example
     ```

4. **Trigger the exception**  
   - Click **Send**. The server returns an HTTP 500 response containing a full Java stack trace.

5. **Extract the framework version**  
   - Inspect the stack trace for lines such as:  
     ```
     org.apache.struts2… version 2.3.31
     ```
   - Note **Apache Struts 2.3.31**.

6. **Submit the version**  
   - Return to the lab interface, choose *Submit solution*, and enter `2.3.31` to solve the lab.

## Root Cause
The application propagates unhandled exceptions directly to the client, exposing full debug information and software versions. Attackers can map these details to public exploits.

## Mitigations
- Disable verbose error output in production; configure generic user-facing error pages.
- Log detailed errors server-side only, protected from public access.
- Sanitize and validate all input parameters to prevent runtime type errors.

## Conclusion
Verbose error messages significantly increase an application’s attack surface by revealing internal technologies and versions. Proper error handling and logging practices are critical to limit the information available to adversaries.
