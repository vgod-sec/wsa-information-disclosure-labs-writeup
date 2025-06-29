Lab: Source Code Disclosure via Backup Files

## Description
This lab demonstrates how source code can be unintentionally exposed through backup files stored in publicly accessible directories. The goal is to retrieve a hard-coded database password from a leaked `.bak` file.

## Steps to Reproduce

1. **Discover backup directory:**
   - Navigate to `/robots.txt`
   - Observe a disallowed entry: `/backup`

2. **Explore the backup directory:**
   - Go to `/backup/`
   - Locate and access `ProductTemplate.java.bak`

3. **Analyze the leaked source code:**
   - Review the `.bak` file and locate a hard-coded password inside the database connection string

4. **Submit the solution:**
   - Copy the exposed database password
   - Return to the main lab page and submit the password to solve the challenge

## Payload / Endpoint
```
GET /backup/ProductTemplate.java.bak
```

## Mitigation
- Never store sensitive data like credentials in source code
- Avoid exposing backup files publicly
- Use `.gitignore` and server-side access restrictions to prevent exposure
- Regularly audit accessible files and directories on production servers

## Tags
Information Disclosure, Backup Files, Source Code Leak, Security Misconfiguration
